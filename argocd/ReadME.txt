1st  download the installation file of install.yaml from here .

then create a namespace as argocd
--> create ns argocd
then,
k apply -f install.yaml -n argocd

wait and watch the installation .

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

then restart the argocd services and wait and watch.


After installation able to access via the web using the argocd generated password and the password changed after 1st login.

Get initial password using .
kubectl get secret argocd-initial-admin-secret -n argocd \
  -o jsonpath="{.data.password}" | base64 -d && echo
Now the argocd clustee not showing any apps pods aor services.
Lets conect all the running services to the argocd too.


first install argocd cli on server.
##
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64

##
check versin 
##
argocd version --client

##
if working then login to argocd 
#argocd login argocd.smileshosting.com
{"level":"warning","msg":"Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, use flag --grpc-web.","time":"2025-12-23T01:18:08Z"}
Username: admin
Password:
'admin:login' logged in successfully 
##
now enter this command to remove the warning 
"{"level":"warning","msg":"Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, use flag --grpc-web.","time":"2025-12-23T01:24:30Z"}
" 
export ARGOCD_OPTS="--grpc-web"
############
now it shows error free.
# argocd cluster list
SERVER                          NAME        VERSION  STATUS   MESSAGE                                                  PROJECT
https://kubernetes.default.svc  in-cluster           Unknown  Cluster has no applications and is not being monitored.
#######
To activate it create a simple test application or your application .
# argocd app create hello \
  --repo https://github.com/argoproj/argocd-example-apps.git \
  --path guestbook \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
application 'hello' created
# argocd cluster list
SERVER                          NAME        VERSION  STATUS      MESSAGE  PROJECT
https://kubernetes.default.svc  in-cluster  1.33     Successful

Now it seems to be healthy and you can see on dashboards too.
###########################################################################################################################
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
---------------------------------------------------------------------------------------------------------------------------
To test the applicatin using ingress a Templetes and ingress group folder is added inside argocd .
insede Tempeletes folder the actual config that pulls the data from the github repository of example argocd and applies to the argocd cluster
of.
Then we took one services from the multiple services named called  kustomize-guestbook-ui and exposed to the domain using port 80 internally.
 
