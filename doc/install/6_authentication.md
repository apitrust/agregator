```
URI=https://0c731386-f5ce-4b5b-94c3-e5d89af47824-10-244-3-102-80.spch.r.killercoda.com
LOCAL_URI=http://auth-keycloak.keycloak.svc.cluster.local
KC_REALM=apisix
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=g5yCKRgYrEM5mdN9RC4VbxkamPhaBKE6
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
      - /redirect_uri
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
        introspection_endpoint: ${KC_INTRO_ENDPOINT}
        realm: ${KC_REALM}
        redirect_uri: /redirect_uri
    - name: file-logger
      enable: true
      config:
        path: logs/vve.log
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
