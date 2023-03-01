## REvision 

### application COntainerization process 

<img src="appc.png">

### k8s control plane/master and data plane / worker / minions 

<img src="k8s.png">

### to deploy app -- we are having some k8s Resources like pod / rc / rs /deployment 

<img src="dep.png">

### Service introduction 

<img src="svc.png">

### creating deployment YAML using some Docker hub image 

```
kubectl   create  deployment  ashu-ui-app --image=docker.io/dockerashu/newapp:v1  --port 80 --dry-run=client  -o yaml  >day8_deployment.yaml 
```

### modifiy deployment file if required 

```
apiVersion: apps/v1 # apiserver Version to Understand request
kind: Deployment 
metadata:
  creationTimestamp: null
  labels:
    app: ashu-ui-app
  name: ashu-ui-app # name of deployment 
spec:
  replicas: 1 # number of pods 
  selector:
    matchLabels:
      app: ashu-ui-app
  strategy: {}
  template: # any number of pod we want to create --i.e using template 
    metadata:
      creationTimestamp: null
      labels: # lable of my Pods 
        app: ashu-ui-app
    spec:
      containers:
      - image: docker.io/dockerashu/newapp:v1
        name: newapp
        ports:
        - containerPort: 80
        resources: {}
        env: # to create or pass Env variable value 
        - name: deploy  # name of Env 
          value: ui3 # value of ENv--values ui1 , ui2 , ui3 
status: {}

```

### lets deploy 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ ls
ashu_deployment.yaml  ashupod1.yaml  day8_deployment.yaml  newpod.yaml  nodeport.yaml
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  create  -f  day8_deployment.yaml 
deployment.apps/ashu-ui-app created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  deploy
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   0/1     1            0           10s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  pods
NAME                             READY   STATUS              RESTARTS      AGE
ashu-ui-app-87d59bd5-mqnb5       1/1     Running             0             20s
mfarag-ui-app-688f66c45c-xvzmd   0/1     ContainerCreating   0             7s
shamaa-pod002                    1/1     Running             1 (17h ago)   23h
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 

```

### Reality of Deployment 

<img src="realdep.png">

### again understanding service usage and two types also 

<img src="svc1.png">

### creating service using exposing deployment 

```
ashu_deployment.yaml  ashupod1.yaml  day8_deployment.yaml  newpod.yaml  nodeport.yaml
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  deploy
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
abdo-ui-app           1/1     1            1           16m
amr-ui-app            1/1     1            1           12m
ashu-ui-app           1/1     1            1           17m
hhalkhazragy-ui-app   1/1     1            1           17m
ihor-ui-app           1/1     1            1           16m
mfarag-ui-app         1/1     1            1           17m
rjamaro-ui-app        1/1     1            1           15m
shamaa-ui-app         1/1     1            1           14m
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  expose  deployment  ashu-ui-app  --type  NodePort  --port 1234 --target-port 80 --name ashu-lb111 --dry-run=client  -o yaml  >day8_nodeport.yaml 
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl create  -f  day8_nodeport.yaml 
service/ashu-lb111 created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  svc 
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
ashu-lb111   NodePort    10.101.3.198   <none>        1234:30349/TCP   5s
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP          45h
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 

```


