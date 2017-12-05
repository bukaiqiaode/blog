I planned to build an environment of Jenkins to learn CI.
Ubuntu is selected to be the OS for this.

## Step 1 Install Ubuntu on PC
> _Estimation: 40 minutes_

I used Ubuntu **v14.04.1**

## Step 2
> _Estimation: 20 minutes_

Review the [official documentation](https://jenkins.io/doc/book/installing/) to get the overview of the installation.

## Step 3
> _Estimation: 30 minutes_


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

## Step 4
> _Estimation: 10 minutes_

Jenkins [official documentation](https://jenkins.io/doc/book/installing/) provided two ways to install Jenkins
- Install Docker and run Jenkins using Docker.
- Don't use Docker and run Jenkins with WAR file.

I decided not to use Docker now, to reduce the complexity of my work.
I follow the document and uses the below commands to install Jenkins.
```shell
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    sudo apt-get update
    sudo apt-get install jenkins
```

## Step 5
> _Estimation: 20 minutes_

Change the port to be '8081' following [document](https://jenkins.io/doc/book/installing/#debian-ubuntu)

Start Jenkins, as root user.
```
  sudo /etc/init.d/jenkins start
```
Find the password to unlock Jenkins, following [document](https://jenkins.io/doc/book/installing/#unlocking-jenkins)

On my Ubuntu, password can be found by using command `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` or `cat /var/log/jenkins/jenkins.log`

## Step 6
> _Estimation: 20 minutes_

The work is quite straight-forward, just follow the documentation to install plug-ins and create administrator user.


## Back to [index](./index.md)
