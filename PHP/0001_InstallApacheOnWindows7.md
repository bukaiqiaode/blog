# Backgound
Currently I have IIS enabled on my Windows-7, which seized port #80.
I also use Sprint-boot, which may take port #8080 sometimes.
So for Apache, I want to configure it to use another port.
For me, I will use port **#7070** for Apache.

# Step 1 Check usage of ports on Windows-7
- Execute `netstat -ano` in command-line console. On my machine, we can see the below output. We can also use `netstat -ano | findstr "135"` to filter the output.

```shell
C:\Users\home>netstat -ano

活动连接

  协议  本地地址          外部地址        状态           PID
  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       568
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:554            0.0.0.0:0              LISTENING       1356
  TCP    0.0.0.0:2869           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:5357           0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:10243          0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:49152          0.0.0.0:0              LISTENING       620
  TCP    0.0.0.0:49153          0.0.0.0:0              LISTENING       1072
  TCP    0.0.0.0:49154          0.0.0.0:0              LISTENING       1144
  TCP    0.0.0.0:49155          0.0.0.0:0              LISTENING       700
```

- From the output, we can see that port #135 is associated with someone with PID=568. We can use `tasklist | findstr "568"` to check the process with that PID.

```shell
C:\Users\home>tasklist | findstr "568"
svchost.exe                    568 Services                   0      8,628 K
svchost.exe                   2568 Services                   0      1,564 K
```

- If needed, we can use `taskkill /f /t /im xyz.exe` to kill the process

# Step 2 Install Apache
- Download Apache zip-file from the official site.
- Un-zip the file and move it to some where.

  >Unzip the Apache24 folder in the package zip file to the root directory on any drive. <br/>
  This is the "ServerRoot" in the config. 
  Example: c:\Apache24
  
- From command-line, change directory into the path and execute `httpd -k install` to install it.

  >To Install Apache as a service:<br/>
In most cases you will want to run Apache as a Windows Service. <br/>
To do so you install Apache as a service by typing at the command prompt [1]: `httpd -k install`.
You can then start Apache by typing `httpd -k start`.<br/>
Apache will then start and eventually release the command prompt window.<br/>
[1] You have to run the command prompt as Administrator in Windows Vista/7/2008/8/8.1/10/2012/

```shell
E:\Downloads\tools\Apache24\bin>httpd -k install
Installing the 'Apache2.4' service
The 'Apache2.4' service is successfully installed.
Testing httpd.conf....
Errors reported here must be corrected before the service can be started.
httpd: Syntax error on line 39 of
E:/Downloads/tools/Apache24/conf/httpd.conf: ServerRoot must be a valid directory
```

# Step 3 Configuring Apache
- We can use the tray-utility `bin\ApacheMonitor.exe` to monitor the status of Apache.
- Open `conf\httpd.conf`
  - Set `Listen 80` to be `Listen 7070`
  - Set `Define SRVROOT` to be `Define SRVROOT "E:/Downloads/tools/Apache24"`
  - Set `ServerName localhost:80` to be `ServerName localhost:7070`

# References
- [Check port usage on Windows](https://jingyan.baidu.com/article/3c48dd34491d47e10be358b8.html)
- [Baidu experience about installing Apache on Windows](https://jingyan.baidu.com/article/29697b912f6539ab20de3cf8.html)
- [Apache official site](http://httpd.apache.org/docs/current/platform/windows.html)
- [Document for installing PHP on Windows-7](https://www.cnblogs.com/timmmmit/archive/2017/10/22/7709483.html)

## Back to [index](./index.md)