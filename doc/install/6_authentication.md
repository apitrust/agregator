```
KC_CLIENT_ID=apisix1
KC_CLIENT_SECRET=Xpx9FYacA2o7O5RTsP62a7pQOou5nzqv
KC_DISCOVERY_ENDPOINT=https://dc034e88-2130-4759-9100-510b7a122f87-10-244-5-177-80.spch.r.killercoda.com//realms/apisix/.well-known/openid-configuration
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
    plugins:
    - name: openid-connect
      enable: true
      config:
        client_id: ${KC_CLIENT_ID}
        client_secret: ${KC_CLIENT_SECRET}
        discovery: ${KC_DISCOVERY_ENDPOINT}
        realm: ${KC_REALM}
        redirect_uri: /auth
        access_token_in_authorization_header: true
        ssl_verify: false
EOF
```
