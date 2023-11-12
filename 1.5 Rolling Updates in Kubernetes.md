# We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image nginx:1.17 with the latest changes.


Perform a rolling update for this application and incorporate nginx:1.17 image. The deployment name is nginx-deployment

Make sure all pods are up and running after the update.

```ruby
thor@jump_host ~$ ls
thor@jump_host ~$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-989f57c54-9n6cn   1/1     Running   0          79s
nginx-deployment-989f57c54-gl4sv   1/1     Running   0          79s
nginx-deployment-989f57c54-rsqsk   1/1     Running   0          79s




thor@jump_host ~$ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           116s
thor@jump_host ~$ kubectl get svc
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        24m
nginx-service   NodePort    10.96.154.183   <none>        80:30008/TCP   2m7s





thor@jump_host ~$ kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17
deployment.apps/nginx-deployment image updated
thor@jump_host ~$ kubectl get pods -w
NAME                                READY   STATUS              RESTARTS   AGE
nginx-deployment-5dd558cf95-44bqd   1/1     Running             0          5s
nginx-deployment-5dd558cf95-fsw6d   0/1     ContainerCreating   0          3s
nginx-deployment-5dd558cf95-s7vbd   1/1     Running             0          17s
nginx-deployment-989f57c54-rsqsk    1/1     Running             0          6m6s
nginx-deployment-5dd558cf95-fsw6d   1/1     Running             0          3s
nginx-deployment-989f57c54-rsqsk    1/1     Terminating         0          6m6s
nginx-deployment-989f57c54-rsqsk    0/1     Terminating         0          6m6s
nginx-deployment-989f57c54-rsqsk    0/1     Terminating         0          6m7s
nginx-deployment-989f57c54-rsqsk    0/1     Terminating         0          6m7s
nginx-deployment-989f57c54-rsqsk    0/1     Terminating         0          6m7s

 
thor@jump_host ~$ 
thor@jump_host ~$ kubectl rollout status deployment/nginx-deployment
deployment "nginx-deployment" successfully rolled out
thor@jump_host ~$ 
```



Certainly! The `kubectl set image` command is used to update the image of a container within a Kubernetes deployment. Let's break down the command:

- `kubectl`: This is the command-line tool for interacting with a Kubernetes cluster.

- `set image`: This is a specific subcommand of `kubectl` used for updating images in a deployment.

- `deployment/nginx-deployment`: This part specifies the deployment you want to update. `nginx-deployment` is the name of the deployment in this example. A deployment manages a set of identical pods, ensuring that a specified number of them are running at any given time.

- `nginx-container`: This is the name of the container within the deployment that you want to update. Containers are the individual units that run applications within pods.

- `=nginx:1.17`: Here, you specify the new image that you want to use for the container. In this case, it's `nginx:1.17`. This means you are instructing Kubernetes to replace the existing image in the specified container with the `nginx:1.17` image.

So, when you run the command `kubectl set image deployment/nginx-deployment nginx-container=nginx:1.17`, you are telling Kubernetes to update the image of the `nginx-container` within the `nginx-deployment` to `nginx:1.17`. Kubernetes will then initiate a rolling update, gradually replacing the pods with the new image to ensure minimal downtime while applying the changes to your application.
