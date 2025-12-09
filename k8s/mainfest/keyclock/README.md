
Install via mainefest file

kubectl apply - kuycloak.yml

kubectl --namespace keycloak port-forward pod/keycloak-6cdcb44876-fstbc 8080

username: admin
password: admin

Oauth2 Proxy

kubectl create namespace auth


kubectl -n auth apply secret generic oauth2-proxy-secret \
  --from-literal=client-secret="8ifc6d2lBGHmusmKk8lK55w29yUenfwz"
