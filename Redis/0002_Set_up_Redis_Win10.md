# Set up Redis on Windows 10
## Step 1 Download file from server
> _Estimation: 5 minutes_

```shell
  wget http://download.redis.io/releases/redis-5.0.3.tar.gz
  tar xzf redis-5.0.3.tar.gz
  cd redis-5.0.3
```

## Step 2 Compile the source code
> _Estimation: 20 minutes_

```shell
  make
```

## Step 3 Install tools required and then run the tests
> _Estimation: 20 minutes_

```shell
   yum install tcl
   make test
```

## Step 4 Start Redis on the backgroud
> _Estimation: 2 minutes_

```shell
    nohup src/redis-server &
```

## Step 5 Start Redis-shell to interact with Redis
> _Estimation: 2 minutes_

```
  src/redis-cli
  //further options...
```

## References
- [Redis（window版本）安装及使用](https://blog.csdn.net/qq_32444825/article/details/80718650)
- [Redis安装启动及设置密码<windows>](https://www.cnblogs.com/springlight/p/6288902.html)
- [Microsoft Redis](https://github.com/MicrosoftArchive/redis/releases)

## Back to [index](./index.md)
