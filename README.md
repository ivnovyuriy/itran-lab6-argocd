
# ArdoCD with Wordpress Application && Notifying to Telegram app

1. Installing ArgoCD and Proxifying it to our publicIP

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8080:443
```

2. Installing CLI 

```
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
```

3. Initialize password for ArgoCD

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

OUTPUT will be smth like this: HbjIRYnUjywH35JJ

4. ArgoCD login

Your credentials will looks like this

```
PublicIP:8080
admin / HbjIRYnUjywH35JJ
```

Login to ArgoCD via CLI

```
argocd login localhost:8080 --insecure
```

5. Create our first application && make our app sync automatically

```
argocd app create wordpress --repo https://github.com/ivnovyuriy/wordpress-chart.git --path helm --dest-server https://kubernetes.default.svc --dest-namespace default
argocd app sync wordpress
argocd app set wordpress --sync-policy automated
```
![Jenkins](https://github.com/ivnovyuriy/itran-lab5-jenkins/blob/bca906fb397429d2fcc7ed654f8aa344d0871942/img/1.png)
![Jenkins](https://github.com/ivnovyuriy/itran-lab5-jenkins/blob/bca906fb397429d2fcc7ed654f8aa344d0871942/img/1.png)

6. Add User deploy && manage user permissions

```
kubectl apply -f new-user.yaml
argocd account update-password --account deploy
kubectl apply -f user-perm.yaml
```
IMAGE

7. Check User permissions 

IMAGE


