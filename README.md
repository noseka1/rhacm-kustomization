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
## Troubleshooting

Refer to the [Troubleshooting guide](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/troubleshooting/troubleshooting) for ideas on what could have gone wrong. The guide describes typical issues you may encounter.

### Enabling Application Manager verbose logging

Prevent the operators from restoring the klusterlet-addon-appmgr resource configuration:

```
$ oc scale --replicas 0 -n open-cluster-management-agent deploy klusterlet 
$ oc scale --replicas 0 -n open-cluster-management-agent deploy klusterlet-work-agent
$ oc scale --replicas 0 -n open-cluster-management-agent-addon deploy klusterlet-addon-operator
```

Add `--v=10` parameter to the args of `subscription-controller` container to increase the log level to maximum:

```
$ oc edit deploy -n open-cluster-management-agent-addon klusterlet-addon-appmgr
```

Follow the very verbose logs:

```
$ oc logs -n open-cluster-management-agent-addon -l app=application-manager -c subscription-controller -f
```

### Collecting debugging data

Collect debugging data about the currently running Openshift cluster:

```
$ oc adm must-gather
```

Collect debugging information specific to Red Hat Advanced Cluster Manager:

```
$ oc adm must-gather --image registry.redhat.io/rhacm2/acm-must-gather-rhel8:v2.1.0
```

## Labs

* https://github.com/open-cluster-management/lab
* https://github.com/redhat-gpte-devopsautomation/rhacm-labs

## Manifest examples

* https://github.com/open-cluster-management/demo-subscription-gitops
* https://github.com/open-cluster-management/multicloud-operators-application/tree/master/examples
* https://github.com/open-cluster-management/multicloud-operators-channel/tree/master/examples/e2e_channel_all_types_example
* https://github.com/open-cluster-management/multicloud-operators-deployable/tree/master/examples
* https://github.com/open-cluster-management/multicloud-operators-foundation/tree/master/examples
* https://github.com/open-cluster-management/multicloud-operators-subscription/tree/master/examples
* https://github.com/open-cluster-management/multicloud-operators-subscription-release/tree/master/examples
* https://github.com/openshift/openshift-gitops-examples

## Full GitOps examples

* https://github.com/PixelJonas/cluster-gitops
* https://github.com/gnunn-gitops/cluster-config
* https://github.com/christianh814/openshift-cluster-config
* https://github.com/kasuboski/k8s-gitops

## Related links

* https://github.com/redhat-edge-computing/blueprint-management-hub
* https://github.com/open-cluster-management/policy-collection
* https://github.com/redhat-cop/acm-policies
* https://github.com/open-cluster-management/demo-subscription-gitops
