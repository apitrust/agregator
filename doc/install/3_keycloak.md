Keycloak (need HELM > 3.9)
```
ADMIN_USER=user
ADMIN_PASSWORD=ADMIN123

kubectl create namespace keycloak
kubectl label namespace keycloak istio-injection=enabled

helm install auth oci://registry-1.docker.io/bitnamicharts/keycloak -n keycloak \
  --set auth.adminUser=${ADMIN_USER} \
  --set auth.adminPassword=${ADMIN_PASSWORD} \
  --set proxy=edge

```
```
kubectl port-forward -n keycloak svc/auth-keycloak --address 0.0.0.0 80
```
