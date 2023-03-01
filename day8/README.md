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

### Understanding NodePort 

<img src="p.png">

### creating Load Balancer service 

```
ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  expose  deployment  ashu-ui-app  --type  LoadBalancer  --port 1234 --target-port 80 --name ashu-lb222 --dry-run=client  -o yaml  >day8_lbsvc.yam
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl create  -f  day8_lbsvc.yam 
service/ashu-lb222 created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  svc
NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
abdo-lb111           NodePort       10.99.236.148    <none>        2002:30183/TCP   13m
amr-lb111            NodePort       10.109.225.40    <none>        1234:30738/TCP   13m
ashu-lb111           NodePort       10.101.3.198     <none>        1234:30349/TCP   14m
ashu-lb222           LoadBalancer   10.107.223.126   <pending>     1234:31533/TCP   4s
hhalkhazragy-10111   NodePort       10.100.214.72    <none>        1982:31560/TCP   13m
ihor-lb111           NodePort       10.109.113.221   <none>        2365:31000/TCP   11m
kubernetes           ClusterIP      10.96.0.1        <none>        443/TCP          45h
mfarag-lb111         NodePort       10.103.34.89     <none>        2244:30801/TCP   14m
rjamaro-lb           NodePort       10.96.4.107      <none>        3333:30952/TCP   13m
shamaa-lb111         NodePort       10.98.209.4      <none>        1244:30129/TCP   12m
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 
```

### Load balancer & NodePort 

<img src="lbnp.png">

### Understanding & creating namespace in k8s 

```
ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  namespaces 
NAME                   STATUS   AGE
default                Active   15d
ingress-nginx          Active   15d
kube-node-lease        Active   15d
kube-public            Active   15d
kube-system            Active   15d
kubernetes-dashboard   Active   15d
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  pods
No resources found in default namespace.
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  deploy 
No resources found in default namespace.
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  create   ns  ashu-space  --dry-run=client -o yaml >ns.yaml 
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  create   ns  ashu-space  
namespace/ashu-space created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  ns
NAME                   STATUS   AGE
ashu-space             Active   8s
default                Active   15d
ingress-nginx          Active   15d
kube-node-lease        Active   15d
kube-public            Active   15d
kube-system            Active   15d
kubernetes-dashboard   Active   15d
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  config  set-context  --current --namespace=ashu-space 
Context "kubernetes-admin@kubernetes" modified.
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl   get  po
No resources found in ashu-space namespace.
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl   get  deploy 
No resources found in ashu-space namespace.
```

### Checking current namespace 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  config  get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 


```


### task1 solution 

```
691  kubectl  run  ashupod1  --image=busybox --command ping fb.com  --dry-run=client -oyaml >task1.yaml 
  692  kubectl  create -f  task1.yaml 
  693  kubectl  get  pod 
  694  kubectl logs  ashupod1 
  695  kubectl logs  ashupod1   >logs.txt 
  696  ls
  697  hitsor
  698  history 
[ashu@ip-172-31-29-207 k8s-app-deploy]$ ls
ashu_deployment.yaml  ashupod1.yaml  day8_deployment.yaml  day8_lbsvc.yam  day8_nodeport.yaml  logs.txt  newpod.yaml  nodeport.yaml  ns.yaml  task1.yaml
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  exec -it ashupod1 -- sh 
/ # ls /
bin    dev    etc    home   lib    lib64  proc   root   sys    tmp    usr    var
/ # mkdir /opt
/ # ls 
bin    dev    etc    home   lib    lib64  opt    proc   root   sys    tmp    usr    var
/ # exit
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl cp logs.txt ashupod1:/opt/
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  exec -it ashupod1 -- ls /opt
logs.txt
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get po ashupod1 -o wide
NAME       READY   STATUS    RESTARTS   AGE   IP                NODE    NOMINATED NODE   READINESS GATES
ashupod1   1/1     Running   0          97s   192.168.166.143   node1   <none>           <none>
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  exec -it ashupod1 -- sh 
/ # 
/ # cd /opt/
/opt # ls
logs.txt
/opt # echo "node1" >>logs.txt 
/opt # exit
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 
```

### Pod scaling 

<img src="pods.png">

### Deployment -- create pods -- we are putting limits in POd 

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
        resources: 
          requests:
            memory: 100M
            cpu: 100m # 100 milicore 
          limits:
            memory: 400M
            cpu: 200m 
        env: # to create or pass Env variable value 
        - name: deploy  # name of Env 
          value: ui3 # value of ENv--values ui1 , ui2 , ui3 
status: {}

```

### apply changes 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  create -f day8_deployment.yaml 
deployment.apps/ashu-ui-app created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   1/1     1            1           4s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl apply  -f day8_deployment.yaml 
Warning: resource deployments/ashu-ui-app is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
deployment.apps/ashu-ui-app configured
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  po 
NAME                           READY   STATUS        RESTARTS   AGE
ashu-ui-app-5dd986f45c-glrd9   1/1     Running       0          4s
ashu-ui-app-7f96876cbf-2b5jp   1/1     Terminating   0          38s
```

### if some monitoring / metric server is running then we can check the pod & node resource consumption 

```
ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  top 
Display Resource (CPU/Memory) usage.

 The top command allows you to see the resource consumption for nodes or pods.

 This command requires Metrics Server to be correctly configured and working on the server.

Available Commands:
  node          Display resource (CPU/memory) usage of nodes
  pod           Display resource (CPU/memory) usage of pods

Usage:
  kubectl top [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  top pods
NAME                           CPU(cores)   MEMORY(bytes)   
ashu-ui-app-5dd986f45c-glrd9   1m           17Mi            
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  top nodes
NAME            CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
control-plane   177m         4%     3480Mi          22%       
node1           75m          3%     651Mi           17%       
node3           67m          3%     748Mi           19% 
```

### manual horizental scaling of pods 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   1/1     1            1           5m11s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  scale  deployment  ashu-ui-app --replicas=5
deployment.apps/ashu-ui-app scaled
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   1/5     5            1           5m26s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   5/5     5            5           5m32s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get po -owide 
NAME                           READY   STATUS    RESTARTS   AGE    IP                NODE    NOMINATED NODE   READINESS GATES
ashu-ui-app-5dd986f45c-glrd9   1/1     Running   0          5m4s   192.168.135.2     node3   <none>           <none>
ashu-ui-app-5dd986f45c-tt4c5   1/1     Running   0          14s    192.168.166.157   node1   <none>           <none>
ashu-ui-app-5dd986f45c-wzrw2   1/1     Running   0          14s    192.168.135.6     node3   <none>           <none>
ashu-ui-app-5dd986f45c-zs4vx   1/1     Running   0          14s    192.168.166.155   node1   <none>           <none>
ashu-ui-app-5dd986f45c-zwm4d   1/1     Running   0          14s    192.168.166.151   node1   <none>           <none>
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 


```

### Introduction HPA in k8s 

<img src="hpa.png">

## k8s api-resources 

<img src="apis.png">

### Hpa implementations 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl   get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   1/1     1            1           31m
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  autoscale deployment  ashu-ui-app --min=3 --max=30 --cpu-percent=70  --dry-run=client -o yaml 
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  creationTimestamp: null
  name: ashu-ui-app
spec:
  maxReplicas: 30
  minReplicas: 3
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ashu-ui-app
  targetCPUUtilizationPercentage: 70
status:
  currentReplicas: 0
  desiredReplicas: 0
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  autoscale deployment  ashu-ui-app --min=3 --max=30 --cpu-percent=70  --dry-run=client -o yaml >hpa.yaml
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl apply -f hpa.yaml 
horizontalpodautoscaler.autoscaling/ashu-ui-app created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get hpa
NAME          REFERENCE                TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
ashu-ui-app   Deployment/ashu-ui-app   <unknown>/70%   3         30        0          3s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   1/1     1            1           33m
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  deploy 
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
ashu-ui-app   3/3     3            3           33m
```






