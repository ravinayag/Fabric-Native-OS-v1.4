# Hyperledger Fabric on Native OS – Version used hlf1.4 & Ubuntu 16.4
The purpose of the project to install  and run the Hyperledger fabric v1.4 on  Native  OS ( Ubuntu 16.04).   The Smart contracts written in GO lang and  used as GO Plugins. 
Docker and docker-compose are not used in this project either for fabric peers or chaincode.

### Lets get into task to dirt our hands. 
First you need to prepare your Native OS – Ubuntu 16.04 
Setting up the environment: 
## Prerequisites
Installing the Go language, Hyperledger Fabric depends upon the Go language.
* Go - version 1.11.9 
* Change into your home directory with the command cd ~/  
*	Download the tar file with the command wget [gotarfile](https://storage.googleapis.com/golang/)
*	Unpack the file with the command tar xvzf go1*.tar.gz 

######## Now we need to set GOPATH and GOROOT with the following commands: 
*	$ mkdir $HOME/gopath
*	$ export GOPATH=$HOME/gopath
*	$ export GOROOT=$HOME/go
*	$ export PATH=$PATH:$GOROOT/bin
*	$ export PATH=${PWD}/bin:${PWD}:$PATH
##### Install dependencies
Next, few dependencies to install. 
- $sudo apt install libltdl-dev python build-essential checkinstall libssl-dev
- $ mkdir -p $GOPATH/src/github.com/hyperledger/
- $ cd $GOPATH/src/github.com/hyperledger/

## Now Download the source code from [GitHub](https://github.com/hyperledger/fabric)
*	wget [sourcecodefile](https://github.com/hyperledger/fabric/archive/release-1.4.zip)
*	tar zxf hyperledger-fabric-linux-amd64-1.4.0.tar.gz
*	mv $DIR fabric
*	cd $GOPATH/src/github.com/hyperledger/fabric
*	make native

It will time to build the binaries and config files and back to prompt.

###### At this point your fabric  components are compiled and binaries are ready to run on Native OS (Ubuntu 16)

-	Im creating one more path to cutdown the length of the directories and files.
-	export FAB_BIN=$HOME/gopath/src/github.com/hyperledger/fabric/.build/bin
-	export FAB_CONF=$HOME/gopath/src/github.com/hyperledger/fabric/sampleconfig

#### Smart Contract as Go Plugin
Since I'm going to use the sample system chaincode for the smart contract from examples and little modification for native os. 
If you try with default chaincode (example02) you end with this errors as given here ![chaincode build error](https://github.com/ravinayag/Fabric-Native-OS-v1.4/blob/master/chaincode_error.png)

Hence i  recommend and used the this [chaincode.go](https://github.com/ravinayag/Fabric-Native-OS-v1.4/blob/master/chaincode.go) file for learning purpose.
###### Lets build the go plugin 

Download the chaincode.go file to the below location and build
```bash
$ cd ~/gopath/src/github.com/hyperledger/fabric/examples/chaincode/go/example02
$ go build -buildmode=plugin -o example02.so chaincode.go
```
That will produce example02.so file and will use this to our smart contract plugin binaries.

##### Configure plugin with peer
1,  edit the $FAB_CONF\core.yaml  and find this line "chaincode:" and then "system:" , now add “example02: enable” under the system.chaincode
```bash
chaincode:
  system:
    ...
    example02: enable
```
Finally it should be like this : ![1core.yaml file ](https://github.com/ravinayag/Fabric-Native-OS-v1.4/blob/master/1core.yaml.png)

2,  Now Add the plugins configuration for the same to "systemPlugins:" 
```bash
chaincode:
  systemPlugins:
      - enabled: true
        name: example02
        path: $FAB-CONF/src/github.com/hyperledger/fabric/examples/chaincode/go/example02/example02.so
        invokableExternal: true
        invokableCC2CC: true
```
Finally it should be like this : ![2core.yaml file ](https://github.com/ravinayag/Fabric-Native-OS-v1.4/blob/master/2core.yaml.png)

## Time to Launch Fabric Network 
 Our fabric consists for 1 orderer and 1 peer using profile sampleconfig given in the configtx.yaml.
 Lets open two additional terminals and export your paths given above.
 
 
 #### Start the Orderer service first one terminal.
 ```bash
 $ sudo mkdir -p  /var/hyperledger && sudo chown $(whoami):$(whoami) /var/hyperledger
 
 $ FABRIC_CFG_PATH=$FAB_CONF ORDERER_GENERAL_GENESISPROFILE=SampleSingleMSPSolo $FAB_BIN/orderer
 ```
 #### Start the peer service next  on second terminal
 ```bash
 $ FABRIC_CFG_PATH=$FAB_CONF FABRIC_LOGGING_SPEC=gossip=warn:msp=warn:debug $FAB_BIN/peer node start
 ```
Let the orderer and peer run on the two terminals, now you back to old terminal and ensure, don’t have any errors and running.
 
Now lets create the channel for our smart contract communication.  We use configtxgen tool to generate a  channel based on $FAB_CONF\configtx.yaml, Open a new terminal, 
```bash
$ FABRIC_CFG_PATH=$FAB_CONF $FAB_BIN/configtxgen -profile SampleSingleMSPChannel -outputCreateChannelTx mychannel.tx -channelID mychannel
```

A file will be created as mychannel.tx and we will use this file to create channel in the network with orderer.
```bash 
$ FABRIC_CFG_PATH=$FAB_CONF $FAB_BIN/peer channel create -f ./mychannel.tx -c mychannel -o 127.0.0.1:7050
```
_mychannel.block_  containing the artifacts about the channel and we ask peers to join the channel. 
```bash
$ FABRIC_CFG_PATH=$FAB_CONF $FAB_BIN/peer channel join -b mychannel.block
```
If everything perfect, then did a wonderfull job. _Kudos !!!_

### Initiate the Transactions with smart contract

we have to explicitly initialize our smart contract assets first.
```bash
$ FABRIC_CFG_PATH=$FAB_CONF $FAB_BIN/peer chaincode invoke -C mychannel -n example02 -c '{"Args":["invoke", "a", "500", "b","200"]}'
```
> After successfully initialized, we can send/query  other transactions. For example:
```bash
$ FABRIC_CFG_PATH=$FAB_CONF $FAB_BIN/peer chaincode invoke -C mychannel -n example02 -c '{"Args":["transfer","a","b","100"]}'

$ FABRIC_CFG_PATH=$FAB_CONF $FAB_BIN/peer chaincode query -C mychannel -n example02 -c '{"Args":["query","a"]}'
```

Now you able to do chaincode transactions and query.

Kindly drop me msg for comments. 

