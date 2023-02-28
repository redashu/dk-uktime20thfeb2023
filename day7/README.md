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


