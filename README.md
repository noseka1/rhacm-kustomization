# Kustomization for Deploying Red Hat Advanced Cluster Manager

This kustomization deploys Red Hat Advanced Cluster Manager (RHACM) Hub on OpenShift.

RHACM product documentation can be found [here](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes)

## Quick start

Note that some of the following commands require *cluster-admin* role.

Install RHACM operator:
```
$ oc apply --kustomize rhacm-operator/base
```

Deploy a RHACM instance:
```
$ oc apply --kustomize rhacm-instance/overlays/ha
```

Optionally, deploy Ansible Automation Platform Resource Operator aka [AWX Resource Operator](https://github.com/ansible/awx-resource-operator):

```
$ oc apply --kustomize awx-resource-operator/base/
```

## Enabling observability service

Refer to the [Observing environments](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/observing_environments/observing-environments) docs.

Review the [rhacm-observability/base](rhacm-observability/base) manifests and modify it to suit your needs.

To deploy the observability service, issue the command:

```
$ oc apply --kustomize rhacm-observability/overlays/ha
```

## Related links

* https://github.com/redhat-gpte-devopsautomation/rhacm-labs
* https://github.com/open-cluster-management/policy-collection
* https://github.com/redhat-cop/acm-policies
* https://github.com/open-cluster-management/demo-subscription-gitops
