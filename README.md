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
*	wget https://github.com/hyperledger/fabric/archive/release-1.4.zip
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
Since I'm going to use the system chaincode for the smart contract. If you try to execute with default chaincode (example02) you end with this errors. Hence i used the this [chaincode](http://github.com)


