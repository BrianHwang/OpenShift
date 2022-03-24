# add access key id and key
```
aws configure
```

# verify AWS config
```
aws sts get-caller-identity
```

# install rosa
```
wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/rosa/latest/rosa-linux.tar.gz
tar zxvf rosa-linux.tar.gz
export PATH=~:$PATH
rosa download oc
tar zxvf openshift-client-linux.tar.gz
rm rosa-linux.tar.gz openshift-client-linux.tar.gz README.md
```

```
rosa version 
rosa login
```
```
rosa verify permissions
rosa verify quota
rosa whoami
```

```
rosa init
 
rosa create account-roles --mode auto –yes
```

```
#rosa create cluster --cluster-name=rosa-poc --interactive –sts

rosa create cluster --cluster-name rosa-poc --sts --mode auto --yes

rosa logs install -c rosa-poc --watch

rosa list clusters
rosa describe cluster --cluster=rosa-poc
``` 

```
rosa create admin -c rosa-poc 
```

## WAIT few Mins

# verify with oc login commend from response
```
oc login https://api.rosa-poc-<given_link>-openshiftapps.com:6443 
oc whoami
oc version
oc get nodes
```


 
# openshift console -> addon -> Cluster Logging Operator -> install ->
then logs will go to cloudwatch
```
login pod statrs
watch oc get pods -n openshift-logging
``` 
# to check the openshift cluster operators
```
oc get nodes
oc get co
```
