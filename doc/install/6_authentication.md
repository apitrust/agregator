```
URI=
KC_REALM=apisix
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=
KC_DISCOVERY_ENDPOINT=${URI}/realms/apisix/.well-known/openid-configuration
KC_TOKEN_ENDPOINT=${URI}/realms/apisix/protocol/openid-connect/token
KC_INTRO_ENDPOINT=${URI}/realms/apisix/protocol/openid-connect/token/introspect
KC_LOGOUT=/auth/logout
KC_POST_LOGOUT=/anything/echo

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
      - /logout
      - /auth/logout
      - /anything/login
      - /anything/redirect_uri
      - /anything/info
    upstreams:
    - name: httpbin-upstream
    plugins:
    - name: openid-connect
      enable: true
      config:
        client_id: ${KC_CLIENT_ID}
        client_secret: ${KC_CLIENT_SECRET}
        discovery: ${KC_DISCOVERY_ENDPOINT}
        introspection_endpoint: ${KC_INTRO_ENDPOINT}
        token_endpoint: ${KC_TOKEN_ENDPOINT}
        realm: ${KC_REALM}
        unauth_action: "auth" # "deny" # "pass"
        set_access_token_header: false
        redirect_uri: /anything/redirect_uri
        logout_path: ${KC_LOGOUT}
        post_logout_redirect_uri: ${KC_POST_LOGOUT}
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
