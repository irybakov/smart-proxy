# RabbitMQ Installation 

## update and install wget

``` sh
  sudo yum -y update
  sudo yum -y install wget
```

## install erlang

``` sh
  wget https://www.rabbitmq.com/releases/erlang/erlang-18.1-1.el7.centos.x86_64.rpm
  sudo yum -y install erlang-18.1-1.el7.centos.x86_64.rpm 
```
## install rabbitMQ

``` sh
  wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.5.7/rabbitmq-server-3.5.7-1.noarch.rpm
  sudo rpm --import https://www.rabbitmq.com/rabbitmq-signing-key-public.asc
  sudo yum -y install rabbitmq-server-3.5.7-1.noarch.rpm
```

## configure autostart

``` sh
  sudo chkconfig rabbitmq-server on
```

## start server

``` sh
 sudo /sbin/service rabbitmq-server start
```
## install plugings

``` sh
sudo rabbitmq-plugins enable rabbitmq_management
```

>### federation 

``` sh
  sudo rabbitmq-plugins enable rabbitmq_federation
  sudo rabbitmq-plugins enable rabbitmq_federation_management
```

## add admin user
``` sh
  rabbitmqctl add_user admin admin
  rabbitmqctl set_user_tags admin administrator
  rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"
```
