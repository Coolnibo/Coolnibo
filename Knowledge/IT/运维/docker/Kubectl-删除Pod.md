

删除状态为`Terminating`的Pod

```shell
kubectl delete pod my-pod -n 命名空间 --grace-period=0 --force
```