# The Nautilus DevOps team is going to deploy some applications on kubernetes cluster as they are planning to migrate some of their existing applications there. Recently one of the team members has been assigned a task to write a template as per details mentioned below:



Create a ReplicaSet using httpd image with latest tag only and remember to mention tag i.e httpd:latest and name it as httpd-replicaset.


Labels app should be httpd_app, labels type should be front-end.


The container should be named as httpd-container; also make sure replicas counts are 4.


### attempt 1

```ruby
thor@jump_host ~$ 
thor@jump_host ~$ cat > httpd-replicaset.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd_app
      type: front-end
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
thor@jump_host ~$ kubectl get nodes
NAME                      STATUS   ROLES           AGE   VERSION
kodekloud-control-plane   Ready    control-plane   14m   v1.27.3-44+b5c876a05b7bbd
thor@jump_host ~$ kubectl get nod
error: the server doesn't have a resource type "nod"
thor@jump_host ~$ kubectl get pd
error: the server doesn't have a resource type "pd"
thor@jump_host ~$ kubectl get pod
No resources found in default namespace.
thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.
thor@jump_host ~$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   14m
thor@jump_host ~$ 
thor@jump_host ~$ 
thor@jump_host ~$ 
thor@jump_host ~$ ls
httpd-replicaset.yaml
thor@jump_host ~$ cat httpd-replicaset.yaml 
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd_app
      type: front-end
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
thor@jump_host ~$ kubectl apply -f httpd-replicaset.yaml
replicaset.apps/httpd-replicaset created
thor@jump_host ~$ kubectl get pod
NAME                     READY   STATUS              RESTARTS   AGE
httpd-replicaset-26wkr   0/1     ContainerCreating   0          8s
httpd-replicaset-974bl   0/1     ContainerCreating   0          8s
httpd-replicaset-l2p9n   0/1     ContainerCreating   0          8s
httpd-replicaset-p8j9x   0/1     ContainerCreating   0          8s
thor@jump_host ~$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   15m
thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.
thor@jump_host ~$ 
```

![image](https://github.com/Althaf-official/KodeKloud_K8s_SysAdmin_level1/assets/105126131/a46ed770-5181-4b9b-97e3-d60cec1d24fd)


```ruby
thor@jump_host ~$ cat >  httpd.yaml
apiVersion: apps/v1

kind: ReplicaSet

metadata:

  name: httpd-replicaset

  labels:

    app: httpd_app

    type: front-end

spec:

  replicas: 4

  selector:

    matchLabels:

      app: httpd_app

  template:

    metadata:

      labels:

        app: httpd_app

        type: front-end

    spec:

      containers:

        - name: httpd-container

          image: httpd:latestthor@jump_host ~$ 
thor@jump_host ~$ cat httpd.yaml 
apiVersion: apps/v1

kind: ReplicaSet

metadata:

  name: httpd-replicaset

  labels:

    app: httpd_app

    type: front-end

spec:

  replicas: 4

  selector:

    matchLabels:

      app: httpd_app

  template:

    metadata:

      labels:

        app: httpd_app

        type: front-end

    spec:

      containers:

        - name: httpd-container

          image: httpd:latestthor@jump_host ~$ kubectl create -f httpd.yaml 
replicaset.apps/httpd-replicaset created
thor@jump_host ~$ 
```
Thank you for providing the corrected YAML configuration for the ReplicaSet. The YAML you posted is indeed correct and will create a ReplicaSet named "httpd-replicaset" with the desired labels and settings.

Here's a breakdown of the YAML configuration:

- The `metadata` section specifies the name of the ReplicaSet and labels the ReplicaSet with "app: httpd_app" and "type: front-end."

- The `spec` section defines that the ReplicaSet should have 4 replicas and that it should use the `template` to create the pods.

- In the `template` section, the `metadata.labels` section ensures that the pods created by the ReplicaSet have the same labels as the ReplicaSet itself, setting "app" to "httpd_app" and "type" to "front-end."

- Finally, the `containers` section within `template.spec` defines a container named "httpd-container" that runs the "httpd:latest" image.

This YAML configuration will create a ReplicaSet with the specified settings and labels. Thank you for your patience and for providing the correct YAML.
