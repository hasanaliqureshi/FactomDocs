Community Tester Install Guide for Factom Federation (M2)
==========

This will walk you through setting up the experimental distributed version of Factom.

### Prepare Operating System

The most testing to date has been done under Linux. To get the best testing experience, it should be done on Linux. That will also help us recreate the bugs you experience better. If you are running Windows, please install a virtual machine to run Factom. Installing on Windows has been known to work, but is not covered here.

Here are some good directions to get a virtual machine installed with Linux. http://www.instructables.com/id/Introduction-38/?ALLSTEPS

The following are assuming you are using the 64 bit version of Ubuntu, or one of its descendant projects like xubuntu or mint.

#### Install the go language and dependencies, then setup GOPATH

###### Install Package Managers 

In a terminal window, install Git

On Linux:
```
sudo apt-get install git
```

On Mac:
Steps 1 and 3 should be enough in this tutorial:
https://confluence.atlassian.com/pages/viewpage.action?pageId=269981802

###### Install Golang

Download latest version of go https://golang.org/dl/  This example uses 64 bit Linux and 1.7.3 is the latest version.
```
sudo tar -C /usr/local -xzf go1.7.3.linux-amd64.tar.gz
```

On Mac, installing go this way should work:
http://www.cyberciti.biz/faq/installing-go-programming-language-on-mac-os-x/


###### Setup Paths

Put the go binary directory in you path.  Also add the GOPATH to your profile.
Open the file `~/.profile` and add these lines to the bottom.  If they are not exact, then your Linux may not be bootable.
```
export PATH=$PATH:/usr/local/go/bin

export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
**logout and login**.  This is the most straightforward way to run/test these changes.  Run `go version` from a terminal to verify the install.


### Install Factom

Currently, the blog post [here](https://factom.com/blog/factom-launches-federated-testnet) has some screenshots of what to expect Factom Federation (M2) to look like.

These steps will get factom installed onto a linux machine. The steps should be similar for mac.

```
# install glide, the package dependency manager
go get -u github.com/Masterminds/glide
# download the code
git clone https://github.com/FactomProject/factomd $GOPATH/src/github.com/FactomProject/factomd
git clone https://github.com/FactomProject/factom-cli $GOPATH/src/github.com/FactomProject/factom-cli
git clone https://github.com/FactomProject/factom-walletd $GOPATH/src/github.com/FactomProject/factom-walletd

# switch to the develop branch of the factom programs
cd ~/go/src/github.com/FactomProject/factomd
git checkout develop
cd ~/go/src/github.com/FactomProject/factom-cli
git checkout develop
cd ~/go/src/github.com/FactomProject/factom-walletd
git checkout develop

# get the dependencies and build each factom program
glide cc
cd ~/go/src/github.com/FactomProject/factomd
glide install
go install -v
cd ~/go/src/github.com/FactomProject/factom-cli
glide install
go install -v
cd ~/go/src/github.com/FactomProject/factom-walletd
glide install
go install -v

# done.  factomd should be installed
# you can optionally use a config file to run in a non-standard mode
# mkdir -p ~/.factom/m2/
# cp ~/go/src/github.com/FactomProject/factomd/factomd.conf ~/.factom/m2/
```


### Testing Factom

You can run a local version of factomd to get greater flexibility by running it like this:

`factomd -network=LOCAL`

This will make a new blockchain without worrying about other's data interfering with your testing.  You can use the key `factom-cli importaddress Fs1KWJrpLdfucvmYwN2nWrwepLn8ercpMbzXshd1g8zyhKXLVLWj` to get local Factoids after starting factom-walletd.

For issues with the software, please post [here](https://github.com/FactomProject/factomd/issues).



