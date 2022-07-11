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


```
FROM ubuntu:20.04

RUN apt-get update && apt-get install -y --no-install-recommends curl jq git sed wget

RUN wget --no-check-certificate https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/stable/openshift-client-linux.tar.gz \
    && tar xvzf openshift-client-linux.tar.gz \
    && mv kubectl /usr/local/bin \
    && mv oc /usr/local/bin

USER root
ENTRYPOINT ["bash"]
```

```
FROM ubuntu:20.04


ARG DOWNLOAD_URL=http://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/integration/12.0.4.0-ACE-LINUX64-DEVELOPER.tar.gz
ARG PRODUCT_LABEL=ace-12.0.4.0

# Prevent errors about having no terminal when using apt-get
ENV DEBIAN_FRONTEND noninteractive

# Install ACE v12.0.4.0 and accept the license
RUN apt-get update && apt-get install -y --no-install-recommends curl && \
    mkdir /opt/ibm && echo Downloading package ${DOWNLOAD_URL} && \
    curl ${DOWNLOAD_URL} | tar zx --directory /opt/ibm && \
    mv /opt/ibm/${PRODUCT_LABEL} /opt/ibm/ace-12 && \
    /opt/ibm/ace-12/ace make registry global accept license deferred

# Configure the system
RUN echo "ACE_12:" > /etc/debian_chroot \
  && echo ". /opt/ibm/ace-12/server/bin/mqsiprofile" >> /root/.bashrc

# mqsicreatebar prereqs; need to run "Xvfb -ac :99 &" and "export DISPLAY=:99"
RUN apt-get -y install libgtk2.0-0 libxtst6 xvfb git curl libswt-gtk-4-java libswt-gtk-4-jni

# Set BASH_ENV to source mqsiprofile when using docker exec bash -c
ENV BASH_ENV=/opt/ibm/ace-12/server/bin/mqsiprofile


RUN su - root -c "export LICENSE=accept && . /opt/ibm/ace-12/server/bin/mqsiprofile && mqsicreateworkdir /root"

USER root
RUN echo "Xvfb -ac :100 &" >> /root/.bashrc
RUN echo "export DISPLAY=:100" >> /root/.bashrc
ENTRYPOINT ["bash"]
```
```
podman build -t ace-bar-builder:12.0.4.0-ubuntu -f Dockerfile.ace-bar-builder

Successfully tagged localhost/ace-bar-builder:12.0.4.0-ubuntu

podman tag localhost/ace-bar-builder:12.0.4.0-ubuntu image-registry.openshift-image-registry.svc:5000/cp4i-poc/ace-bar-builder:12.0.4.0-ubuntu
podman push image-registry.openshift-image-registry.svc:5000/cp4i-poc/ace-bar-builder:12.0.4.0-ubuntu
```
