Microservice 1
```
kubectl run httpecho --image=mendhak/http-https-echo --labels="app=httpecho"
kubectl create service clusterip httpecho --tcp=8080:8080
kubectl port-forward services/httpecho --address 0.0.0.0 8080
```
