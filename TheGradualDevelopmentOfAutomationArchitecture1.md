# 自动化框架的逐步演进(1)

## 无数据收集阶段
我们在项目的开始就认识到，收集业务数据(测试结果)的重要性。</br>
有了数据，我们就可以从不同的角度，对不同的业务状态进行统计，今儿分析业务的进度和质量。例如迭代过多少个版本；每个版本的运行时间是多少；各版本上执行了哪些脚本，通过率如何；每个脚本在不同版本上的表现如何，等等。</br>
但是，由于前期人手不够，并没有实际进行数据的收集，只是在系统架构过程中对此进行了一定的思考和预留。</br>
这个时期没有数据收集。

## 简单Python脚本阶段
在业务运行起来之后，我们得以考虑进行数据的收集。</br>
我们计划在数据收集方面实践微服务的理念，这要求我们对业务流程进行相当程度的细化和解耦，已达到灵活地进行设计、开发、部署、替换的目的。</br>
我们决定采用MySQL数据库，采用Python来快速搭建原型。我们使用Python的bottle框架，搭建了简单的WebService来收集业务数据。为了保证安全性，我们采用token的方式来保证客户端和服务端的通信。</br>

## Apache转发阶段
我们快速完成了基于Python的开发并部署到云上进行验证。在实际使用过程中我们发现，云上的服务器面临各种端口扫描和探测等网络攻击。虽然攻击的规模不大，但是简单的bottle实现依然无法承受，出现了服务不可用的情形。</br>
根据分析，攻击IP有的来自国外，有的来自腾讯云等云服务商，有的来自电信等传统运营商，显然不能通过黑名单的方式来阻止来自万维网的进攻。同样，系统的分布式特性，也意味着不能使用白名单。</br>
由此，我们考虑使用包容性策略，使用成熟的开源服务器来容纳一定的攻击并保证服务的可用性。我们使用Apache2来接受请求，然后使用代理模块将请求转发到Python服务上，进行业务逻辑的处理。</br>
Ubuntu下启用Apache2的代理转发需要以下步骤：
### 启用Apache的相应模块
```shell
  a2enmod proxy
  a2enmod proxy_http
```
### 编辑Apache的站点配置文件(/etc/apache2/sites-enabled/xxxx)
```shell
<VirtualHost .*>
  ProxyPreserveHose On
  ProxyPass / http://111.222.333.444:9999/
</VirtualHost>
```
这样，就由Apache作为第一层，来接受请求并转发到相应的端口。而Python服务脚本监听的端口可以不公开，也可以部署在其他私有云的机器上，从系统层面减小了受攻击面。通过合理配置转发规则，还可以屏蔽掉大部分格式不对的“脏请求”。</br>
未来还可以在现有Python服务脚本不停机的情况下，部署新的服务脚本，只需要重新启动Apache即可，这给我们的部署和管理提供了更多的可能性。</br>
当然，也可以使用Iptables类配置转发，我们并没有采用这种方式。

## Spring Boot过滤阶段
虽然使用Apache的转发可以避免Python直接暴露在外，通过修改转发规则也可以屏蔽掉大部分"脏请求"，但我们还是希望能够继续对请求进行过滤，尽量使得只有合法的请求才能到达Python服务。同时我们还希望能够实践服务发现、负载均衡、服务降级与恢复等方面的技术。因此我们选择在Apache和Python服务中间增加一层Spring Boot逻辑，对转发过来的请求进行过滤，并实践其他技术。</br>
外部请求 -> Apache(过滤掉格式错误的请求) -> Spring Boot层(从业务层面检查请求的合法性) -> Python服务(业务检查，数据处理) -> MySQL(持久化数据)

Maven在pom.xml中配置对okhttp3的引用.
```xml
		<dependency>
			<groupId>com.squareup.okhttp3</groupId>
			<artifactId>okhttp</artifactId>
			<version>3.10.0</version>
		</dependency>
```
使用okhttp3发送POST请求.
```java
    /** 
     * 发送httppost请求 
     *  
     * @param url 
     * @param data  提交的参数为key=value&key1=value1的形式 
     * @return 
     */  
	private String httpPost(String url, String data) {  
        String result = null;  
        OkHttpClient httpClient = new OkHttpClient();  
        RequestBody requestBody = RequestBody.create(MediaType.parse("text/html;charset=utf-8"), data);  
        Request request = new Request.Builder().url(url).post(requestBody).build();  
        try {  
            Response response = httpClient.newCall(request).execute();  
            result = response.body().string();  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
        return result;  
    }
```


## 参考与引用
- 配置 Apache转发 https://stackoverflow.com/questions/33703965/spring-boot-running-app-on-port-80
- Spring Boot搭建Restful服务 https://spring.io/guides/gs/rest-service/
- Java中String的比较 https://www.cnblogs.com/dongguol/p/5845076.html
- OkHttp的Github http://square.github.io/okhttp/
- GET与POST的区别 https://zhuanlan.zhihu.com/p/22536382
