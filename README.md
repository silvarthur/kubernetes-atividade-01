# kubernetes-atividade-01 - Grupo 1

Aplicação **TodoList - Grupo 01**

## 1. Criar o cluster Kind

```bash
kind create cluster --name kubernetes-atividade-01 --config kind-devops-labs.yaml
```

Verificar se o cluster foi criado:

```bash
kind get clusters
kubectl cluster-info --context kind-kubernetes-atividade-01
```

## 2. Instalar o Ingress Controller (NGINX)

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/kind/deploy.yaml
```

Aguardar o controller ficar pronto:

```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```

## 3. Instalar o Metrics Server (necessário para o HPA)

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl patch -n kube-system deployment metrics-server --type=json \
  -p '[{"op":"add","path":"/spec/template/spec/containers/0/args/-","value":"--kubelet-insecure-tls"}]'
```

## 4. Aplicar os manifests (ordem correta)

```bash
# 1 - Namespace
kubectl apply -f k8s/namespace_1.yaml

# 2 - ConfigMap (variáveis não sensíveis)
kubectl apply -f k8s/configmap_2.yaml

# 3 - Secret (variáveis sensíveis)
# Os valores em secret_3.yaml são apenas exemplos para teste rápido em laboratório.
kubectl apply -f k8s/secret_3.yaml

# 4 - PersistentVolumeClaim
kubectl apply -f k8s/persistent-volume-claim_4.yaml

# 5 - Service (ClusterIP)
kubectl apply -f k8s/cluster-ip-service_5.yaml

# 6 - Service Account
kubectl apply -f k8s/service-account_6.yaml

# 7 - Role
kubectl apply -f k8s/role_7.yaml

# 8 - Role Binding
kubectl apply -f k8s/role-binding_8.yaml

# 9 - Deployment
kubectl apply -f k8s/deployment_9.yaml

# 10 - Ingress
kubectl apply -f k8s/ingress-service_10.yaml

# 11 - CronJob (limpeza a cada 5 min)
kubectl apply -f k8s/cronjob_11.yaml

# 12 - HorizontalPodAutoscaler
kubectl apply -f k8s/hpa_12.yaml

# 13 - PodDisruptionBudget
kubectl apply -f k8s/pdb_13.yaml
```

## 5. Configurar o acesso local

Adicionar a entrada no `/etc/hosts`:

```bash
echo "127.0.0.1 todolist-grupo-01.local" | sudo tee -a /etc/hosts
```

Acessar a aplicação em: http://todolist-grupo-01.local

## 6. Verificar o ambiente

```bash
kubectl get all -n todolist-grupo-01
kubectl get configmap -n todolist-grupo-01
kubectl get secret -n todolist-grupo-01
kubectl get ingress -n todolist-grupo-01
kubectl get cronjob -n todolist-grupo-01

kubectl get deployment,hpa,pdb,sa,role,rolebinding -n todolist-grupo-01
```

## 7. Verificar permissões

```bash
kubectl auth can-i get pods --as=system:serviceaccount:todolist-grupo-01:todo-list-sa -n todolist-grupo-01

kubectl auth can-i delete pods --as=system:serviceaccount:todolist-grupo-01:todo-list-sa -n todolist-grupo-01
```

## 8. Deletar o cluster

```bash
kind delete cluster --name kubernetes-atividade-01
```