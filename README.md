## UAPI-UI Operator deploys API, UI and MongoDB service. 

### Deploy the UAPI Ansible operator 
```bash
# CRD - all-in-one 
oc create -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/all-in-one.yaml
# CR
oc create -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml
```

### Cleanup
```bash
# CR
oc delete -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml
# CRD
oc delete -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/all-in-one.yaml
```


### Build & Run Operator locally 
```bash
# Install Ansible dependencies
pipenv install 
# Create CRD
oc create -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_crd.yaml
# Run Operator 
pipenv run op
# Create CR
oc create -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml
# Cleanup CR
oc delete -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml 
# Cleanup CRD
oc delete -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_crd.yaml
```

### Debug Operator locally 
```bash
# Run Ansible
pipenv run ansible-playbook uiapi.yml --extra-vars='{"namespace": "uapi","ui": {"size": 1,"name": "ui","service_node_port": 30080,"api_url": "http://127.0.0.1:30081","image": "docker.io/dimssss/upapi-ui:latest"},"api": {"size": 1,"name": "uiapi","service_node_port": 30081,"conf_secret_name": "uapisecret","image": "docker.io/dimssss/upapi:latest"},"db": {"port": 27017,"host": "mongo","name": "uapi","image": "registry.redhat.io/rhscl/mongodb-36-rhel7:latest"}}'
# Cleanup 
oc delete deployment mongo ui uiapi
oc delete service mongo ui uiapi
oc delete secret uapisecret

```

### Build UAPI Ansible operator
```bash
operator-sdk build docker.io/dimssss/uapi-operator:TAG
docker push docker.io/dimssss/uapi-operator:TAG
```


Customize CR
```bash
apiVersion: uiapi.com/v1alpha1
kind: Uapi
metadata:
  name: uapi
spec:
  namespace: "uapi"
  ui:
    size: 1
    name: "ui"
    service_node_port: 30080
    api_url: "http://127.0.0.1:30081"
    image: "docker-registry.default.svc:5000/uapi/ui:latest"
  api:
    size: 1
    name: "uiapi"
    service_node_port: 30081
    conf_secret_name: "uapisecret"
    image: "docker-registry.default.svc:5000/uapi/uapi:latest"
  db:
    port: 27017
    host: "mongo"
    name: "uapi"
    image: "registry.redhat.io/rhscl/mongodb-36-rhel7:latest"

```