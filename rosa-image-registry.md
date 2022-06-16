https://docs.openshift.com/container-platform/4.6/registry/accessing-the-registry.html

```
oc policy add-role-to-user registry-viewer <user_name>
oc policy add-role-to-user registry-editor <user_name>

oc get nodes
oc debug nodes/<node_address>
```


```
chroot /host
oc login -u cluster-admin -p <password_from_install_log> https://api-int.<cluster_name>.<base_domain>:6443
podman login -u cluster-admin -p $(oc whoami -t) image-registry.openshift-image-registry.svc:5000





configs.imageregistry.operator.openshift.io

oc patch configs.imageregistry.operator.openshift.io/cluster --type merge -p '{"spec":{"defaultRoute":true}}'
