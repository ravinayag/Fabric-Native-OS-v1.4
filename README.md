# Hyperledger Fabric on Native OS – Version used 1.4
The purpose of the project to install  and run the Hyperledger fabric v1.4 on  Native  OS ( Ubuntu 16.04).   The Smart contracts written in GO lang and  used as GO Plugins. 
Docker and docker-compose are not used in this project either for fabric peers or chaincode.

### Lets get into task to dirt our hands. 
First you need to prepare your Native OS – Ubuntu 16.04 
Setting up the environment: 
## Prerequisites
Installing the Go language
Hyperledger Fabric depends upon the Go language.
•	Go - version 1.11.9 
•	Change into your home directory with the command cd ~/  
•	Download the tar file with the command wget https://storage.googleapis.com/golang/ 
•	Unpack the file with the command tar xvzf go1*.tar.gz 

######## Now we need to set GOPATH and GOROOT with the following commands: 
•	mkdir $HOME/gopath
•	export GOPATH=$HOME/gopath
•	export GOROOT=$HOME/go
•	export PATH=$PATH:$GOROOT/bin
•	export PATH=${PWD}/bin:${PWD}:$PATH
###### Install dependencies
Next, few dependencies to install. 
sudo apt install libltdl-dev python build-essential checkinstall libssl-dev
mkdir -p $GOPATH/src/github.com/hyperledger/
cd $GOPATH/src/github.com/hyperledger/

