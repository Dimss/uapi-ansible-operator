## UAPI-UI Operator deploys API, UI and MongoDB service. 

### Build UAPI Ansible operator
```bash
operator-sdk build docker.io/dimssss/uapi-operator:TAG
docker push docker.io/dimssss/uapi-operator:TAG
```

### Deploy the UAPI Ansible operator 
```bash
kubectl create -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/all-in-one.yaml
kubectl create -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml
```

### Cleanup
```bash
kubectl delete -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml
kubectl delete -f https://raw.githubusercontent.com/Dimss/uapi-ansible-operator/master/deploy/all-in-one.yaml
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