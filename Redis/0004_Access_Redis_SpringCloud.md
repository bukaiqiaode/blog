# Guide to access remote Redis using Spring-Cloud
## Step 1 Read the Spring-Cloud-Guide
> _Estimation: 20 minutes_

Read the guide to get a general view of its logic.

## Step 2 Download the guide and inspect the code using STS
> _Estimation: 20 minutes_

Walk through the code of the sample project.

## Add YML file to the project to define the connection parameters
> application.yml
```yml
spring:
  redis:
    database: 1
    host: aaa.bbb.ccc.ddd
    port: 6379
    password: xxxyyyzzz
```

## Resolution of issues when access Redis thru Spring-Cloud
```shell
 org.springframework.data.redis.RedisConnectionFailureException: Unable to connect to Redis; nested exception is io.lettuce.core.RedisException: io.lettuce.core.RedisConnectionException: DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```

> Define a password in `redis.conf` under `requirepass` directive, and run with `redis-server path/to/redis.conf`

```shell
org.springframework.data.redis.RedisConnectionFailureException: Unable to connect to Redis; nested exception is io.lettuce.core.RedisConnectionException: Unable to connect to aaa.bbb.ccc.ddd:6379
```

> Comment the `bind` directive in `redis.conf`

```shell
org.springframework.data.redis.RedisConnectionFailureException: Unable to connect to Redis; nested exception is io.lettuce.core.RedisCommandExecutionException: NOAUTH Authentication required.
```

> Comment the `requirepass` line and then set `protected-mode no` in `redis.conf`.

## References
- [Official guide from Spring-Cloud org](https://cloud.spring.io/)

## Back to [index](./index.md)
