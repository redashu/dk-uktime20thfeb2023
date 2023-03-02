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

