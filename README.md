# Домашнее задание к занятию «Управление доступом»

<br>

## Задание 1.

Демонстрация работы допусков созданного пользователя на ооснове выданных прав

```
kubectl get pods -o wide
NAME                                  READY   STATUS    RESTARTS       AGE     IP              NODE       NOMINATED NODE   READINESS GATES
csi-nfs-controller-7d8fcf9fd9-qsh6b   4/4     Running   75 (49m ago)   18d     192.168.1.103   workbook   <none>           <none>
my-nginx-bcfc6fdbd-96926              1/1     Running   4 (49m ago)    5d21h   10.1.211.215    workbook   <none>           <none>
nginx-multitool-7cd7457944-c9pqr      2/2     Running   8 (49m ago)    5d22h   10.1.211.208    workbook   <none>           <none>
ditry@workbook:~/kuber-homeworks-2.4$ kubectl get ns
Error from server (Forbidden): namespaces is forbidden: User "ditry" cannot list resource "namespaces" in API group "" at the cluster scope
ditry@workbook:~/kuber-homeworks-2.4$ kubectl logs pods/nginx-multitool-7cd7457944-c9pqr 
Defaulted container "multitool" out of: multitool, nginx, init-myservice (init)
The directory /usr/share/nginx/html is not mounted.
Therefore, over-writing the default index.html file with some useful information:
WBITT Network MultiTool (with NGINX) - nginx-multitool-7cd7457944-c9pqr - 10.1.211.208 - HTTP: 1180 , HTTPS: 11443 . (Formerly praqma/network-multitool)
Replacing default HTTP port (80) with the value specified by the user - (HTTP_PORT: 1180).
Replacing default HTTPS port (443) with the value specified by the user - (HTTPS_PORT: 11443).
ditry@workbook:~/kuber-homeworks-2.4$ kubectl describe pods/nginx-multitool-7cd7457944-c9pqr 
Name:             nginx-multitool-7cd7457944-c9pqr
Namespace:        default
Priority:         0
Service Account:  default
Node:             workbook/192.168.1.103
Start Time:       Tue, 05 Mar 2024 23:00:36 +0900
Labels:           app=nginx-multitool
                  pod-template-hash=7cd7457944
Annotations:      cni.projectcalico.org/containerID: aa962b0f45ca9d8a684855010690c5d605e9e9e19736303d1d6a0dcaf9ed7e91
                  cni.projectcalico.org/podIP: 10.1.211.208/32
                  cni.projectcalico.org/podIPs: 10.1.211.208/32
Status:           Running
IP:               10.1.211.208
IPs:
  IP:           10.1.211.208
Controlled By:  ReplicaSet/nginx-multitool-7cd7457944
Init Containers:
  init-myservice:
    Container ID:  containerd://df1f3e07af5bd3ba0a15e8d9ee705bf0ca3ca0943aee3a923ee71ed2e15f2c41
    Image:         nginx
    Image ID:      docker.io/library/nginx@sha256:c26ae7472d624ba1fafd296e73cecc4f93f853088e6a9c13c0d52f6ca5865107
    Port:          <none>
    Host Port:     <none>
    Command:
      bash
      -c
      mkdir -p /var/data
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Mon, 11 Mar 2024 20:24:16 +0900
      Finished:     Mon, 11 Mar 2024 20:24:16 +0900
    Ready:          True
    Restart Count:  4
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-jgs5f (ro)
Containers:
  multitool:
    Container ID:   containerd://5e11fbacc5e5844849bb5d15b66b91c6f7916ed9a57bfc617958e28c02610481
    Image:          wbitt/network-multitool
    Image ID:       docker.io/wbitt/network-multitool@sha256:d1137e87af76ee15cd0b3d4c7e2fcd111ffbd510ccd0af076fc98dddfc50a735
    Ports:          1180/TCP, 11443/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Running
      Started:      Mon, 11 Mar 2024 20:24:19 +0900
    Last State:     Terminated
      Reason:       Unknown
      Exit Code:    255
      Started:      Sun, 10 Mar 2024 17:13:46 +0900
      Finished:     Mon, 11 Mar 2024 20:23:04 +0900
    Ready:          True
    Restart Count:  4
    Requests:
      cpu:     1m
      memory:  20Mi
    Environment:
      HTTP_PORT:   1180
      HTTPS_PORT:  11443
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-jgs5f (ro)
  nginx:
    Container ID:   containerd://23c13fea1040a1d7a9522dd63b09dcf423a1fa3d91fe14b814505ad1826f069e
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:c26ae7472d624ba1fafd296e73cecc4f93f853088e6a9c13c0d52f6ca5865107
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 11 Mar 2024 20:24:26 +0900
    Last State:     Terminated
      Reason:       Unknown
      Exit Code:    255
      Started:      Sun, 10 Mar 2024 17:13:30 +0900
      Finished:     Mon, 11 Mar 2024 20:23:04 +0900
    Ready:          True
    Restart Count:  4
    Environment:    <none>
    Mounts:
      /etc/nginx/nginx.conf from nginx-config (rw,path="nginx.conf")
      /var/data/index.html from nginx-config (rw,path="index.html")
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-jgs5f (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  nginx-config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      nginx-conf
    Optional:  false
  kube-api-access-jgs5f:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:                      <none>
```