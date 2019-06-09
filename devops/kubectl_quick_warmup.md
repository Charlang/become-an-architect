# Kubernetes CLI quick commands(DRAFT):

* Install minikube

`
brew update && brew install kubectl && brew cask install docker minikube virtualbox
`

* build a docker image

`
docker build -t markotraining .
docker tag markotraining chalang/markotraining:2
docker login
docker push chalang/markotraining:2
`

* Run an docker image

`
docker run --name kubia-container -p 3001:3001 -d kubia
`

* delete all Docker Containers

`
docker container rm -f $(docker container ls -a -q)
`

* get inside a container

`
docker container run -it --name sample kubia /bin/bash
`

* Run Image in kubenetes

`
kubectl run kubia --image=chalang/kubianew --port=8080 --generator=run/v1
`

* get Pods info

`
kubectl get pods
kubectl get pods -o wide
kubectl describe pod kubia-rr8b6
`

* Forward a local network port to a port in the pod

`
kubectl port-forward kubia-bnqvm 8888:3001
`

* node debug

`
kubectl check nodes
kubectl get nodes
kubectl describe node k8s-vm-minion-1761013
kubectl cluster-info
`

* create service object

`
kubectl expose rc kubia --type=LoadBalancer --name kubia-http
kubectl expose rc kubia --type=NodePort --name kubia-http
kubectl get services
minikube service kubia-http
`

* Get replications list:

`
kubectl get replicationcontrollers
`

* change replicas

`
kubectl scale rc kubia --replicas=3
`

* Switch kubernetes cluster

`
kubectl config get-contexts
kubectl config use-context kubernetes-admin@kubernetes
`

* Rolling out with Deployment:

`
kubectl rollout status deployment markotraining
kubectl rollout undo deployment markotraining
kubectl rollout history deployment markotraining
`

* Create ssl

`
openssl genrsa -out https.key 2048
openssl req -new -x509 -key https.key -out https.cert -days 3650 -subj CN=xxx.com
kubectl create secret generic markotraining-secret --from-file=https.key --from-file=https.cert
`

* Talking to the API server from within a pod:
Disabling role-based access control (RBAC)

`
kubectl create clusterrolebinding permissive-binding --clusterrole=cluster-admin --group=system:serviceaccounts
export CURL_CA_BUNDLE=/var/run/secrets/kubernetes.io/serviceaccount/ca.crt
TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
curl -H "Authorization: Bearer $TOKEN" https://kubernetes
`

* Deploy with private image:

kubectl delete secret regcred
kubectl create secret docker-registry regcred --docker-username=chalang --docker-password=XXXXX --docker-email=xxx@xxx.com

kubectl get secret regcred --output=yaml
kubectl get secret regcred --output="jsonpath={.data.\.dockerconfigjson}" | base64 --decode

`
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
  name: markotraining
spec:
  replicas: 3
  selector:
    matchLabels:
      app: markotraining
  template:
    metadata:
      labels:
        app: markotraining
    spec:
      containers:
      - name: markotraining
        image: chalang/markotraining:1
      imagePullSecrets:
      - name: regcred
  `
  
Use NFS:

https://help.ubuntu.com/lts/serverguide/network-file-system.html.en

/root/nfs xx.xx.xx.xx(rw,sync,fsid=0,no_root_squash,crossmnt,no_subtree_check,no_acl) 10.148.213.251(rw,sync,fsid=0,no_root_squash,crossmnt,no_subtree_check,no_acl) 10.148.213.38(rw,sync,fsid=0,no_root_squash,crossmnt,no_subtree_check,no_acl)
