## Setup Guide
0. Install MicroK8s:
   ```
     $ snap install microk8s --classic
     $ microk8s enable dns && microk8s enable helm3
   ```
1. Apply namespaces.yaml
2. Create Tunnel secret:
   ```
     $ kubectl create secret generic tunnel-credentials \
         --from-file=ironforge-ingress.json=/etc/cloudflared/<ingress-tunnel-ID>.json \
         --namespace argotunnel
   ```
3. Obtain the MicroK8s CA pool in PEM format and supply it to cloudflared:
   ```
     $ openssl x509 \
         -in /var/snap/microk8s/current/certs/ca.crt \
         -out /etc/cloudflared/k8s-ca.pem \
         -outform PEM
   ```
4. Provision DNS records in Cloudflare

## Remarks
### MicroK8s CA Pool
MicroK8s CA pool needs to be accessible to `cloudflared` in PEM format for requests
to be routable to MicroK8s endpoints. The pool must be configured for `cloudflared`
in the `ingress` section using the `caPool` key under `originRequest`.

