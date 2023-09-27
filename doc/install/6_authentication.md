```
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=dLuzOP4Z0OkZC01B8zftfadn4vUqAKLc
KC_DISCOVERY_ENDPOINT="https://dc034e88-2130-4759-9100-510b7a122f87-10-244-5-177-80.spch.r.killercoda.com/realms/apisix/.well-known/openid-configuration"
KC_TOKEN_ENDPOINT="https://38613ce3-12cc-4c0e-a13e-184a349e7578-10-244-0-248-80.spch.r.killercoda.com/realms/apisix/protocol/openid-connect/token"
KC_REALM=apisix

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
        redirect_uri: "https://dc034e88-2130-4759-9100-510b7a122f87-10-244-5-177-80.spch.r.killercoda.com/echo"
EOF

unset KC_CLIENT_ID
unset KC_CLIENT_SECRET
unset KC_DISCOVERY_ENDPOINT
unset KC_REALM

```
```
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=6nZTyezkfYM1YdiVuknMdmhzv1PDdej0
KC_DISCOVERY_ENDPOINT="https://38613ce3-12cc-4c0e-a13e-184a349e7578-10-244-0-248-80.spch.r.killercoda.com"
KC_REALM=apisix

kubectl apply -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: http-route-auth1
spec:
  http:
  - name: http-auth1
    match:
      paths:
      - /auth
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
        redirect_uri: "https://38613ce3-12cc-4c0e-a13e-184a349e7578-10-244-0-248-80.spch.r.killercoda.com/echo"
        access_token_in_authorization_header: true
        ssl_verify: false
EOF

unset KC_CLIENT_ID
unset KC_CLIENT_SECRET
unset KC_DISCOVERY_ENDPOINT
unset KC_REALM

```
