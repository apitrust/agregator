Update HELM
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh -v 3.12.3
mv /usr/local/bin/helm /usr/bin/helm
 
```
```
apt install nginx -y
kubectl get svc/apisix-gateway -n apisix
vi /etc/nginx/sites-enabled/default
vi /etc/nginx/nginx.conf
http {
  # proxy_buffer_size 32k;
  # proxy_buffers 8 16k;
  # proxy_busy_buffers_size 32k;
```
```
systemctl restart nginx.service
```
