```
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=Xpx9FYacA2o7O5RTsP62a7pQOou5nzqv
KC_DISCOVERY_ENDPOINT=http://auth-keycloak.keycloak.svc.cluster.local/realms/apisix/.well-known/openid-configuration

kubectl apply -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: httpecho1-route-auth
spec:
  http:
  - name: httpecho1-auth
    match:
      paths:
      - /echo-auth
    backends:
       - serviceName: httpecho
         servicePort: 8080
    plugins:
    - name: openid-connect
      enable: true
      config:
        client_id: ${KC_CLIENT_ID}
        client_secret: ${KC_CLIENT_SECRET}
        discovery: ${KC_DISCOVERY_ENDPOINT}
        bearer_only: false
        realm: ${KC_REALM}
EOF
```
