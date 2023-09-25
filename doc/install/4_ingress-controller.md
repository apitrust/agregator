
```
kubectl apply -f - <<EOF
apiVersion: apisix.apache.org/v2
kind: ApisixRoute
metadata:
  name: httpserver-route
spec:
  http:
  - name: rule1
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
