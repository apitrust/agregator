```
kubectl get -n apisix svc/apisix-gateway
```
```
kubectl port-forward -n apisix svc/apisix-gateway --address 0.0.0.0 9099:80
```
```
kubectl apply -n keycloak -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: keycloak1-route
spec:
  http:
  - name: keycloak1
    match:
      paths:
      - /
      - /admin/*
      - /resources/*
      - /realms/*
      - /welcome-content/*
    backends:
       - serviceName: auth-keycloak
         servicePort: 80
EOF

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
  name: httpecho1-route
spec:
  http:
  - name: httpecho1
    match:
      paths:
      - /anything/echo
    upstreams:
    - name: httpbin-upstream
EOF

```
```
kubectl apply -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: httpecho1-route
spec:
  http:
  - name: httpecho1
    match:
      paths:
      - /echo
    backends:
       - serviceName: httpecho
         servicePort: 8080
EOF

```
