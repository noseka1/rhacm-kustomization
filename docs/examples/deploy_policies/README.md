Add "dev" label to `local-cluster`:

```
$ oc patch -f local-cluster-managedcluster.yaml --patch "$(cat local-cluster-managedcluster.yaml)" --type merge
```

Create `policies` namespace:

```
$ oc apply -f policies-ns.yaml
```

Switch to the `policies` namespace:

```
$ oc project policies
```

Apply sample policies:

```
$ oc apply -f stable/AC-Access-Control/policy-limitclusteradmin.yaml
$ oc apply -f stable/AC-Access-Control/policy-role.yaml
$ oc apply -f stable/AC-Access-Control/policy-rolebinding.yaml
```

```
$ oc apply -f stable/SC-System-and-Communications-Protection/policy-certificate.yaml
$ oc apply -f stable/SC-System-and-Communications-Protection/policy-etcdencryption.yaml
$ oc apply -f stable/SC-System-and-Communications-Protection/policy-limitmemory.yaml
$ oc apply -f stable/SC-System-and-Communications-Protection/policy-psp.yaml
$ oc apply -f stable/SC-System-and-Communications-Protection/policy-scc.yaml
```

```
$ oc apply -f stable/CA-Security-Assessment-and-Authorization/policy-compliance-operator-install.yaml
```

Didn't work, operator failed to install:

```
$ oc patch policy policy-comp-operator --patch '{"spec":{"remediationAction":"enforce"}}' --type merge
```

```
$ oc apply -f stable/CM-Configuration-Management/policy-compliance-operator-cis-scan.yaml
$ oc apply -f stable/CM-Configuration-Management/policy-compliance-operator-e8-scan.yaml
```

Let compliance-operator start the scanning:

```
$ oc patch policy policy-cis-scan --patch '{"spec":{"remediationAction":"enforce"}}' --type merge
```

```
$ oc patch policy policy-e8-scan --patch '{"spec":{"remediationAction":"enforce"}}' --type merge
```
