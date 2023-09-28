```
URI=https://9a98b457-1e61-4617-8bb8-d4a858c70b78-10-244-4-11-31614.spch.r.killercoda.com:80
LOCAL_URI=http://auth-keycloak.keycloak.svc.cluster.local
KC_REALM=apisix
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=OjsUmbuVaMSZt472sDcQFNu69dMPfiqi
KC_DISCOVERY_ENDPOINT=${URI}/realms/apisix/.well-known/openid-configuration
KC_TOKEN_ENDPOINT=${URI}/realms/apisix/protocol/openid-connect/token
KC_INTRO_ENDPOINT=${URI}/realms/apisix/protocol/openid-connect/token/introspect

```
```
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
      - /auth
      - /auth/*
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
        token_endpoint: ${KC_TOKEN_ENDPOINT}
        realm: ${KC_REALM}
        redirect_uri: /auth/redirect_uri
EOF

```
```
kubectl apply -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: http-route-authz1
spec:
  http:
  - name: http-authz1
    match:
      paths:
      - /authz
    backends:
    - serviceName: httpecho
      servicePort: 8080
    plugins:
    - name: authz-keycloak
      enable: true
      config:
        client_id: ${KC_CLIENT_ID}
        client_secret: ${KC_CLIENT_SECRET}
        discovery: ${KC_DISCOVERY_ENDPOINT}
        realm: ${KC_REALM}
        ssl_verify: false
EOF


```
