🔐 cert-manager FULL COMMAND LIST (Kubernetes)
📦 cert-manager installation & status
kubectl get pods -n cert-manager
kubectl get deploy -n cert-manager
kubectl get svc -n cert-manager
kubectl logs -n cert-manager deploy/cert-manager --tail=100


Restart cert-manager:

kubectl rollout restart deploy/cert-manager -n cert-manager

🧾 ClusterIssuer / Issuer commands

List issuers:

kubectl get clusterissuer
kubectl get issuer -A


Describe issuer:

kubectl describe clusterissuer letsencrypt-prod
kubectl describe issuer -n apache


Delete issuer:

kubectl delete clusterissuer letsencrypt-prod

📜 Certificate commands

List certificates:

kubectl get certificate -A


Describe certificate:

kubectl describe certificate lbk8s-smileshosting-tls -n apache


Delete certificate:

kubectl delete certificate lbk8s-smileshosting-tls -n apache

📄 CertificateRequest (VERY IMPORTANT)

List:

kubectl get certificaterequest -A


Describe:

kubectl describe certificaterequest -n apache


Delete:

kubectl delete certificaterequest -n apache --all

🧩 ACME Order & Challenge (debug level)

List:

kubectl get order -A
kubectl get challenge -A


Describe:

kubectl describe order -n apache
kubectl describe challenge -n apache


Delete (force retry):

kubectl delete order -n apache --all
kubectl delete challenge -n apache --all

🔑 Secrets (TLS & account keys)

List secrets:

kubectl get secret -n apache
kubectl get secret -n cert-manager


TLS secret details:

kubectl describe secret lbk8s-smileshosting-tls -n apache


Delete TLS secret:

kubectl delete secret lbk8s-smileshosting-tls -n apache


Delete ACME account key:

kubectl delete secret letsencrypt-prod-account-key -n cert-manager

🌐 Ingress & Traefik (cert related)

List ingress:

kubectl get ingress -A


Describe ingress:

kubectl describe ingress apache-ingress -n apache


Ingress YAML:

kubectl get ingress apache-ingress -n apache -o yaml


Restart Traefik:

kubectl rollout restart deploy/traefik -n kube-system


Traefik logs:

kubectl logs -n kube-system deploy/traefik --tail=100

🔍 Live watch (useful)

Watch cert progress:

kubectl get cert -n apache -w


Watch secrets:

kubectl get secret -n apache -w


Watch challenges:

kubectl get challenge -n apache -w

🔄 FULL RESET (nuclear option)

⚠️ Use only if stuck

kubectl delete cert lbk8s-smileshosting-tls -n apache
kubectl delete secret lbk8s-smileshosting-tls -n apache
kubectl delete certificaterequest -n apache --all
kubectl delete order -n apache --all
kubectl delete challenge -n apache --all
kubectl rollout restart deploy/cert-manager -n cert-manager
kubectl rollout restart deploy/traefik -n kube-system


Then reapply ingress:

kubectl apply -f apache-ingress.yaml

✅ Final success checks
kubectl get clusterissuer
kubectl get cert -n apache
kubectl get secret -n apache | grep tls


Expected:

READY   True
kubernetes.io/tls

🧠 Pro tip (save time later)

Check ACME rate limit:

kubectl logs -n cert-manager deploy/cert-manager | grep -i rate


Test staging issuer:

https://acme-staging-v02.api.letsencrypt.org/directory
