
# Install Erlang
1. Remove existing Erlang, `yum remove erlang*`
2. Set up yum repository, following https://github.com/rabbitmq/erlang-rpm

```bash
# In /etc/yum.repos.d/rabbitmq_erlang.repo
[rabbitmq_erlang]
name=rabbitmq_erlang
baseurl=https://packagecloud.io/rabbitmq/erlang/el/6/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[rabbitmq_erlang-source]
name=rabbitmq_erlang-source
baseurl=https://packagecloud.io/rabbitmq/erlang/el/6/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/rabbitmq/erlang/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
```

3. Install Erlang again, `yum install erlang`

# Install RabbitMQ
1. Set up repository, following https://packagecloud.io/rabbitmq/rabbitmq-server/install#bash-rpm

```shell
curl -s https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.rpm.sh | sudo bash
```

2. Install RabbitMQ, `yum install rabbitmq-server`

# Start RabbitMQ server

# Reference
- [Installing on RPM-based Linux (RHEL, CentOS, Fedora, openSUSE)](https://www.rabbitmq.com/install-rpm.html)