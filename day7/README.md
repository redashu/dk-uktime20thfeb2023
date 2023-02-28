## getting started

### Revision 

<img src="rev.png">

### deleting all the pods 

```
ashu@ip-172-31-29-207 ashu-app-images]$ kubectl  delete pods --all
pod "amr-pod002" deleted
pod "ihor-pod002" deleted
pod "mfarag-pod002" deleted
pod "rjamaro-pod002" deleted
pod "rjamaro-pod123" deleted
```

### k8s gonna use CNI for container networking which is running under Pod Env 

<img src="net1.png">

### CNI bridge on k8s 

<img src="cni1.png">

### cni vs cnm 

<img src="net2.png">

### choosing project calico as CNI implementation 
### we can choose CNI while k8s installation or after k8s installation as well
### we can have more than one CNI plugin also to get more networking support or features 

### deploy pod and check IP address by CNI 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ ls
ashupod1.yaml  newpod.yaml
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  create -f  newpod.yaml 
pod/ashu-pod002 created
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  pods
NAME          READY   STATUS    RESTARTS   AGE
ashu-pod002   1/1     Running   0          4s
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  get  pods -o wide
NAME          READY   STATUS    RESTARTS   AGE   IP                NODE    NOMINATED NODE   READINESS GATES
ashu-pod002   1/1     Running   0          8s    192.168.166.159   node1   <none>           <none>
[ashu@ip-172-31-29-207 k8s-app-deploy]$ 


```

### checking the name of CNI plugin 

```
[ashu@ip-172-31-29-207 k8s-app-deploy]$ kubectl  describe  pod  ashu-pod002 
Name:             ashu-pod002
Namespace:        default
Priority:         0
Service Account:  default
Node:             node1/172.31.92.45
Start Time:       Tue, 28 Feb 2023 09:12:55 +0000
Labels:           run=ashu-pod002
Annotations:      cni.projectcalico.org/containerID: 3dbffde54d2e4eb0f2402a2dbd0cb1da33746e2632dd9b4e03a5104c7baf2af4
                  cni.projectcalico.org/podIP: 192.168.166.159/32
                  cni.projectcalico.org/podIPs: 192.168.166.159/32
Status:           Running
IP:               192.168.166.159
IPs:
  IP:  192.168.166.159
Containers:
  ashu-pod002:
    Container ID:   docker://0e064b94c15b60c51082697a4b1fce3baedb774ddfc0ed4d919f7ba965ebc517
    Image:          docker.io/dockerashu/ashu-ui:mobiv1
    Image ID:       docker-pullable://dockerashu/ashu-ui@sha256:e6e1111eddfee0b1ca40d72d04e07f7f0c58153ee448575272572b0be81c1e68
    Port:           80/TCP
    Host Port:      0/TCP
```

### INtroduction to service Resources in k8s 

<img src="svc.png">

### more understanding of service 

<img src="svc1.png">

