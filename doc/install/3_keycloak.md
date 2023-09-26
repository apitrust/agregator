Keycloak (need HELM > 3.9)
```
ADMIN_USER=user
ADMIN_PASSWORD=ADMIN123

helm install auth oci://registry-1.docker.io/bitnamicharts/keycloak -n keycloak --create-namespace \
  --set auth.adminUser=${ADMIN_USER} \
  --set auth.adminPassword=${ADMIN_PASSWORD} \
  --set proxy=edge

```
```
kubectl port-forward svc/auth-keycloak --address 0.0.0.0 80
```
