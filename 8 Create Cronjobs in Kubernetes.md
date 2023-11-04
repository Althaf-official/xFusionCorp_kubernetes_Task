# There are some jobs/tasks that need to be run regularly on different schedules. Currently the Nautilus DevOps team is working on developing some scripts that will be executed on different schedules, but for the time being the team is creating some cron jobs in Kubernetes cluster with some dummy commands (which will be replaced by original scripts later). Create a cronjob as per details given below:



Create a cronjob named xfusion.


Set Its schedule to something like */12 * * * *, you set any schedule for now.


Container name should be cron-xfusion.


Use httpd image with latest tag only and remember to mention the tag i.e httpd:latest.


Run a dummy command echo Welcome to xfusioncorp!.


Ensure restart policy is OnFailure.


Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


```ruby
thor@jump_host ~$ cat > xfusion-cronjob.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: xfusion
spec:
  schedule: "*/12 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-xfusion
            image: httpd:latest
            command: ["echo", "Welcome to xfusioncorp!"]
          restartPolicy: OnFailure
thor@jump_host ~$ kubectl create -f xfusion-cronjob.yaml
cronjob.batch/xfusion created
thor@jump_host ~$ kubectl get pod
No resources found in default namespace.
thor@jump_host ~$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   28m
thor@jump_host ~$ kubectl get deploy
No resources found in default namespace.
thor@jump_host ~$ kubectl get cronjob
NAME      SCHEDULE       SUSPEND   ACTIVE   LAST SCHEDULE   AGE
xfusion   */12 * * * *   False     0        <none>          48s
thor@jump_host ~$ 

```
