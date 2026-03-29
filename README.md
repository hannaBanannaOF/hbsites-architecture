# Liminal Labs Architecture
Architecture and design files for all my sites  
*(**Liminal Labs** was just a proxy name I came up, not business related)*

### Running stuff

#### Docker compose
- Globals:
```shell
docker-compose -f composes/docker-dev/docker-compose.globals-dev.yml -p globals up -d
```

- Auth:
```shell
docker-compose -f composes/docker-dev/docker-compose.auth-dev.yml -p auth up -d
```

- QuestMaster:
```shell
docker-compose -f composes/docker-dev/docker-compose.questmaster-dev.yml -p questmaster up -d
```

#### K8s:
- Instalar Cert Manager e selfsigned issuer:
```shell
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml
kubectl apply -f k8/dev/cluster_issuer.yml 
```

- Instalar o NGINX Ingress Controller:
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

- Instalar RabbitMQCluster e Messaging Topology:
```shell
kubectl apply -f https://github.com/rabbitmq/cluster-operator/releases/latest/download/cluster-operator.yml
kubectl apply -f https://github.com/rabbitmq/messaging-topology-operator/releases/latest/download/messaging-topology-operator-with-certmanager.yaml
```

- Auth:
```shell
kubectl apply -f k8/dev/auth/namespace.yml
kubectl apply -R -f k8/dev/auth/database
kubectl apply -R -f k8/dev/auth/kc
kubectl scale deployment --all --replicas=0 -n auth # Para poupar recursos quando não está utilizando
kubectl scale statefulset --all --replicas=0 -n auth # Para poupar recursos quando não está utilizando
```
> Lembrete: Importar o realm em ```./realm``` no keycloak!

- Globals:
```shell
kubectl apply -f k8/dev/globals/namespace.yml
kubectl apply -R -f k8/dev/globals/rabbitmq
kubectl scale statefulset --all --replicas=0 -n globals # Para poupar recursos quando não está utilizando
```

- Questmaster:
```shell
kubectl apply -f k8/dev/questmaster/namespace.yml
kubectl apply -f k8/dev/questmaster/gateway/rabbitmq/queue.yml
kubectl apply -f k8/dev/questmaster/gateway/rabbitmq/exchange.yml
kubectl apply -f k8/dev/questmaster/gateway/rabbitmq/user.yml
kubectl apply -f k8/dev/questmaster/gateway/rabbitmq/permissions.yml
kubectl apply -R -f k8/dev/questmaster/gateway/database
kubectl apply -R -f k8/dev/questmaster/gateway/app
kubectl apply -R -f k8/dev/questmaster/permissions_db
kubectl apply -f k8/dev/questmaster/core/rabbitmq/binding.yml
# kubectl apply -R -f k8/dev/questmaster/core/app
```

##### Opcional: Kubernetes metrics
```shell
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl edit deployment metrics-server -n kube-system
```
Adicionar nos args do ```container```:
```yaml
- --kubelet-insecure-tls
```
Rollout novamente:
```shell
kubectl rollout restart deployment metrics-server -n kube-system
```

##### Opcional: RabbitMQ Management UI
Para acessar a UI, precisa do username e senhas gerados pelo deployment do RabbitMQCluster:
```shell
kubectl get secret rabbitmq-default-user -n globals -o jsonpath='{.data.username}' | base64 --decode
kubectl get secret rabbitmq-default-user -n globals -o jsonpath='{.data.password}' | base64 --decode
```