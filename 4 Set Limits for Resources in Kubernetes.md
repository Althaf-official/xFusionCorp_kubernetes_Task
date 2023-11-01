# Recently some of the performance issues were observed with some applications hosted on Kubernetes cluster. The Nautilus DevOps team has observed some resources constraints, where some of the applications are running out of resources like memory, cpu etc., and some of the applications are consuming more resources than needed. Therefore, the team has decided to add some limits for resources utilization. Below you can find more details.


Create a pod named httpd-pod and a container under it named as httpd-container, use httpd image with latest tag only and remember to mention tag i.e httpd:latest and set the following limits:

Requests: Memory: 15Mi, CPU: 100m

Limits: Memory: 20Mi, CPU: 100m


```ruby
thor@jump_host ~$ ls


thor@jump_host ~$ cat > httmpd-pod-definition.yaml




apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    resources:
      limits:
        memory: "20Mi"
        cpu: "100m"
      requests:
        memory: "15Mi"
        cpu: "100m"




thor@jump_host ~$ ls
httmpd-pod-definition.yaml



thor@jump_host ~$ kubectl create -f httmpd-pod-definition.yaml 
pod/httpd-pod created




thor@jump_host ~$ kubectl get pod
NAME        READY   STATUS              RESTARTS   AGE
httpd-pod   0/1     ContainerCreating   0          10s
thor@jump_host ~$ kubectl get pod
NAME        READY   STATUS    RESTARTS   AGE
httpd-pod   1/1     Running   0          17s





thor@jump_host ~$ kubectl describe pod httpd-pod 
Name:             httpd-pod
Namespace:        default
Priority:         0
Service Account:  default
Node:             kodekloud-control-plane/172.17.0.2
Start Time:       Wed, 01 Nov 2023 02:32:35 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  httpd-container:
    Container ID:   containerd://a2e9371deb3e9bf3310a073ff000670c99f077cb4004594cdbc0ba40b44a37d8
    Image:          httpd:latest
    Image ID:       docker.io/library/httpd@sha256:25ca9b3fea146bf21727c3397b33746edc86d5d760d2f04aa60cf901e773d8bd
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Wed, 01 Nov 2023 02:32:48 +0000
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     100m
      memory:  20Mi
    Requests:
      cpu:        100m
      memory:     15Mi
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-ppj5k (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-ppj5k:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  45s   default-scheduler  Successfully assigned default/httpd-pod to kodekloud-control-plane
  Normal  Pulling    44s   kubelet            Pulling image "httpd:latest"
  Normal  Pulled     33s   kubelet            Successfully pulled image "httpd:latest" in 11.039698318s (11.039721074s including waiting)
  Normal  Created    33s   kubelet            Created container httpd-container
  Normal  Started    32s   kubelet            Started container httpd-container
thor@jump_host ~$ 
```
