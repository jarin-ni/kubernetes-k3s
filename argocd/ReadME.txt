Note:
while runing argocd using the nginx ingress i faced multiple issues at while deploying ssl.
1. multiple  redireaction issue 
2. 404 isue
3. internal error issue.
.

This was fixed by placing the insecure inside the argocd  config file.

use This .
###################################################
 kubectl apply -n argocd -f - <<'EOF'
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cmd-params-cm
data:
  server.insecure: "true"
EOF
##################################################
