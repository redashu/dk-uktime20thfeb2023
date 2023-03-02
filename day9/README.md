## Lab connect 

### checking 

```
[ashu@ip-172-31-29-207 ashu-app-images]$ kubectl  get  nodes
NAME            STATUS   ROLES                  AGE   VERSION
control-plane   Ready    control-plane,master   16d   v1.23.16
node1           Ready    <none>                 16d   v1.23.16
node3           Ready    <none>                 16d   v1.23.16
[ashu@ip-172-31-29-207 ashu-app-images]$ kubectl  config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ashu@ip-172-31-29-207 ashu-app-images]$ kubectl  get  ns
NAME                   STATUS   AGE
abdo-space             Active   22h
amr-space              Active   22h
ashu-space             Active   22h
default                Active   16d
hhalkhazragy-space     Active   22h
ihor-space             Active   22h
ingress-nginx          Active   15d
kube-node-lease        Active   16d
kube-public            Active   16d
kube-system            Active   16d
kubernetes-dashboard   Active   16d
mfarag-space           Active   22h
rjamaro                Active   22h
shamaa-space           Active   22h
```
### Introduction to Ingress controller

<img src="ingress.png">

### checking nginx ingress controller 

```
ashu@ip-172-31-29-207 ashu-app-images]$ kubectl  get  all -n ingress-nginx 
NAME                                           READY   STATUS    RESTARTS      AGE
pod/ingress-nginx-controller-bcd4c88f9-f2dwh   1/1     Running   3 (20h ago)   14d

NAME                                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
service/ingress-nginx-controller             NodePort    10.103.158.63    <none>        80:30279/TCP,443:30317/TCP   16d
service/ingress-nginx-controller-admission   ClusterIP   10.108.127.171   <none>        443/TCP                      16d

NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ingress-nginx-controller   1/1     1            1           16d

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/ingress-nginx-controller-bcd4c88f9   1         1         1       16d

NAME                                       COMPLETIONS   DURATION   AGE
job.batch/ingress-nginx-admission-create   1/1           5s         16d
job.batch/ingress-nginx-admission-patch    1/1           6s         16d
[ashu@ip-172-31-29-207 ashu-app-images]$ 


```



