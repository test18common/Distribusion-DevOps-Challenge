helm repo add jetstack https://charts.jetstack.io
helm repo update

kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.crds.yaml

kubectl create namespace cert-manager

values.yml

ingressShim:
  defaultIssuerName: "letsencrypt-prod"
  defaultIssuerKind: "ClusterIssuer"


  helm install cert-manager jetstack/cert-manager --values values.yaml -n cert-manager


# File name is `cluster-issuer.yaml`
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  # The name should be the same with `defaultIssuerName` above
  name: letsencrypt-prod
  namespace: cert-manager
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    # Replace with your domain email.
    email: support@drunkcoding.net
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
      - http01:
          ingress:
            # The ingress class name of nginx.
            class: nginx


kubectl apply -f cluster-issuer.yaml


apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: echo-ingress
  namespace: default
  annotations:
    # 1. enable cert-manager for this ingress
    kubernetes.io/tls-acme: "true"
spec:
  ingressClassName: nginx
  # 3. Config the tls secret name. Replace the domain and secretName below with your config accordingly.
  tls:
    - hosts:
        - echo.drunkcoding.net
      secretName: tls-drunkcoding-net
  rules:
    - host: echo.drunkcoding.net
      http:
        paths:
          - pathType: Prefix
            path: "/"
            backend:
              service:
                name: echo-service
                port:
                  number: 80
