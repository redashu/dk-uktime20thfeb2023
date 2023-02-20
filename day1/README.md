## getting started with Docker 

### problem with app deployment in bare-metal 

<img src="prob.png">

### app conflict in bare-metal server

<img src="appc.png">

### solution of bare-metal related problems using -- Virtaul machine 

<img src="vm.png">

### problem with vm 

<img src="provm.png">

## Introduction to containers 

<img src="cont1.png">

### technology to manage containers 

<img src="cr.png">

### Docker CE vs docker ee 

<img src="docker-info.png">

## access docker host with personal user account 

```
fire@ashutoshhs-MacBook-Air ~ % ssh  ashu@43.205.200.84
ashu@43.205.200.84's password: 

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[ashu@docker-host ~]$ 
[ashu@docker-host ~]$ whoami
ashu
[ashu@docker-host ~]$ hostname
docker-host
[ashu@docker-host ~]$ 


```

### Installing docker-ce on Linux VM --make linux mv is having 3.10 or later kernel version 

```
[root@docker-host ~]# yum install docker 
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                                                               | 3.7 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package docker.x86_64 0:20.10.17-1.amzn2.0.2 will be installed
--> Processing Dependency: runc >= 1.0.0 for package: docker-20.10.17-1.amzn2.0.2.x86_64
--> Processing Dependency: libcgroup >= 0.40.rc1-5.15 for package: docker-20.10.17-1.amzn2.0.2.x86_64
--> Processing Dependency: containerd >= 1.3.2 for package: docker-20.10.17-1.amzn2.0.2.x86_64
--> Processing Dependency: pigz for package: docker-20.10.17-1.amzn2.0.2.x86_64
--> Running transaction check
---> Package containerd.x86_64 0:1.6.8-1.amzn2.0.1 will be installed
---> Package libcgroup.x86_64 0:0.41-21.amzn2 will be installed
---> Package pigz.x86_64 0:2.3.4-1.amzn2.0.1 will be installed
---> Package runc.x86_64 0:1.1.4-1.amzn2.0.1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

```

### starting docker daemon service 

```
[root@docker-host ~]# systemctl start docker 
[root@docker-host ~]# systemctl status  docker 
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2023-02-20 09:29:00 UTC; 4s ago
     Docs: https://docs.docker.com
  Process: 1526 ExecStartPre=/usr/libexec/docker/docker-setup-runtimes.sh (code=exited, status=0/SUCCESS)
  Process: 1525 ExecStartPre=/bin/mkdir -p /run/docker (code=exited, status=0/SUCCESS)
 Main PID: 1530 (dockerd)
    Tasks: 10
   Memory: 21.3M
   CGroup: /system.slice/docker.service
           └─1530 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --default-ulimit nofile=32768:65536

Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.361100676Z" level=info msg="ClientConn switching b...e=grpc
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.390311947Z" level=warning msg="Your kernel does no...eight"
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.390351091Z" level=warning msg="Your kernel does no...evice"
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.390537804Z" level=info msg="Loading containers: start."
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.733687372Z" level=info msg="Default bridge (docker...dress"
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.807392970Z" level=info msg="Loading containers: done."
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.819177152Z" level=info msg="Docker daemon" commit=....10.17
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.819282911Z" level=info msg="Daemon has completed i...ation"
Feb 20 09:29:00 docker-host systemd[1]: Started Docker Application Container Engine.
Feb 20 09:29:00 docker-host dockerd[1530]: time="2023-02-20T09:29:00.840066219Z" level=info msg="API listen on /run/docker.sock"
Hint: Some lines were ellipsized, use -l to show in full.
[root@docker-host ~]# systemctl enable   docker 
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@docker-host ~]# 

```

### a non root /admin user is not allowed to connect docker host

```
[ashu@docker-host ~]$ whoami
ashu
[ashu@docker-host ~]$ docker version 
Client:
 Version:           20.10.17
 API version:       1.41
 Go version:        go1.18.6
 Git commit:        100c701
 Built:             Sat Dec  3 04:13:49 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied
[ashu@docker-host ~]$ 

```

### solution for a single and all users 

```
[root@docker-host ~]# usermod -aG docker ashu 
[root@docker-host ~]# 
[root@docker-host ~]# for  i in `ls /home`
> do
> usermod -aG docker $i
> done
[root@docker-host ~]# 



```




