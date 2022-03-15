---
layout: post
title: "[etc] vagrant ssh 대신 ssh 접속"
subtitle: "vagrant ssh"
categories: study
tags: etc
---

### 1. vagrant ssh-config, ssh -i 옵션 사용

1. `$ vagrant ssh-config` 

```txt
Host m-k8s
  HostName 127.0.0.1
  User vagrant
  Port 60010
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/xxx/vagrant/.vagrant/machines/m-k8s/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

2. `$ ssh -p 60010 -i /Users/xxx/vagrant/.vagrant/machines/m-k8s/virtualbox/private_key root@127.0.0.1`

### 2. ssh 접속

1. `$ ssh -p 60010 root@127.0.0.1`
   - 초기 암호 : vagrant



```
[root@m-k8s ~] 
```

