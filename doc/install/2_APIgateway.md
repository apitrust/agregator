Kong + Kong Ingress Controller
```
helm repo add kong https://charts.konghq.com
helm repo update

kubectl create namespace kong 
kubectl label namespace kong istio-injection=enabled

helm install kong/ingress --namespace kong --generate-name

helm install kong/kong --namespace kong --generate-name \
  --set proxy.type=NodePort

```

APISIX + APISIX-ingress
```
git clone https://github.com/apache/apisix-helm-chart.git
# git clone https://github.com/kodxxl/apisix-helm-chart.git
cd apisix-helm-chart/

APISIX_ADMIN_KEY=$(dd if=/dev/urandom count=1 status=none | md5sum | cut -d " " -f1)
APISIX_VIEWER_KEY=$(dd if=/dev/urandom count=1 status=none | md5sum | cut -d " " -f1)

kubectl create namespace apisix
kubectl label namespace apisix istio-injection=enabled

helm install apisix charts/apisix --namespace apisix \
  --set admin.allow.ipList="{0.0.0.0/0, ::/64}" \
  --set admin.credentials.admin=${APISIX_ADMIN_KEY} \
  --set admin.credentials.viewer=${APISIX_VIEWER_KEY} \
  --set gateway.externalTrafficPolicy=Local 

helm install apisix-ingress-controller charts/apisix-ingress-controller --namespace apisix \
  --set config.apisix.adminKey=${APISIX_ADMIN_KEY} \
  --set config.apisix.serviceNamespace=apisix \
  --set config.apisix.clusterName=cluster.local \
  --set config.apisix.adminAPIVersion=v3

unset APISIX_ADMIN_KEY
unset APISIX_VIEWER_KEY

```
