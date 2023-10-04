Install Istio service mesh

```
helm repo add istio https://istio-release.storage.googleapis.com/charts
helm repo update

kubectl create namespace istio-system

helm install istio-base istio/base -n istio-system --set defaultRevision=default
helm install istiod istio/istiod -n istio-system --wait

helm repo add kiali https://kiali.org/helm-charts
helm install --namespace istio-system kiali-server kiali/kiali-server

```
