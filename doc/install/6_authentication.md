```
URI=https://68fa00bd-755f-4476-a504-4d627e9a56ef-10-244-3-198-80.spch.r.killercoda.com
LOCAL_URI=http://auth-keycloak.keycloak.svc.cluster.local
KC_REALM=apisix
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=4lLY9PXTaFNYWTxIMZebFUCAYat8TLqF
KC_DISCOVERY_ENDPOINT=${URI}/realms/apisix/.well-known/openid-configuration
KC_TOKEN_ENDPOINT=${URI}/realms/apisix/protocol/openid-connect/token
KC_INTRO_ENDPOINT=${URI}/realms/apisix/protocol/openid-connect/token/introspect

```
```
kubectl apply -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixUpstream
metadata:
  name: httpbin-upstream
spec:
  externalNodes:
  - type: Domain
    name: httpbin.org
EOF
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
      - /anything/*
    upstreams:
    - name: httpbin-upstream
    plugins:
    - name: openid-connect
      enable: true
      config:
        client_id: ${KC_CLIENT_ID}
        client_secret: ${KC_CLIENT_SECRET}
        discovery: ${KC_DISCOVERY_ENDPOINT}
        token_endpoint: ${KC_TOKEN_ENDPOINT}
        realm: ${KC_REALM}
        redirect_uri: ${URI}/anything/redirect_uri
        access_token_in_authorization_header: true
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
