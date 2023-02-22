## Revision 

<img src="rev.png">

### container life cycle 

<img src="lifec.png">

### socket 

<img src="socket.png">


## Namespace for container isolation 

<img src="iso.png">

### sharing files between to containers 

<img src="iso1.png">

### demo 

```
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc1  alpine sleep 1000
d1d3b7b6a3d952e0afbea36f97ce8d22f4fcb12dd94f32f3c71e98c0fe99c63a
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc2  alpine sleep 1000
44b3ea905cc6442d4015768c5778f806b8fb5a2086a98b46fa4e7018b82e9fb3
[ashu@docker-host ashu-app-images]$ docker  exec -it ashuc1 sh 
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # echo hey >/helloc1.txt 
/ # ls
bin          etc          home         media        opt          root         sbin         sys          usr
dev          helloc1.txt  lib          mnt          proc         run          srv          tmp          var
/ # exit
[ashu@docker-host ashu-app-images]$ ls
java-code  python-code  webapp-code
[ashu@docker-host ashu-app-images]$ docker  cp  ashuc1:/helloc1.txt  . 
[ashu@docker-host ashu-app-images]$ ls
helloc1.txt  java-code  python-code  webapp-code
[ashu@docker-host ashu-app-images]$ docker cp helloc1.txt   ashuc2:/tmp/
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ docker exec -it ashuc2 ls /tmp
helloc1.txt
[ashu@docker-host ashu-app-images]$ 


```

### limting hardware resources in containers using control groups --

<img src="cg.png">

### creating container with RAM restriction 

```
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashutest1  --memory 200M  ashupy:2.1 
```

### checking status 

```
docker stats 
=====
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT   MEM %     NET I/O     BLOCK I/O    PIDS
7a13193d32eb   ashutest1       0.00%     3.805MiB / 200MiB   1.90%     570B / 0B   9.2MB / 0B   1
9eeb01c5a6ea   mfaragc2        0.00%     336KiB / 15.62GiB   0.00%     920B / 0B   0B / 0B      1
047f9764e79b   mfaragc1        0.00%     368KiB / 15.62GiB   0.00%     920B / 0B   0B / 0B      1
cf62ab5c9ed4   ihlushchenko2   0.00%     448KiB / 15.62GiB   0.00%     920B / 0B   0B / 0B      1
215b94d7b9bc   ihlushchenko1   0.00%     468KiB / 15.62GiB   0.00%     920B / 0B   0B / 0B      1
```

### CPU limits 

```
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashutest2  --cpuset-cpus=1 --cpu-shares=400  ashupy:2.1 
d426fda5ae6b87c251e79850bae57cc60d0dead093b4f255af40bdf6323be9d1
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashutest2  --cpuset-cpus=1 --cpu-shares=400  --memory 500M ashupy:2.1 


```

## Docker container networking 

### creating a docker image for sample webappge hosting 

### Understanding web servers 

<img src="web.png">

### sample html code in ashu.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ashu-webui</title>
</head>
<body>
    <h1> this is my first web page </h1>
    <h2> heading of webpage </h2>
    
</body>
</html>
```

### Dockerfile 

```
FROM nginx
# using image from docker hub 
LABEL   name=ashutoshh
LABEL   email=ashutoshh@linux.com
LABEL   team=mobi-dev
# all labels are optional 
#COPY ashu.html /usr/share/nginx/html/
ADD ashu.html /usr/share/nginx/html/
# COPY and ADD both are same but add can use URL as source 
# if we don't provide CMD in Dockerfile then -- base image CMD will be 
# inherited  
```

### lets build it 

```
[ashu@docker-host ashu-app-images]$ ls
java-code  python-code  webapp
[ashu@docker-host ashu-app-images]$ ls webapp/
ashu.html  Dockerfile
[ashu@docker-host ashu-app-images]$ docker build -t ashu-webui:v1  webapp/ 
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM nginx
 ---> 3f8a00f137a0
Step 2/5 : LABEL   name=ashutoshh
 ---> Running in c2c0bcd28383
Removing intermediate container c2c0bcd28383
 ---> a35f5ff909e7
Step 3/5 : LABEL   email=ashutoshh@linux.com
 ---> Running in e8967ff1b82e
Removing intermediate container e8967ff1b82e
 ---> 8be7864b7e5e
Step 4/5 : LABEL   team=mobi-dev
 ---> Running in 90e25bfa338d
Removing intermediate container 90e25bfa338d
 ---> 7bd319c6daa4
Step 5/5 : ADD ashu.html /usr/share/nginx/html/
 ---> 305540e8a62f
Successfully built 305540e8a62f
Successfully tagged ashu-webui:v1
```

### 

```
[ashu@docker-host ashu-app-images]$ docker images  |  grep ashu
ashu-webui                         v1               305540e8a62f   14 seconds ago   142MB
dockerashu/ashupy                  2.1              fce707ac27b8   24 hours ago     454MB
ashupy                             2.1              fce707ac27b8   24 hours ago     454MB
ashupy                             2.11             fce707ac27b8   24 hours ago     454MB
ashupy                             1.2gmailupdate   6fb1e2cfc106   24 hours ago     925MB
ashupy                             1.2              d64d5203ff8a   24 hours ago     925MB
```



