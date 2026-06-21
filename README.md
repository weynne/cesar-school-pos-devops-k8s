# CESAR School — Pós DevOps: Kubernetes Labs

Repositório com os labs práticos do módulo de Orquestração de Containers com Kubernetes.

---

## Lab 1 — Workloads + Acesso + Persistência

Deploy completo do **TodoList** no cluster Kubernetes, cobrindo: Namespace, ConfigMap, Secret, PVC, Deployment, Service, Ingress e CronJob.

### Pré-requisitos

- Cluster [kind](https://kind.sigs.k8s.io/) rodando localmente
- NGINX Ingress Controller instalado no cluster
- `kubectl` configurado apontando para o cluster

### Estrutura dos manifests

```
lab1/
├── namespace.yaml
├── configmap.yaml
├── secret.yaml
├── pvc.yaml
├── deployment.yaml
├── service.yaml
├── ingress.yaml
└── cronjob.yaml
```

### Como subir o ambiente

> Aplique os manifests na ordem abaixo. Cada passo depende do anterior.

#### 1. Namespace

Cria o namespace isolado para todos os recursos do lab.

```bash
kubectl apply -f lab1/namespace.yaml
```

#### 2. ConfigMap

Injeta as variáveis de configuração da aplicação (`APP_NAME`, `APP_PORT`, `APP_COLOR`).

```bash
kubectl apply -f lab1/configmap.yaml
```

#### 3. Secret

Injeta as credenciais e tokens sensíveis (`SESSION_KEY`, `ADMIN_USER`, `ADMIN_PASSWORD`, `CLEANUP_TOKEN`).

```bash
kubectl apply -f lab1/secret.yaml
```

#### 4. PersistentVolumeClaim

Provisiona o volume de 500Mi onde o banco SQLite (`/data/todos.db`) será armazenado.

```bash
kubectl apply -f lab1/pvc.yaml
```

#### 5. Deployment

Sobe a aplicação TodoList com 2 réplicas, montando o PVC em `/data` e consumindo ConfigMap e Secret via `envFrom`.

```bash
kubectl apply -f lab1/deployment.yaml
```

#### 6. Service

Expõe o Deployment internamente via ClusterIP na porta 80, redirecionando para a porta 5000 do container.

```bash
kubectl apply -f lab1/service.yaml
```

#### 7. Ingress

Roteia o host `todolist-grupo-05.local` para o Service, permitindo acesso via navegador.

```bash
kubectl apply -f lab1/ingress.yaml
```

#### 8. CronJob

Agenda a rotina de limpeza automática a cada 5 minutos, chamando `POST /cleanup` com o token do Secret.

```bash
kubectl apply -f lab1/cronjob.yaml
```

### Verificação

```bash
kubectl get all -n todolist-grupo-05
kubectl get pvc,ingress,cronjob -n todolist-grupo-05
```

### Acesso via navegador

Adicione a entrada abaixo em `/etc/hosts`:

```
127.0.0.1 todolist-grupo-05.local
```

Acesse: [http://todolist-grupo-05.local](http://todolist-grupo-05.local)

---

## Lab 2 — *em breve*
