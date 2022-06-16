https://docs.openshift.com/container-platform/4.6/registry/accessing-the-registry.html


local machine
```
oc policy add-role-to-user registry-viewer <user_name>
oc policy add-role-to-user registry-editor <user_name>

oc get nodes
oc debug nodes/<node_address>
```


debug pod in one of node
```
chroot /host
oc login https://api.<rosa_cluster>.openshiftapps.com:6443 --username cluster-admin --password <password>
oc whoami

podman login -u cluster-admin -p $(oc whoami -t) image-registry.openshift-image-registry.svc:5000


podman images | grep -i <image>

podman tag docker.io/library/nginx image-registry.openshift-image-registry.svc:5000/brian/nginx:0.1
podman tag name.io/image image-registry.openshift-image-registry.svc:5000/openshift/image
podman push image-registry.openshift-image-registry.svc:5000/openshift/image



```




configs.imageregistry.operator.openshift.io

oc patch configs.imageregistry.operator.openshift.io/cluster --type merge -p '{"spec":{"defaultRoute":true}}'