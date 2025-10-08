# 🚀 TechFleet - App Portal Kubernetes

**Desenvolvido por:** William Coelho  
**RM:** 556336  
**Projeto:** CP-02 - Kubernetes TechFleet  

## 📋 Cenário do Projeto

**Empresa:** TechFleet  
**Objetivo:** Migração de aplicações para containers orquestrados em Kubernetes  
**Desafio:** Configurar ambiente de produção simulado com alta disponibilidade, escalabilidade e resiliência

---

## 🎯 Requisitos Técnicos Implementados

| Requisito | Especificação | Status |
|-----------|---------------|--------|
| **Cluster** | Kubernetes local (Kind/Minikube/AWS) | ✅ |
| **Namespace** | `producao` | ✅ |
| **Imagem** | `nginx:latest` | ✅ |
| **Deployment** | `app-portal` / 3 réplicas / label `app: app-portal` | ✅ |
| **Service** | NodePort, porta externa 30080, porta interna 80 | ✅ |
| **Escalabilidade** | Escalar para 5 réplicas | ✅ |
| **Resiliência** | Simular exclusão de Pod e observar recuperação | ✅ |
| **Customização** | Mensagem personalizada no index.html | ✅ |

---

## 📁 Estrutura do Projeto

```
CP-02-William-K8s/
├── docs/                             # Pasta para prints/evidências
│   ├── README.md                     # Guia de nomenclatura dos prints
│   ├── 01-cluster-info.png           # Cluster Kind funcionando
│   ├── 02-namespace.png              # Namespace producao criado
│   ├── 03-recursos-gerais.png        # Todos recursos aplicados
│   ├── 04-pods-iniciais.png          # Pods com 3 réplicas
│   ├── 05-service.png                # Service NodePort 30080
│   ├── 06-antes-scale.png            # Antes escalabilidade (3 pods)
│   ├── 07-depois-scale.png           # Depois escalabilidade (5 pods)
│   ├── 08-antes-exclusao.png         # Antes teste resiliência
│   ├── 09-depois-recuperacao.png     # Depois recuperação automática
│   ├── 10-aplicacao-funcionando.png  # Aplicação acessível
│   └── 11-aplicacao-funcionando.png  # Evidência adicional
├── namespace.yaml                    # Definição do namespace producao
├── configmap.yaml                    # HTML customizado para a aplicação
├── deployment.yaml                   # Deployment com 3 réplicas
├── service.yaml                      # Service NodePort na porta 30080
└── README.md                         # Esta documentação completa
```

---

## 🚀 Guia de Execução Passo a Passo

### **🔴 ETAPA 0: CONFIGURAR CLUSTER KIND (EXECUTE PRIMEIRO!)**

**ANTES DE TUDO:** Configure um cluster Kubernetes local com Kind:

#### **Instalar Kind (se não tiver):**

**Baixar Kind:**
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
```

**Tornar executável:**
```bash
chmod +x ./kind
```

**Mover para PATH:**
```bash
sudo mv ./kind /usr/local/bin/kind
```

**Verificar instalação:**
```bash
kind version
```

#### **Criar cluster:**

**Criar cluster Kind:**
```bash
kind create cluster --name techfleet-cluster
```

**Configurar contexto:**
```bash
kubectl config use-context kind-techfleet-cluster
```

**Verificar se funcionou:**
```bash
kubectl cluster-info
```

**📸 EVIDÊNCIA:** `kubectl cluster-info`

![Cluster Info](docs/01-cluster-info.png)


### **Pré-requisitos - Verificação:**

**Verificar kubectl:**
```bash
kubectl version --client
```

**Verificar cluster ativo:**
```bash
kubectl cluster-info
```

**Ver contexto atual:**
```bash
kubectl config current-context
```

**Usar contexto correto:**
```bash
kubectl config use-context kind-techfleet-cluster
```
---

### **Etapa 1: Criar o Namespace**

**Aplicar o namespace:**
```bash
kubectl apply -f namespace.yaml
```

**Verificar se foi criado:**
```bash
kubectl get namespaces | grep producao
```

**📸 EVIDÊNCIA:**

![Namespace](docs/02-namespace.png)

**Resultado esperado:**
```
namespace/producao created
producao   Active   <age>
```

---

### **Etapa 2: Aplicar o ConfigMap (HTML Customizado)**

**Aplicar o ConfigMap:**
```bash
kubectl apply -f configmap.yaml
```

**Verificar se foi criado:**
```bash
kubectl get configmap -n producao
```

**Resultado esperado:**
```
configmap/app-portal-html created
NAME              DATA   AGE
app-portal-html   1      <age>
```

---

### **Etapa 3: Criar o Deployment (3 Réplicas)**

**Aplicar o Deployment:**
```bash
kubectl apply -f deployment.yaml
```

**Verificar o status do deployment:**
```bash
kubectl get deployment -n producao
```

**Ver os pods sendo criados:**
```bash
kubectl get pods -n producao -w
```

**Resultado esperado:**
```
deployment.apps/app-portal created
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
app-portal   3/3     3            3           <age>
```

---

### **Etapa 4: Criar o Service NodePort**

**Aplicar o Service:**
```bash
kubectl apply -f service.yaml
```

**Verificar o service:**
```bash
kubectl get svc -n producao
```

**Ver detalhes do service:**
```bash
kubectl describe svc app-portal-service -n producao
```

**📸 EVIDÊNCIA:** `kubectl get svc -n producao`

![Service](docs/05-service.png)

**Resultado esperado:**
```
service/app-portal-service created
NAME                 TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
app-portal-service   NodePort   10.96.xxx.xxx   <none>        80:30080/TCP   <age>
```

---

### **Etapa 5: Verificar Status Geral**

**Ver todos os recursos no namespace producao:**
```bash
kubectl get all -n producao
```

**📸 EVIDÊNCIA:** 

![Recursos Gerais](docs/03-recursos-gerais.png)

**Ver labels dos pods:**
```bash
kubectl get pods -n producao --show-labels
```

**Verificar se os pods estão prontos:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**📸 EVIDÊNCIA:**

![Pods Iniciais](docs/04-pods-iniciais.png)

---

### **Etapa 6: Testar Conectividade**

**Método 1: Port-forward:**
```bash
kubectl port-forward svc/app-portal-service 8080:80 -n producao
```

**📸 EVIDÊNCIA (Opcional):**

![Aplicação Funcionando](docs/10-aplicacao-funcionando.png)

**📸 EVIDÊNCIA ADICIONAL:**

![Aplicação Funcionando - Adicional](docs/11-aplicacao-funcionando.png)

**💡 Dica:** Use Ctrl+C para parar o port-forward

---

### **Etapa 7: Teste de Escalabilidade (3 → 5 réplicas)**

**Estado inicial (3 réplicas):**
```bash
kubectl get pods -n producao -l app=app-portal
```

**Escalar para 5 réplicas:**
```bash
kubectl scale deployment app-portal --replicas=5 -n producao
```

**Verificar o processo de scaling:**
```bash
kubectl get pods -n producao -l app=app-portal -w
```

**Aguardar todos ficarem prontos:**
```bash
kubectl wait --for=condition=ready pod -l app=app-portal -n producao --timeout=120s
```

**Verificar resultado final:**
```bash
kubectl get deployment -n producao
```

**Comandos para evidências:**

**Antes do scale:**
```bash
echo "=== ANTES DO SCALE ==="
```

**Ver pods antes:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**📸 EVIDÊNCIA:**

![Antes Scale](docs/06-antes-scale.png)

**Depois do scale:**
```bash
echo "=== DEPOIS DO SCALE ==="
```

**Ver pods depois:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**📸 EVIDÊNCIA:**
![Depois Scale](docs/07-depois-scale.png)

---

### **Etapa 8: Teste de Resiliência (Exclusão de Pod)**

**Ver pods atuais:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**Pegar o nome de um pod para deletar:**
```bash
POD_NAME=$(kubectl get pods -n producao -l app=app-portal -o jsonpath='{.items[0].metadata.name}')
```

**Mostrar pod a ser deletado:**
```bash
echo "Pod a ser deletado: $POD_NAME"
```

**Deletar o pod:**
```bash
kubectl delete pod $POD_NAME -n producao
```

**Observar a recuperação automática:**
```bash
kubectl get pods -n producao -l app=app-portal -w
```

**Verificar que um novo pod foi criado:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**Para evidências:**

**Ver pods antes:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**📸 EVIDÊNCIA:**

![Antes Exclusão](docs/08-antes-exclusao.png)

**Executar exclusão:**
```bash
echo "=== DELETANDO POD ==="
```

**Deletar pod:**
```bash
kubectl delete pod $POD_NAME -n producao
```

**Aguardar 10 segundos:**
```bash
sleep 10
```

**Após recuperação:**
```bash
echo "=== APÓS RECUPERAÇÃO ==="
```

**Ver pods após:**
```bash
kubectl get pods -n producao -l app=app-portal
```

**📸 EVIDÊNCIA:**

![Depois Recuperação](docs/09-depois-recuperacao.png)

---

**Para remover tudo (quando terminar os testes):**
```bash
kubectl delete -f .
```

**Deletar cluster Kind:**
```bash
kind delete cluster --name techfleet-cluster
```

**Desenvolvido por:** William  
**Projeto:** CP-02 - Kubernetes TechFleet  
**Data:** Outubro 2025