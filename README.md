# Tutorial Hyperledger Fabric

This tutorial is aimed to help you setting up and running your own Hyperledger Fabric network locally.

This tutorial will not explain what Hyperledger Fabric is. This was covered in the lecture. The ultimate goal of this tutorial is not to train you to work with Hyperledger Fabric, it is to give you a feel of what it is like to work with a production-ready blockchain application.

Therefore this tutorial will also not explain the internal workings of Hyperledger Fabric; that is, you will use it as a block box. This means that not all instructions below will be motivated in-depth. Unless explained in more detail in this tutorial, you can assume you *just need to do* a command in order to get Hyperledger Fabric working, and that the details of the step are not important to your ability to play around with the software.

## Prerequisites

This tutorial is written for `Ubuntu Linux 16.04 LTS` (**64 bit**) and should also work on other Debian based Linux distributions. It should also, without too much hassle, work on MacOS, **but without any guarantees**. If you wish to use MacOS or Windows, please know that we **might not be able to help you**.

This tutorial assumes that you already have a working installation of Ubuntu 16.04 LTS (**64 bit**), and will not cover installation and configuration. It also assumes you are familiar with the command line. You needn't do many difficult things, but you should at least know how to use it.

**Make sure your Ubuntu (virtual machine) has a disk of around 20GB.**

## Setting up

Although Hyperledger Fabric has installation and prerequisites documentation, the process is all rather technical. That is why we chose to outline the preparation process ourselves, tailored to the specific Ubuntu Linux installation we are going to use.

### Docker

Hyperledger Fabric is provided as a `Docker` image, so `Docker` is the first component that we will install. Docker already has a great tutorial for installing the latest Docker version on Ubuntu from their own repository. You can find it [**here**](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository). This link links directly to the right starting point for the installation. Follow the instructions until you hit the `UPGRADE DOCKER CE` heading and stop there. From the instructions behind the `Linux postinstall` link, be sure to execute the steps in the `Manage Docker as a non-root user` section. Verify your installation succeeded by executing the command below and verify that you have at least version `17.06.02-ce` running (at this time you will probably have installed version `17.12.0-ce`).

```
docker --version
```

Also install `docker-compose` like so:

```
sudo apt-get install docker-compose
```

And verify it worked with:

```
docker-compose --version
```

This should give version `1.8` or higher.

### Go

Next we need to install the `Go` programming language:

```
sudo add-apt-repository ppa:gophers/archive
sudo apt-get update
sudo apt-get install golang-1.9-go
```

This should install `Go` in `/usr/lib/go-1.9/`. The `Go` installer doesn't add the right binaries to your `PATH` yet, so we'll do this now. This tutorial assumes you are using the `bash` shell (if you didn't configure a shell yourself, you are using `bash`). Open `~/.bashrc` in your favorite editor and add this to the bottom of the file:

```
export GOPATH=/usr/lib/go-1.9
export PATH=$PATH:$GOPATH/bin
```

Restart your shell. You can now verify `Go` is correctly installed and the environment variables set by executing:

```
go version
```

If it shows you are using version `1.9.2` or newer, you are golden. You can ignore the `warning: GOPATH set to GOROOT (/usr/lib/go-1.9) has no effect` warning.

### Node.js

Next, we'll need to install `Node.js` and `npm` again, but this time we will use a different version. If you are working on the same machine as the `Ethereum` tutorial, please uninstall all existing repositories and binaries first:

```
sudo apt-get remove --purge nodejs npm
sudo rm /etc/apt/sources.list.d/nodesource.list*
```

Now you can install `Node.js 6.12` and `npm 3.10`:

```
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Python

We also need `Python 2.7`. This should already be installed by default in your Ubuntu installation. The following command should show you version `2.7` or higher:

```
python --version
```

If it shows version `3.0` or higher, or if the command fails, use:

```
sudo apt-get install python
```

## Running your first network

Now, Hyperledger Fabric has an excellent tutorial available [**here**](https://hyperledger-fabric.readthedocs.io/en/latest/build_network.html) that we will follow from now on. So open that tutorial and let's get started. Do finish reading this document first, so you know what to expect from the tutorial.

In that tutorial, it also notes that you may need to install the [**Hyperledger Fabric Samples**](https://hyperledger-fabric.readthedocs.io/en/latest/samples.html). Be sure to follow those instructions as soon as it is mentioned in the tutorial. Otherwise you will run into errors.

The tutorial will guide you through the steps required to:

* configure a local Hyperledger Fabric network;
* bootstrap several nodes on that network;
* create a channel and join nodes to that channel;
* run an example chaincode on your channel.

Note that the lower sections of the tutorial, starting with `Understanding the Docker Compose topology`, are information only and you can read them in your own time. There is also a `troubleshooting` section with some common troubleshooting steps, if you don't want to wait for assistance.

As Hyperledger Fabric is under active development, it changes frequently. We have found that you need to do a few things different from the linked tutorial to avoid errors. The linked tutorial is not yet updated with these changes. This steps are outlined below.

### Before *Bring Up the Network* in the linked tutorial

Open the file `base/docker-compose-base.yaml` in an editor and look for the line:

```
image: hyperledger/fabric-orderer
```

Change this line to:

```
image: hyperledger/fabric-orderer:x86_64-1.0.5
```

Also open the file `base/peer-base.yaml` and look for the line:

```
image: hyperledger/fabric-peer
```

And change it to:

```
image: hyperledger/fabric-peer:x86_64-1.0.5
```

Finally open the file `docker-compose-cli.yaml` and look for the line:

```
image: hyperledger/fabric-tools
```

And change it to:

```
image: hyperledger/fabric-tools:x86_64-1.0.5
```

## Writing your own chaincode

If you managed to complete the `Running your first network` tutorial and have some time left, you can try to write your own, small, chaincode program. You can use the example chaincode from the tutorial as a basis. You can find the source [here](https://github.com/hyperledger/fabric-test/blob/master/chaincodes/example02/node/chaincode_example02.js). That repository also contains more chaincode example code.

Revisit [these](https://hyperledger-fabric.readthedocs.io/en/latest/build_network.html#install-instantiate-chaincode) instructions on how to deploy chaincode to your own Hyperledger Fabric network, or visit [this](https://hyperledger-fabric.readthedocs.io/en/latest/chaincode4ade.html) page for more information on writing chaincode (note that that document is mainly focused on Go).
