## UAPI-UI Operator deploys API, UI and MongoDB service. 

To deploy the UAPIUI app with operator, 
- Deploy the Operator `kubectl create -f https://gitlab.com/dimss/ppro-uapiui-operator/raw/master/deploy/all-in-one.yaml`
- Create new Custom Resource `kubectl create -f https://gitlab.com/dimss/ppro-uapiui-operator/raw/master/deploy/crds/uiapi_v1alpha1_uapi_cr.yaml`