```
kubectl logs -n apisix pods/apisix-
```
```
kubectl exec -n apisix pods/apisix-
```
```
kubectl exec -n apisix pods/apisix- -- curl 127.0.0.1:9080/auth/redirect_uri
```
```
kubectl delete apisixroutes.apisix.apache.org httpecho1-route-auth

```
