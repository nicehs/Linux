## 10.2. Kubernetes

Về bản chất kubernetes là 1 platform dùng để quản trị cluster

More info: [Giới thiệu và cài đặt Kubernetes Cluster](https://xuanthulab.net/gioi-thieu-va-cai-dat-kubernetes-cluster.html)

### 1. Install kubernetes
You need install docker first

**Centos 7**
Add repo

```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```

**Ubuntu

### 2. Config master node
**Configure init setup on Master Node**

```
kubeadm init --apiserver-advertise-address=192.168.53.144 --pod-network-cidr=10.244.0.0/16
```
--apiserver-advertise-address: địa chỉ ip cua api kubernetes

--pod-network-cidr: địa chỉ của pod network

**Configure Pod Network with Pod**

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

**Set cluster admin**
```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config 
```

**Configure Pod network with Flannel**
Flannel is a tool connect network between pod nodes
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

**Check master node**
```
kubectl get nodes

NNAME    STATUS   ROLES           AGE     VERSION
cent7   Ready    control-plane   8m36s   v1.24.2
```

### 3. Configure worker node
**Print command to domain**
```
kubeadm token create --print-join-command
```

**Join worker to cluster**
```
kubeadm join 192.168.53.144:6443 --token 7sx8k0.6t5peiddmsyzs86e \
        --discovery-token-ca-cert-hash sha256:5bc2e126d0a020d352336b567888c1b4aca487c94a11bc51df932eebcc1f952d
```

### 4. Deploy pod
**Create pod**
```
kubectl create deployment test-nginx --image=nginx
```

**Check pod**
```
kubectl get pods

NAME                         READY   STATUS    RESTARTS   AGE
test-nginx-59ffd87f5-9dkwg   1/1     Running   0          26s
```

**Access to pod**
```
kubectl exec -it test-nginx-59ffd87f5-9dkwg -- bash
```

**Delete pod**
```
kubectl delete deployment test-nginx
```

**Get logs from pod**
```
kubectl logs test-nginx-59ffd87f5-9dkwgx
```

### 5. Service

**Create service**
```
kubectl expose deployment test-nginx --type="NodePort" --port 80
```
--type: NodePort giống như standalone server. Loadbalancer cân bằng tải giữa các node
--port: port của pod

**Check service**
```
kubectl get svc

NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        3h2m
test-nginx   NodePort    10.104.180.225   <none>        80:30753/TCP   6s
```

Truy cập vào theo đường dẫn http://<ip>:30753