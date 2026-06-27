# kubernetes-atividade-01 - Grupo 1

Aplicação **TodoList - Grupo 01**

## 1. Criar o cluster Kind

```bash
kind create cluster --name kubernetes-atividade-01 --config kind-devops-labs.yaml
```

Verificar se o cluster foi criado:

```bash
kind get clusters
kubectl cluster-info --context kind-devops-labs
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

## 3. Aplicar os manifests (ordem correta)

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

# 6 - Deployment
kubectl apply -f k8s/deployment_6.yaml

# 7 - Ingress
kubectl apply -f k8s/ingress-service_7.yaml

# 8 - CronJob (limpeza a cada 5 min)
kubectl apply -f k8s/cronjob_8.yaml
```

## 4. Configurar o acesso local

Adicionar a entrada no `/etc/hosts`:

```bash
echo "127.0.0.1 todolist-grupo-01.local" | sudo tee -a /etc/hosts
```

Acessar a aplicação em: http://todolist-grupo-01.local

## 5. Verificar o ambiente

```bash
kubectl get all -n todolist-grupo-01
kubectl get configmap -n todolist-grupo-01
kubectl get secret -n todolist-grupo-01
kubectl get ingress -n todolist-grupo-01
kubectl get cronjob -n todolist-grupo-01
```

## 6. Deletar o cluster

```bash
kind delete cluster --name kubernetes-atividade-01
```