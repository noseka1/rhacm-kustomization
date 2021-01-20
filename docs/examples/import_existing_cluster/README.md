# Importing Existing Cluster

Based on the docs [Importing a target managed cluster to the hub cluster](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_cluster/importing-a-target-managed-cluster-to-the-hub-cluster).

On the Hub cluster, create the ManagedCluster resources:
```
$ oc apply --kustomize .
```

```
$ CLUSTER=mycluster
```

Obtain the manifests needed to join a spoke cluster:

```
$ oc extract \
    secret/$CLUSTER-import \
    --namespace $CLUSTER \
    --to - \
    --keys crds.yaml,import.yaml \
    > $CLUSTER-manifests.yaml
```

Log into the spoke cluster and apply the manifests:

```
$ oc login <mycluster>
```

You may need to run this command twice, as the CRDs may not be instantiated in time:
```
$ oc apply --filename $CLUSTER-manifests.yaml
```

Validate the agent deployment on the target cluster:

```
$ oc get pod --namespace open-cluster-management-agent
$ oc get pod --namespace open-cluster-management-agent-addon
```

Validate the status of the imported cluster on the Hub:
```
$ oc get managedcluster $CLUSTER --output jsonpath='{.status.conditions}' | jq
[
  {
    "lastTransitionTime": "2021-01-03T15:45:40Z",
    "message": "Accepted by hub cluster admin",
    "reason": "HubClusterAdminAccepted",
    "status": "True",
    "type": "HubAcceptedManagedCluster"
  },
  {
    "lastTransitionTime": "2021-01-03T16:09:45Z",
    "message": "Managed cluster is available",
    "reason": "ManagedClusterAvailable",
    "status": "True",
    "type": "ManagedClusterConditionAvailable"
  },
  {
    "lastTransitionTime": "2021-01-03T16:09:45Z",
    "message": "Managed cluster joined",
    "reason": "ManagedClusterJoined",
    "status": "True",
    "type": "ManagedClusterJoined"
  }
]
```
