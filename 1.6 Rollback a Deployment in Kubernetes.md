# This morning the Nautilus DevOps team rolled out a new release for one of the applications. Recently one of the customers logged a complaint which seems to be about a bug related to the recent release. Therefore, the team wants to rollback the recent release.


There is a deployment named nginx-deployment; roll it back to the previous revision.

```ruby
thor@jump_host ~$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment nginx-deployment nginx-container=nginx:stable --kubeconfig=/root/.kube/config --record=true

thor@jump_host ~$ kubectl rollout undo deployment/nginx-deployment --to-revision=<REVISION>
bash: syntax error near unexpected token `newline'

thor@jump_host ~$ kubectl rollout undo deployment/nginx-deployment --to-revision=2
deployment.apps/nginx-deployment skipped rollback (current template already matches revision 2)


thor@jump_host ~$ kubectl rollout undo deployment/nginx-deployment --to-revision=1
deployment.apps/nginx-deployment rolled back


thor@jump_host ~$ kubectl rollout undo deployment/nginx-deployment --to-revision=2
deployment.apps/nginx-deployment rolled back


thor@jump_host ~$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
3         <none>
4         kubectl set image deployment nginx-deployment nginx-container=nginx:stable --kubeconfig=/root/.kube/config --record=true

thor@jump_host ~$ kubectl rollout undo deployment/nginx-deployment --to-revision=3
deployment.apps/nginx-deployment rolled back
thor@jump_host ~$ 


```



To roll back a deployment to a previous revision in Kubernetes, you can use the `kubectl rollout undo` command. Here are the steps to roll back the `nginx-deployment` to the previous revision:

1. **View Deployment Revisions:**

   First, you need to list the available revisions of the deployment to determine which revision you want to roll back to. You can use the following command to view the deployment revisions:

   ```bash
   kubectl rollout history deployment/nginx-deployment
   ```

   This command will show you a list of revisions along with their details.

2. **Roll Back to a Specific Revision:**

   To roll back to a specific revision, you can use the `kubectl rollout undo` command. Replace `<REVISION>` with the revision number you want to roll back to.

   ```bash
   kubectl rollout undo deployment/nginx-deployment --to-revision=<REVISION>
   ```

   For example, if you want to roll back to revision 2, the command would be:

   ```bash
   kubectl rollout undo deployment/nginx-deployment --to-revision=2
   ```

3. **Monitor the Rollback:**

   You can monitor the rollback process using the `kubectl rollout status` command:

   ```bash
   kubectl rollout status deployment/nginx-deployment
   ```

   This command will show the status of the deployment as it progresses.

After executing these steps, your `nginx-deployment` will be rolled back to the specified revision, effectively undoing the changes made in the later revision. Make sure to choose the revision number that corresponds to the state you want to revert to.
