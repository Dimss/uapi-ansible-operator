## UAPI-UI Operator deploys API, UI and MongoDB service. 

To deploy the UAPIUI app with operator, 
- Deploy the Operator `kubectl create -f https://gitlab.com/dimss/ppro-uapiui-operator/raw/master/deploy/all-in-one.yaml`
- Make sure the Operator container is up and running `kubectl get pods | grep uapi-operator`
- Create new Custom Resource `kubectl create -f https://gitlab.com/dimss/ppro-uapiui-operator/raw/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml`

Customize CR
```bash
apiVersion: uiapi.com/v1alpha1
kind: Uapi
metadata:
  name: uapi
spec:
  # Specify to which namespace deploy services  
  namespace: "default"
  ui:
    # Set replica counts of the service 
    size: 1
    # That name will be used for creating various related K8S objects  
    name: "ui"
    # Service node port
    service_node_port: 30080
    # The backend API URL - THAT URL MUST BE ACCESSIBLE FROM OUTSIDE OF THE CLUSTER
    api_url: "http://192.168.99.100:30081"
    # UI service docker image 
    image: "docker.io/dimssss/uapiui:latest"
  api:
    # Set replica counts of the service
    size: 1
    # That name will be used for creating various related K8S objects 
    name: "uiapi"
    # The node port for the API service 
    service_node_port: 30081
    # Secret name for configuration 
    conf_secret_name: "uapisecret"
    # API docker image 
    image: "docker.io/dimssss/uapi:latest"
  db:
    # Service name for MongoDB service 
    host: "mongo"
    # DB name 
    name: "uapi"
```