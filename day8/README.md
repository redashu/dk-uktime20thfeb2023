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

