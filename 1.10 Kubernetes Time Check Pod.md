# The Nautilus DevOps team want to create a time check pod in a particular Kubernetes namespace and record the logs. This might be initially used only for testing purposes, but later can be implemented in an existing cluster. Please find more details below about the task and perform it.


Create a pod called time-check in the xfusion namespace. This pod should run a container called time-check, container should use the busybox image with latest tag only and remember to mention tag i.e busybox:latest.

Create a config map called time-config with the data TIME_FREQ=12 in the same namespace.

The time-check container should run the command: while true; do date; sleep $TIME_FREQ;done and should write the result to the location /opt/sysops/time/time-check.log. Remember you will also need to add an environmental variable TIME_FREQ in the container, which should pick value from the config map TIME_FREQ key.

Create a volume log-volume and mount the same on /opt/sysops/time within the container.

### attempt 1
```ruby
thor@jump_host ~$ vi time-check-pod.yaml 
thor@jump_host ~$ cat time-check-pod.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: xfusion
spec:
  containers:
  - name: time-check
    image: busybox:latest
    command:
    - sh
    - -c
    - "while true; do date; sleep $TIME_FREQ; done >> /opt/sysops/time/time-check.log"
    env:
    - name: TIME_FREQ
      valueFrom:
        configMapKeyRef:
          name: time-config
          key: TIME_FREQ
    volumeMounts:
    - name: log-volume
      mountPath: /opt/sysops/time
  volumes:
  - name: log-volume
    emptyDir: {}

thor@jump_host ~$ kubectl create -f time-check-pod.yaml
Error from server (NotFound): error when creating "time-check-pod.yaml": namespaces "xfusion" not found
thor@jump_host ~$ touch xfusion-namespace.yaml
thor@jump_host ~$ vi xfusion-namespace.yaml 
thor@jump_host ~$ kubectl create -f xfusion-namespace.yaml
namespace/xfusion created
thor@jump_host ~$ kubectl create -f time-check-pod.yaml
pod/time-check created
thor@jump_host ~$ kubectl get pod,svc,deploy,job,namespace
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   45m

NAME                           STATUS   AGE
namespace/default              Active   45m
namespace/kube-node-lease      Active   45m
namespace/kube-public          Active   45m
namespace/kube-system          Active   45m
namespace/local-path-storage   Active   45m
namespace/xfusion              Active   57s
thor@jump_host ~$ 
```

![image](https://github.com/Althaf-official/KodeKloud_K8s_SysAdmin_level1/assets/105126131/9568e659-0a33-4d41-9a45-dd59078d17b3)


### attempt 2

```ruby
thor@jump_host ~$ kubectl get pod,svc,deploy,namespace
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   32m

NAME                           STATUS   AGE
namespace/default              Active   32m
namespace/kube-node-lease      Active   32m
namespace/kube-public          Active   32m
namespace/kube-system          Active   32m
namespace/local-path-storage   Active   32m
thor@jump_host ~$ kubectl create namespace devops
namespace/devops created
thor@jump_host ~$ vi /tmp/time.yaml
thor@jump_host ~$ cat /tmp/time.yaml
apiVersion: v1

kind: ConfigMap

metadata:

  name: time-config

  namespace: devops

data:

  TIME_FREQ: "9"

---

apiVersion: v1

kind: Pod

metadata:

  name: time-check

  namespace: devops

  labels:

    app: time-check

spec:

  volumes:

    - name: log-volume

      emptyDir: {}

  containers:

    - name: time-check

      image: busybox:latest

      volumeMounts:

        - mountPath: /opt/security/time

          name: log-volume

      envFrom:

        - configMapRef:

            name: time-config

      command: ["/bin/sh", "-c"]

      args:

        [

          "while true; do date; sleep $TIME_FREQ;done > /opt/security/time/time-check.log",

        ]
thor@jump_host ~$ kubectl create -f /tmp/time.yaml
configmap/time-config created
pod/time-check created
thor@jump_host ~$ 
thor@jump_host ~$ kubectl get pods -n devops
NAME         READY   STATUS    RESTARTS   AGE
time-check   1/1     Running   0          29s
thor@jump_host ~$ 
```
![image](https://github.com/Althaf-official/KodeKloud_K8s_SysAdmin_level1/assets/105126131/3f118c52-460c-4284-a6e1-4b8893f0323b)
https://www.nbtechsupport.co.in/2021/04/kubernetes-time-check-pod.html
