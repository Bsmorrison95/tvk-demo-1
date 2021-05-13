
```
oc adm policy add-scc-to-user anyuid -z default -n demotarget
```



```
oc create -f restore.yaml
Error from server (AlreadyExists): error when creating "restore.yaml": restores.triliovault.trilio.io "demo-restore" already exists
```
