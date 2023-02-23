### Revision 

<img src="rev.png">

### Implementing None Network in container 

```
[ashu@docker-host ashu-app-images]$ docker run -it  --network none  alpine  sh 
/ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
/ # ifconfig 
lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ # ping 172.17.0.1
PING 172.17.0.1 (172.17.0.1): 56 data bytes
ping: sendto: Network unreachable
/ # ping google.com
ping: bad address 'google.com'
/ # exit
```

### creating custom bridge network 

```
[ashu@docker-host ashu-app-images]$ docker  network create   ashubr1 
0ee019b89bb34656674858b1bef77e4c610e07ebc07a6fb39fe3fd143d154c82
[ashu@docker-host ashu-app-images]$ docker  network ls
NETWORK ID     NAME      DRIVER    SCOPE
0ee019b89bb3   ashubr1   bridge    local
38639e995d11   bridge    bridge    local
3372299e537a   host      host      local
dbfa091ea6fa   none      null      local
[ashu@docker-host ashu-app-images]$ docker  network inspect ashubr1
[
    {
        "Name": "ashubr1",
        "Id": "0ee019b89bb34656674858b1bef77e4c610e07ebc07a6fb39fe3fd143d154c82",
        "Created": "2023-02-23T08:46:41.924651537Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
```

### creating containers in network bridge 

```
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc1 --network ashubr1  alpine  sleep 1000 
a11f2a165c7fd6b20346ccf87c88088e1d37a171e67ace8d7a25798e0f3edb05
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc2 --network ashubr1  alpine  sleep 1000 
592317bd183866eee7183b197c2f264a2c499aef81b5c670a2777948afd6229f
[ashu@docker-host ashu-app-images]$ docker network inspect ashubr1
[
    {
        "Name": "ashubr1",
        "Id": "0ee019b89bb34656674858b1bef77e4c610e07ebc07a6fb39fe3fd143d154c82",
        "Created": "2023-02-23T08:46:41.924651537Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "592317bd183866eee7183b197c2f264a2c499aef81b5c670a2777948afd6229f": {
                "Name": "ashuc2",
                "EndpointID": "c8613a2c741c33cac3a464e53b0b40e9aed573bc04f00ac46c2f606c20385543",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "a11f2a165c7fd6b20346ccf87c88088e1d37a171e67ace8d7a25798e0f3edb05": {
                "Name": "ashuc1",
                "EndpointID": "7f8a548f13d6ecd3bc867ec0b05ececc2b9b06e90c6f4a3a2553b3ff914f09bf",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

### testing connections

```
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ docker  exec -it ashuc1 sh 
/ # 
/ # ping ashuc2
PING ashuc2 (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.094 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.086 ms
64 bytes from 172.18.0.3: seq=2 ttl=64 time=0.078 ms
^C
--- ashuc2 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.078/0.086/0.094 ms
/ # exit
[ashu@docker-host ashu-app-images]$ docker  ps
CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS     NAMES
51ee7b230e2e   alpine    "sleep 1000"   30 seconds ago   Up 30 seconds             abdoc2
eefd05fb6d6e   alpine    "sleep 1000"   32 seconds ago   Up 31 seconds             sudarshantc2
8e8b4752b5bc   alpine    "sleep 1000"   38 seconds ago   Up 37 seconds             abdoc1
c855a9a48f8d   alpine    "sleep 1000"   38 seconds ago   Up 37 seconds             sudarshantc1
458415d4c0a4   busybox   "sh"           41 seconds ago   Up 41 seconds             rjamaro-c1
592317bd1838   alpine    "sleep 1000"   2 minutes ago    Up 2 minutes              ashuc2
a11f2a165c7f   alpine    "sleep 1000"   2 minutes ago    Up 2 minutes              ashuc1
[ashu@docker-host ashu-app-images]$ docker  exec -it ashuc1 sh 
/ # 
/ # ping abdoc2
ping: bad address 'abdoc2'
/ # exit
[ashu@docker-host ashu-app-images]$ 
```

### creating specific subnet network bridge 

```
[ashu@docker-host ashu-app-images]$ docker network create  ashubr2  --subnet  192.168.100.0/24  --gateway 192.168.100.1 
```

### creating container network with details 

```
[ashu@docker-host ashu-app-images]$ docker network create ashubr1
fc2963a9935259c30d912ec74a0542ce780cae867ef8ca229dc6d0a7b2749bc5
[ashu@docker-host ashu-app-images]$ docker network create ashubr2 --subnet 192.168.179.0/24  --gateway  192.168.179.1 
242d44c6f461c0fa5595fd9968fe6282b24ef2a4e5b771b73c33fa7054eb8d0c
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc1 --network ashubr1  alpine sleep 1000
c89ce796c1afefa13ce920a222eb8b2733c33b75eaf7e66da12a3897edb8174f
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc2 --network ashubr1  alpine sleep 1000
6db59091bb30b17d672a302bc2034348de061ed6fa218616dae9cccb42b5f867
[ashu@docker-host ashu-app-images]$ 
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc3 --network ashubr2  alpine sleep 1000
71c7473796f57465d30928fbc2897b4a2fb1e46afb57b5d78aa12d7b972d4926
[ashu@docker-host ashu-app-images]$ docker run -itd --name ashuc4 --network ashubr2 --ip 192.168.179.200  alpine sleep 1000
2bc6e7e84986cd523725b19d258dc0a8fdf73274edbd64c3405a5e61d8e29a8b
[ashu@docker-host ashu-app-images]$ docker  exec -it  ashuc4  sh 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:C0:A8:B3:C8  
          inet addr:192.168.179.200  Bcast:192.168.179.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:7 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:570 (570.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ # exit
[ashu@docker-host ashu-app-images]$ 
```

### adding multiple interface to the network container 

```
[ashu@docker-host ashu-app-images]$ docker  exec -it  ashuc1  sh 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:C0:A8:50:02  
          inet addr:192.168.80.2  Bcast:192.168.95.255  Mask:255.255.240.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:24 errors:0 dropped:0 overruns:0 frame:0
          TX packets:9 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2120 (2.0 KiB)  TX bytes:530 (530.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:946 (946.0 B)  TX bytes:946 (946.0 B)

/ # 
[ashu@docker-host ashu-app-images]$ docker network connect  ashubr2  ashuc1 
[ashu@docker-host ashu-app-images]$ docker  exec -it  ashuc1  sh 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:C0:A8:50:02  
          inet addr:192.168.80.2  Bcast:192.168.95.255  Mask:255.255.240.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:24 errors:0 dropped:0 overruns:0 frame:0
          TX packets:9 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2120 (2.0 KiB)  TX bytes:530 (530.0 B)

eth1      Link encap:Ethernet  HWaddr 02:42:C0:A8:B3:03  
          inet addr:192.168.179.3  Bcast:192.168.179.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:500 (500.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:946 (946.0 B)  TX bytes:946 (946.0 B)

/ # 
[ashu@docker-host ashu-app-images]$ docker  exec -it  ashuc1  sh 
/ # ping ashuc4
PING ashuc4 (192.168.179.200): 56 data bytes
64 bytes from 192.168.179.200: seq=0 ttl=64 time=0.111 ms
64 bytes from 192.168.179.200: seq=1 ttl=64 time=0.089 ms
^C
--- ashuc4 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.089/0.100/0.111 ms
/ # 
```



