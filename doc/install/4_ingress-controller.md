
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
```
kubectl apply -n keycloak -f - <<EOF
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
```
kubectl port-forward -n apisix svc/apisix-gateway --address 0.0.0.0 80
```
