## validation

installation: 

```
kubectl create -f mongo-nfs-client-class.yaml

kubectl create -f mongodb-statefulset.yaml
```

validation:

```
[root@k8s-master resources]# kubectl get pods -owide|grep mongo
mongo-0                                   2/2       Running       0          10h       192.168.196.61    k8s-node5
mongo-1                                   2/2       Running       0          10h       192.168.122.180   k8s-node4
mongo-2                                   2/2       Running       0          10h       192.168.169.252   k8s-node2

[root@k8s-master resources]# kubectl get pvc|grep mongo
mongo-datadir-mongo-0   Bound     pvc-39786d31-0f57-11e9-8963-005056bc2383   5Gi        RWO            mongo-demo-nfs-storage   17m
mongo-datadir-mongo-1   Bound     pvc-18c4c918-0f58-11e9-8963-005056bc2383   5Gi        RWO            mongo-demo-nfs-storage   11m
mongo-datadir-mongo-2   Bound     pvc-4a490f99-0f58-11e9-8963-005056bc2383   5Gi        RWO            mongo-demo-nfs-storage   9m

使用`rs.conf()`和`rs.status()`命令可看到每个mongodb实例的配置、角色和状态。

[root@k8s-master resources]# kubectl exec -it  mongo-1 mongo bash

2019-01-03T14:04:11.326+0000 I CONTROL  [initandlisten] 

rs0:PRIMARY> rs.conf()
{
	"_id" : "rs0",
	"version" : 316142,
	"protocolVersion" : NumberLong(1),
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.196.61:27017",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "192.168.122.180:27017",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "192.168.169.252:27017",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		}
	],
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : 60000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("5c2e1567b61717cc844a5604")
	}
}
rs0:PRIMARY> 
rs0:PRIMARY> rs.status()
{
	"set" : "rs0",
	"date" : ISODate("2019-01-04T00:53:41.395Z"),
	"myState" : 1,
	"term" : NumberLong(3),
	"syncingTo" : "",
	"syncSourceHost" : "",
	"syncSourceId" : -1,
	"heartbeatIntervalMillis" : NumberLong(2000),
	"optimes" : {
		"lastCommittedOpTime" : {
			"ts" : Timestamp(1546563211, 1),
			"t" : NumberLong(3)
		},
		"appliedOpTime" : {
			"ts" : Timestamp(1546563211, 1),
			"t" : NumberLong(3)
		},
		"durableOpTime" : {
			"ts" : Timestamp(1546563211, 1),
			"t" : NumberLong(3)
		}
	},
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.196.61:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 38944,
			"optime" : {
				"ts" : Timestamp(1546563211, 1),
				"t" : NumberLong(3)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1546563211, 1),
				"t" : NumberLong(3)
			},
			"optimeDate" : ISODate("2019-01-04T00:53:31Z"),
			"optimeDurableDate" : ISODate("2019-01-04T00:53:31Z"),
			"lastHeartbeat" : ISODate("2019-01-04T00:53:39.625Z"),
			"lastHeartbeatRecv" : ISODate("2019-01-04T00:53:40.411Z"),
			"pingMs" : NumberLong(1),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "192.168.122.180:27017",
			"syncSourceHost" : "192.168.122.180:27017",
			"syncSourceId" : 2,
			"infoMessage" : "",
			"configVersion" : 316142
		},
		{
			"_id" : 1,
			"name" : "192.168.169.252:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 38820,
			"optime" : {
				"ts" : Timestamp(1546563211, 1),
				"t" : NumberLong(3)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1546563211, 1),
				"t" : NumberLong(3)
			},
			"optimeDate" : ISODate("2019-01-04T00:53:31Z"),
			"optimeDurableDate" : ISODate("2019-01-04T00:53:31Z"),
			"lastHeartbeat" : ISODate("2019-01-04T00:53:39.625Z"),
			"lastHeartbeatRecv" : ISODate("2019-01-04T00:53:40.581Z"),
			"pingMs" : NumberLong(1),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "192.168.122.180:27017",
			"syncSourceHost" : "192.168.122.180:27017",
			"syncSourceId" : 2,
			"infoMessage" : "",
			"configVersion" : 316142
		},
		{
			"_id" : 2,
			"name" : "192.168.122.180:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 38990,
			"optime" : {
				"ts" : Timestamp(1546563211, 1),
				"t" : NumberLong(3)
			},
			"optimeDate" : ISODate("2019-01-04T00:53:31Z"),
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"electionTime" : Timestamp(1546529246, 2),
			"electionDate" : ISODate("2019-01-03T15:27:26Z"),
			"configVersion" : 316142,
			"self" : true,
			"lastHeartbeatMessage" : ""
		}
	],
	"ok" : 1
}
rs0:PRIMARY> 

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