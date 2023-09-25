Keycloak
```
ADMIN_USER=user
ADMIN_PASSWORD=ADMIN123

helm install auth oci://registry-1.docker.io/bitnamicharts/keycloak \
  --set auth.adminUser=${ADMIN_USER} \
  --set auth.adminPassword=${ADMIN_PASSWORD} \
  --set proxy=edge
```
