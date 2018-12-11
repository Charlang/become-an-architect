# Kubernetes CLI quick commands:

* Install minikube
brew update && brew install kubectl && brew cask install docker minikube virtualbox

* build a docker image
docker build -t markotraining .
docker tag markotraining chalang/markotraining:2
docker login
docker push chalang/markotraining:2

* Run an docker image
docker run --name kubia-container -p 3001:3001 -d kubia

delete all Docker Containers
docker container rm -f $(docker container ls -a -q)

* get inside a container
docker container run -it --name sample kubia /bin/bash

* Run Image in kubenetes
kubectl run kubia --image=chalang/kubianew --port=8080 --generator=run/v1

* get Pods info
kubectl get pods
kubectl get pods -o wide
kubectl describe pod kubia-rr8b6

* Forward a local network port to a port in the pod
kubectl port-forward kubia-bnqvm 8888:3001

* node debug
kubectl check nodes
kubectl get nodes
kubectl describe node k8s-vm-minion-1761013
kubectl cluster-info

* create service object
kubectl expose rc kubia --type=LoadBalancer --name kubia-http
kubectl expose rc kubia --type=NodePort --name kubia-http
kubectl get services
minikube service kubia-http

* Get replications list:
kubectl get replicationcontrollers

* change replicas
kubectl scale rc kubia --replicas=3

* Switch kubernetes cluster
kubectl config get-contexts
kubectl config use-context kubernetes-admin@kubernetes
