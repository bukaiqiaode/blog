I planned to build an environment of Jenkins to learn CI.
Ubuntu is selected to be the OS for this.

## Step 1
Install Ubuntu on PC.<br/>
I used Ubuntu **v14.04.1**

## Step 2
Review the [official documentation](https://jenkins.io/doc/book/installing/) to get the overview of the installation.

## Step 3
Install Java 8 **runtime** on Ubuntu.<br/>
Firstly, I tried the default installation.
```shell
  sudo apt-get install default-jre
```
But for my version of Ubuntu, it only installed Java 7 for me, which is not the one I want.

To install Java 8, I followed the [Oracle offical introduction](http://www.webupd8.org/2012/09/install-oracle-java-8-in-ubuntu-via-ppa.html) and used the following commands
```shell
  sudo add-apt-repository ppa:webupd8team/java
  sudo apt-get update
  sudo apt-get install oracle-java8-installer
```

After installation, I also follow the instruction to set Java 8 to be default.
```shell
  sudo apt-get install oracle-java8-set-default
```

## Back to [index](./index.md)
