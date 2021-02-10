# Red Hat Advanced Cluster Manager (RHACM) Introduction

The official RHACM docs are [here](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/)

Channel types supported by RHACM:

* *namespace* - Namespace (namespace channel)
* *helmrepo* - Helm repositories
* *objectbucket* - Object storage repositories
* *git* - Git repositores (supports [Kustomize](https://kustomize.io/))
* ~~*github*~~ - GitHub repositories (likely deprecated, use *git* instead)

Futher info:

Subscription annotation `apps.open-cluster-management.io/github-path` can point to a single yaml file, example:
```
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  annotations:
    apps.open-cluster-management.io/github-path: docs/examples/configure_image_registry/manifests/cluster-config.yaml
    apps.open-cluster-management.io/reconcile-option: merge
  name: image-registry-config
...
```

* Subscription reconcile-option *replace* (update) vs *merge* (patch)
* Need to validate the deployment using Ansible, bounce the not-fully deployed resources ?
* It is best practice to create each channel in a unique namespace. However, a Git channel can share a namespace with another type of channel, including Git, Helm, and Object storage. Refer to [Managing application](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.1/html/manage_applications/managing-applications).
* Sample OpenShift configuration manifests available at [openshift-post-install](https://github.com/noseka1/openshift-post-install)

## Caveats

* **A single Subscription cannot include duplicate definition of the same resource**. If there are multiple definitions of the same resource, the last definition wins. A resource is identified by GVK (Group, Version, Kind) + NamespacedName.
