# Install AppDynamics Kubernetes Data Collectors 

```
kubectl create namespace appdynamics
```

Apply certificate:
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml
```

Add the helm repo:
```
helm repo add appdynamics-cloud-helmcharts https://appdynamics.jfrog.io/artifactory/appdynamics-cloud-helmcharts/
```

Install AppDynamics Operators
```
helm install appdynamics-operators appdynamics-cloud-helmcharts/appdynamics-operators -n appdynamics --wait
helm install appdynamics-operators appdynamics-cloud-helmcharts/appdynamics-operators -n appdynamics -f operators-values.yaml --wait
```

Install the Cisco AppDynamics Collectors using collectors-values.yaml
helm install appdynamics-collectors appdynamics-cloud-helmcharts/appdynamics-collectors -n appdynamics -f collectors-values.yaml

helm install appdynamics-collectors appdynamics-cloud-helmcharts/appdynamics-collectors -n appdynamics -f collectors-values.yaml

Validate Installation

Connect to cluster:
az aks get-credentials --resource-group mg-otel-demo --name mg-otel-demo

See current context
kubectl config current-context
Set cluster context
kubectl config use-context mg-otel-demo
kubectl get namespaces

Uninstall helm
helm uninstall mg-otel-demo

kubectl create namespace mg-otel-demo

Set namespace
kubectl config set-context --current --namespace=mg-otel-demo
Deploy App
Download the applications
helm fetch open-telemetry/opentelemetry-demo --untar=true --untardir=/Users/mgarces/Documents/aks/otel-demo-app

Local file
Go to directory that has the downloaded file above
helm install mg-otel-demo ./opentelemetry-demo

Re-deploy
Set context to mg-otel-demo space
helm list to see releases
helm upgrade mg-otel-demo ./opentelemetry-demo --force


helm install mg-otel-demo open-telemetry/opentelemetry-demo


Port Forward
kubectl port-forward svc/mg-otel-demo-frontendproxy 8080:8080

Grafana
http://localhost:8080/grafana/


Kubernetes and APM Monitoring
Create Yaml files

Create appdynamics namespace
kubectl create namespace appdynamics

Apply certificate:
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.yaml

Add the helm repo:
helm repo add appdynamics-cloud-helmcharts https://appdynamics.jfrog.io/artifactory/appdynamics-cloud-helmcharts/

Install AppDynamics Operators
helm install appdynamics-operators appdynamics-cloud-helmcharts/appdynamics-operators -n appdynamics --wait

Install the Cisco AppDynamics Collectors using collectors-values.yaml
helm install appdynamics-collectors appdynamics-cloud-helmcharts/appdynamics-collectors -n appdynamics -f collectors-values.yaml

Validate Installation
kubectl get all -n appdynamics

OTEL Instrumentation
check context
kubectl config current-context
Check Services
kubectl -n appdynamics get services
Check Services for otel app
kubectl -n mg-otel-demo get pods

Check activity with AdService (get add service name from above)

kubectl -n mg-otel-demo logs mg-otel-demo-adservice-76d876f6d5-pjlrf


Reinstall Collectors
helm uninstall appdynamics-collectors -n appdynamics
helm repo update
helm install appdynamics-collectors appdynamics-cloud-helmcharts/appdynamics-collectors -n appdynamics -f collectors-values.yaml --wait


Fixing Correlation
Link in Confluence
- name: OTEL_RESOURCE_ATTRIBUTES
- name: OTEL_RESOURCE_ATTRIBUTES

Connect CNAO to Azure
https://docs.appdynamics.com/fso/cloud-native-app-obs/en/infrastructure-monitoring/azure-cloud-infrastructure-observability/configure-azure-cloud-connection

Create Azure App in terminal
Install-Module AzureAD
Connect-AzureAD
az login
az account show --output table

az ad app create --display-name cnao-app-registration

REDEPLOY
helm upgrade mg-otel-demo ./opentelemetry-demo --force

Redeploy AppD collectors
helm upgrade appdynamics-collectors appdynamics-cloud-helmcharts/appdynamics-collectors -n appdynamics -f collectors-values.yaml

Uninstall
helm install appdynamics-collectors appdynamics-cloud-helmcharts/appdynamics-collectors -n appdynamics -f collectors-values-with-logs.yaml --wait

Changes to App
Disabled otel collector in FronEnd Proxy

RESET

Delete helm app
helm uninstall RELEASE_NAME
helm uninstall mg-otel-demo

helm repo remove open-telemetry

Delete namespace
kubectl delete namespace mariog-demo

Delete all
kubectl delete all --all -n mariog-demo

az aks get-credentials --resource-group mg-demo1_group --name mg-demo1
