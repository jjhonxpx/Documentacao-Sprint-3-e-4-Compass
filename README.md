
# Descrição das atividades solicitadas.

Uma documetação sobre a sprint 3 e 4.


# Instalação do Kubernetes no Oracle Linux 8 com Minikube ou S3


 
Instalação do Kubernetes no Oracle Linux 8 com Minikube ou S3
Este guia fornece instruções passo a passo para instalar e configurar o Kubernetes no Oracle Linux 8 usando a ferramenta de sua escolha: Minikube ou S3.

1. Preparativos Iniciais
Antes de começar, certifique-se de ter:

Uma máquina com Oracle Linux 8.
Privilégios de administrador no sistema.
2. Instalação do Minikube
2.1. Atualize o Sistema

```bash
sudo dnf update -y

```
2.2. Instale as Dependências
```bash
sudo dnf install -y conntrack

```
2.3. Baixe e Instale o Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
sudo dnf install -y ./minikube-latest.x86_64.rpm

```
2.4. Inicialize o Minikube

```bash
minikube start

```
2.5. Verifique o Status do Cluster
```bash
kubectl cluster-info

```

# Kubernetes: Implantação de Cluster com Jenkins no Oracle Linux

Instalação do Docker
```bash
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce --nobest -y
sudo systemctl start docker
sudo systemctl enable docker

```

Instalação do Kubernetes
Adicione o Repositório Kubernetes

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF


```
Instale os Pacotes do Kubernetes


```bash
sudo dnf install -y kubelet kubeadm kubectl

```
 Inicie e Ative o Serviço Kubelet

```bash
sudo systemctl start kubelet
sudo systemctl enable kubelet

```
Inicialização do Cluster Kubernetes

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

```

Configure o Kubeconfig para o Usuário Normal
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


```
Deploy do Jenkins
 Crie um Namespace para o Jenkins
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/baremetal/deploy.yaml


```
Acesse o Jenkins via Ingress


```bash
kubectl apply -f jenkins-ingress.yaml

```
# Desafio Kubernetes Avançado: Implantação de Cluster com Kubespray, Rancher e PostgreSQL no Oracle Linux

 Instalação do Ansible
```bash
sudo dnf install -y ansible

```

Clone do Repositório Kubespray


```bash
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray


```
Configuração do Kubespray

```bash
cp -rfp inventory/sample inventory/mycluster


```
Edite o Inventário

```bash
nano inventory/mycluster/inventory.ini


```
Implantação do Cluster com Kubespray

```bash
ansible-playbook -i inventory/mycluster/inventory.ini --become --become-user=root cluster.yml


```

Configuração do Ingress
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/baremetal/deploy.yaml


```
Exponha o Serviço Nginx para Fora do Cluster

```bash
kubectl expose deployment ingress-nginx-controller --target-port=80 --type=NodePort -n ingress-nginx


```
Implantação do Rancher no Kubernetes
Crie um Namespace para o Rancher

```bash
kubectl create namespace cattle-system


```

Instale o Rancher

```bash
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname=rancher-cluster.local


```

Crie um Namespace para o PostgreSQL

```bash
kubectl create namespace postgresql


```
Crie um Namespace para o PostgreSQL

```bash
kubectl apply -f postgresql-manifest.yaml



```