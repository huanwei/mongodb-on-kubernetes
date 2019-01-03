## validation

installation: 

```
kubectl create -f mongo-nfs-client-class.yaml

kubectl create -f mongodb-statefulset.yaml
```

validation:

```
[root@k8s-master resources]# kubectl get pods -owide|grep mongo
mongo-0                                   2/2       Running       0          17m       192.168.196.27    k8s-node5
mongo-1                                   2/2       Running       0          10m       192.168.122.177   k8s-node4
mongo-2                                   2/2       Running       0          9m        192.168.108.27    k8s-node3

[root@k8s-master resources]# kubectl get pvc|grep mongo
mongo-datadir-mongo-0   Bound     pvc-39786d31-0f57-11e9-8963-005056bc2383   5Gi        RWO            mongo-demo-nfs-storage   17m
mongo-datadir-mongo-1   Bound     pvc-18c4c918-0f58-11e9-8963-005056bc2383   5Gi        RWO            mongo-demo-nfs-storage   11m
mongo-datadir-mongo-2   Bound     pvc-4a490f99-0f58-11e9-8963-005056bc2383   5Gi        RWO            mongo-demo-nfs-storage   9m

```

## to avoid sidecar error

```
Error in workloop { [Error: [object Object]]
  message:
   { kind: 'Status',
     apiVersion: 'v1',
     metadata: {},
     status: 'Failure',
     message:
      'pods is forbidden: User "system:serviceaccount:default:default" cannot list pods at the cluster scope',
     reason: 'Forbidden',
     details: { kind: 'pods' },
     code: 403 },
  statusCode: 403 }

```

just create service account for defualt.
```
kubectl create -f mongodb-default-sa.yaml

```

## REF

https://github.com/cvallance/mongo-k8s-sidecar/tree/master/example/StatefulSet

https://kubernetes.io/blog/2017/01/running-mongodb-on-kubernetes-with-statefulsets/

https://github.com/cvallance/mongo-k8s-sidecar/issues/75

https://cloud.tencent.com/developer/article/1138683