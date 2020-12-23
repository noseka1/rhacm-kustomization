# Kustomization for Deploying Red Hat Advanced Cluster Manager

This kustomization deploys Red Hat Advanced Cluster Manager (RHACM) Hub on OpenShift.

RHACM product documentation can be found [here](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes)

## Quick Start

Note that some of the following commands require *cluster-admin* role.

Install RHACM operator:
```
$ oc apply --kustomize rhacm-operator/base
```

Deploy a RHACM instance:
```
$ oc apply --kustomize rhacm-instance/overlays/ha
```
## Sample Manifests

* https://github.com/redhat-gpte-devopsautomation/rhacm-labs
