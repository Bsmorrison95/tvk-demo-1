
# OpenShift Demo Commands

**Pre-demo:**
```
oc delete project demotarget
```
```
oc new-project demotarget
```
```
oc delete restore demo-restore
```
```
oc adm policy add-scc-to-user anyuid -z default -n demotarget
```


✅  **Show the pods deployed via the operator**

```
oc get pods -n openshift-operators | grep -i trilio
```
Output:
```
openshift-operators           k8s-triliovault-admission-webhook-85567b4d45-6qwft                1/1     Running     0          9d
openshift-operators           k8s-triliovault-backend-6744c7548b-nw4jb                          1/1     Running     0          9d
openshift-operators           k8s-triliovault-control-plane-96869db89-9q27x                     1/1     Running     0          9d
openshift-operators           k8s-triliovault-exporter-86889b758-vwn7v                          1/1     Running     0          9d
openshift-operators           k8s-triliovault-ingress-controller-689f9f45ff-q8r27               1/1     Running     0          9d
openshift-operators           k8s-triliovault-resource-cleaner-1611446400-glnlh                 0/1     Completed   0          110m
openshift-operators           k8s-triliovault-web-67c8dfb6d5-f8tp7                              1/1     Running     0          9d
```

Pods in the **openshift-operators** namespace 
  * Admission-webhook  - Recieves admission requests 
  * Backend			         - Backend API calls for the Web UI
  * Control-plane 	 	  - Controller of whole application, services requests of the API, kicks off schedules and cares for the application. 
  * Exporter 	         – Sends metrics of Trilio to Prometheous, Grafana, and FluentD
  * Ingress-controller	– Exposes the services for the TVK UI
  * Resource-cleaner  	– Cleans up all the meta mover and data mover pods which are expired after a backup is complete
  * Web 			            – The Web UI




✅  **Show the Custom Resource Definitions**

```
oc get crd | grep -i trilio
```
Output:
```
backupplans.triliovault.trilio.io                           2021-01-13T08:42:49Z
backups.triliovault.trilio.io                               2021-01-13T08:42:51Z
hooks.triliovault.trilio.io                                 2021-01-13T08:42:53Z
licenses.triliovault.trilio.io                              2021-01-13T08:42:55Z
policies.triliovault.trilio.io                              2021-01-13T08:42:45Z
restores.triliovault.trilio.io                              2021-01-13T08:42:47Z
targets.triliovault.trilio.io                               2021-01-13T08:42:48Z
```


✅  **Show demo-app objects**

```
oc project demosource
```
```
oc get all -n demosource -l app=k8s-demo-app
```
Output:
```
NAME                                        READY   STATUS    RESTARTS   AGE
pod/k8s-demo-app-frontend-7c4bdbf9b-dh6z4   1/1     Running   0          10d
pod/k8s-demo-app-frontend-7c4bdbf9b-gg4kt   1/1     Running   0          10d
pod/k8s-demo-app-frontend-7c4bdbf9b-njjs6   1/1     Running   0          10d
pod/k8s-demo-app-mysql-754f46dbd7-c9zx6     1/1     Running   1          10d

NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/k8s-demo-app-frontend   LoadBalancer   172.30.232.145   <pending>     80:30919/TCP   10d
service/k8s-demo-app-mysql      ClusterIP      None             <none>        3306/TCP       10d

NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/k8s-demo-app-frontend   3/3     3            3           10d
deployment.apps/k8s-demo-app-mysql      1/1     1            1           10d

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/k8s-demo-app-frontend-7c4bdbf9b   3         3         3       10d
replicaset.apps/k8s-demo-app-mysql-754f46dbd7     1         1         1       10d
```


```
cat k8s-demo-app.yaml
```



✅  **Show Target**
```
oc get target
```
```
cat target.yaml
```
Output:
```
NAME            TYPE   THRESHOLD CAPACITY   VENDOR   STATUS      BROWSING ENABLED
sample-target   NFS                         Other    Available   true
```


✅  **Show Backup Plans**

```
oc get backupplan
```
Output:
```
NAME                     COMPONENT NAMESPACE   TARGET          RETENTION POLICY   INCREMENTAL SCHEDULE   FULL BACKUP SCHEDULE   STATUS
frontend-mysql           default               sample-target                                                                    Available
mysql-label-backupplan   default               sample-target                                                                    Available
```


```
cat backup-plan.yaml
```

```
cat retention-policy.yaml
```


✅  **Show Backups**

```
oc get backups --sort-by=status.completionTimestamp
```
Output:
```
NAME                        BACKUPPLAN               BACKUP TYPE   STATUS      DATA SIZE   START TIME             END TIME               PERCENTAGE COMPLETED   BACKUP SCOPE
mysql-label-backup          mysql-label-backupplan   Full          Available   260247552   2021-01-13T08:54:52Z   2021-01-13T09:01:18Z   100                    App
mysql-label-backup-incr-1   mysql-label-backupplan   Incremental   Available   1183744     2021-01-14T19:34:12Z   2021-01-14T19:37:31Z   100                    App
test-backup-ben             test-backupplan-ben      Full          Available   260247552   2021-01-19T22:02:30Z   2021-01-19T22:07:38Z   100                    Namespace
```

✅  **Show Restore**

**No current pods in namespace**

```
oc get pods -n demotarget
```


**show restore file**

```
cat restore.yaml
```

**apply restore file**

```
oc create -f restore.yaml
```



✅  **MetaMover and Datamover pods in action**


**Show Pods and Objects in action**

```
oc get pods -n demotarget
```
Output:
``` 
NAME                                  READY   STATUS    RESTARTS   AGE
demo-restore-metamover-4u1uuv-tpqzj   1/1     Running   0          24s
colinmccarthy@Colins-MacBook-Pro tvk-demo % oc get pods -n demotarget
NAME                                  READY   STATUS      RESTARTS   AGE
demo-restore-datamover-itq965-7g8th   1/1     Running     0          40s
demo-restore-metamover-4u1uuv-tpqzj   0/1     Completed   0          69s
colinmccarthy@Colins-MacBook-Pro tvk-demo % 
colinmccarthy@Colins-MacBook-Pro tvk-demo % oc get pods -n demotarget
NAME                                  READY   STATUS      RESTARTS   AGE
demo-restore-datamover-itq965-7g8th   1/1     Running     0          2m
demo-restore-metamover-4u1uuv-tpqzj   0/1     Completed   0          2m29s
colinmccarthy@Colins-MacBook-Pro tvk-demo % 
colinmccarthy@Colins-MacBook-Pro tvk-demo % 
colinmccarthy@Colins-MacBook-Pro tvk-demo % oc get pods -n demotarget
NAME                                    READY   STATUS    RESTARTS   AGE
k8s-demo-app-frontend-7c4bdbf9b-c7z5b   1/1     Running   0          49s
k8s-demo-app-frontend-7c4bdbf9b-h9c5m   1/1     Running   0          49s
k8s-demo-app-frontend-7c4bdbf9b-tqmrq   1/1     Running   0          49s
k8s-demo-app-mysql-754f46dbd7-xbn9v     1/1     Running   0          47s

```

✅  **App has been Restored to a new namespace.🎉🎉**  

```
oc get all -n demotarget -l app=k8s-demo-app
```
Output:
```
NAME                                        READY   STATUS    RESTARTS   AGE
pod/k8s-demo-app-frontend-7c4bdbf9b-c7z5b   1/1     Running   0          2m2s
pod/k8s-demo-app-frontend-7c4bdbf9b-h9c5m   1/1     Running   0          2m2s
pod/k8s-demo-app-frontend-7c4bdbf9b-tqmrq   1/1     Running   0          2m2s
pod/k8s-demo-app-mysql-754f46dbd7-xbn9v     1/1     Running   0          2m

NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/k8s-demo-app-frontend   LoadBalancer   172.30.246.109   <pending>     80:30900/TCP   2m3s
service/k8s-demo-app-mysql      ClusterIP      None             <none>        3306/TCP       2m3s

NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/k8s-demo-app-frontend   3/3     3            3           2m3s
deployment.apps/k8s-demo-app-mysql      1/1     1            1           2m3s

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/k8s-demo-app-frontend-7c4bdbf9b   3         3         3       2m4s
replicaset.apps/k8s-demo-app-mysql-754f46dbd7     1         1         1       2m3s
```



