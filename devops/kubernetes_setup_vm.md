# Records of cmd(DRAFT):

    1  apt-get update && apt-get install -y apt-transport-https curl

    2  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

    3  cat <<EOF >/etc/apt/sources.list.d/kubernetes.list

       deb http://apt.kubernetes.io/ kubernetes-xenial main

       EOF

    4  apt-get update

    5  apt-get install -y kubelet kubeadm kubectl

    6  apt-mark hold kubelet kubeadm kubectl

    7  apt-get install -y docker.io kubernetes-cni

    8  which kubectl

    9  which kubadm

    10  which kubeadm

    11  docker ps

i.e :
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube

  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.

Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:

  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node

as root:

  kubeadm join xxx.xxx.xxx.xxx:6443 --token xxx.xxx --discovery-token-ca-cert-hash sha256:xxxxx

    13 setup wave network

        kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

    14 check kubernetes cluser status:

        kubectl get nodes

    15 enable localhost of kubernetes:
        kubectl proxy
