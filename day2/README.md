### Docker components 

<img src="compo.png">

### application containerization -- > source code -- build -->images --- run --> containers 

<img src="b1.png">

### image building tools 

<img src="b2.png">

### vscode setup 

<img src="vscode.png">

### Solution of  Question 1 

```
[ashu@docker-host ashu-app-images]$ docker run -d --name ashuc1  alpine sleep 1000 
ffb52d2a562d06cc77de9fe23b2f8d291db288c13bb99c397d0ec33303a784a9
[ashu@docker-host ashu-app-images]$ docker run -d --name ashuc2  alpine sleep 1000 
efe0c4dade13bb60534f865a71c6a11fe5746eca81dd3241b938a1b9b259d2f2
[ashu@docker-host ashu-app-images]$ 




[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ docker  exec -it   ashuc1  sh 
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # pw
sh: pw: not found
/ # pwd
/
/ # echo heyyy  >/helloc1.txt 
/ # ls
bin          etc          home         media        opt          root         sbin         sys          usr
dev          helloc1.txt  lib          mnt          proc         run          srv          tmp          var
/ # exit
[ashu@docker-host ashu-app-images]$ 
```
## sample Dockerfile Example to run python code 

```
FROM python:latest
# we are refering docker hub python image 
LABEL name=ashutoshh
LABEL email=ashutoshh@linux.com 
# optional field but you can write image owner details
RUN mkdir /mycode 
# is used to run any command inside the image while building
COPY ashu.py /mycode/ashu.py 
# copy code inside docker image while image buidling time 
CMD ["python","/mycode/ashu.py"]
# to let container know how to run code while container is getting created 
# for the first time 
# is fix the default process of container 
```

### lets build our first docker image 

```
[ashu@docker-host ashu-app-images]$ ls
java-code  python-code  webapp-code
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ docker build  -t ashupy:1.1  python-code/ 
Sending build context to Docker daemon  3.584kB
Step 1/6 : FROM python:latest
 ---> f92346e0c39e
Step 2/6 : LABEL name=ashutoshh
 ---> Running in 78d01fbb618f
Removing intermediate container 78d01fbb618f
 ---> 5992e5eb1ca4
Step 3/6 : LABEL email=ashutoshh@linux.com
 ---> Running in 6eca31809658
Removing intermediate container 6eca31809658
 ---> d1ce700dc1a4
Step 4/6 : RUN mkdir /mycode
 ---> Running in 3f7aa317a006
Removing intermediate container 3f7aa317a006
 ---> d9a269fac8c6
Step 5/6 : COPY ashu.py /mycode/ashu.py
 ---> a847b4d0923e
Step 6/6 : CMD ["python","/mycode/ashu.py"]
 ---> Running in b6b21400383b
Removing intermediate container b6b21400383b
 ---> fdf624196e69
Successfully built fdf624196e69
Successfully tagged ashupy:1.1
[ashu@docker-host ashu-app-images]$ 
```

### lets check it 

```
ashu@docker-host ashu-app-images]$ docker images
REPOSITORY           TAG           IMAGE ID       CREATED          SIZE
ra                   1.1           583a2ae574d6   29 seconds ago   925MB
ashupy               1.1           fdf624196e69   36 seconds ago   925MB
nginx                alpine        2bc7edbc3cf2   9 days ago       40.7MB
bash                 latest        4f9df00fa318   10 days ago      12.3MB
```






