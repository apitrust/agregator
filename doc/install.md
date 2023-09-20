Update HELM
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
mv /usr/local/bin/helm /usr/bin/helm
```
APISIX + APISIX-ingress
```
$ git clone https://github.com/apache/apisix-helm-chart.git
$ cd apisix-helm-chart/
$ helm install apisix charts/apisix --create-namespace --namespace apisix --set admin.allow.ipList="{0.0.0.0/0, ::/64}"
$ helm install apisix-ingress-controller charts/apisix-ingress-controller --namespace apisix --set config.apisix.serviceNamespace=apisix --set config.apisix.clusterName=cluster.local --set config.apisix.adminAPIVersion=v3
```
