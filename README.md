# Manifests Kubernetes – Projeto CI/CD

Este repositório contém os arquivos `application.yaml`, `deployment.yaml` e `service.yaml` da aplicação FastAPI utilizados no projeto **Projeto CI/CD com GitHub Actions, Docker e ArgoCD – PB Compass UOL – DevSecOps.**

Ele é monitorado pelo ArgoCD, que realiza os deploys automáticos sempre que há atualizações aplicadas via GitOps.

**Lembrando:** Este repositório é atualizado automaticamente pela pipeline do GitHub Actions por meio de Pull Requests gerados de forma contínua.

---

🔗 **Repositório da aplicação FastAPI (com documentação Completa):**  
➡️ [projeto hello-app](https://github.com/GustavoHossein/hello-app)

---

## ⚙️ GitHub - Crie o repositório -> hello-manifests

- Name: `hello-manifests`
  - Visibility: `Public`
  - `Create repository`

---

## ⚙️ Criar arquivos dentro do manifests

``` bash
mkdir hello-manifests && cd hello-manifests

git init
```

1. Crie o arquivo **deployment.yaml**
``` bash
nano deployment.yaml
```
- Cole o código abaixo dentro do main.py:
``` bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-app
  namespace: default
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-app
  template:
    metadata:
      labels:
        app: hello-app
    spec:
      containers:
      - name: hello-app
        image: seu_usuario_docker/hello-app:latest  # 👈 SUBSTITUA AQUI!
        ports:
        - containerPort: 8000
        resources:
          requests:
            memory: "64Mi"
            cpu: "50m"
          limits:
            memory: "128Mi"
            cpu: "100m"
```

2. Crie o **service.yaml**
``` bash
nano service.yaml
```
- Cole o código abaixo dentro do requirements.txt:
``` bash
apiVersion: v1
kind: Service
metadata:
  name: hello-app-service
  namespace: default
spec:
  selector:
    app: hello-app
  ports:
  - port: 80
    targetPort: 8000
  type: ClusterIP
```

3. Crie o **application.yaml**
``` bash
nano application.yaml
```
- Cole o código abaixo dentro do Dockerfile:
``` bash
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: hello-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/seu_usuario_github/hello-manifests.git  # 👈 SUBSTITUA
    targetRevision: main
    path: .
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

- Fazer o push dos arquivos:
``` bash
git add .
git commit -m "Initial Kubernets manifests for hello-app"
git branch -M main
git remote add origin https://github.com/seu_usuario_github/hello-manifests.git
git push -u origin main
```

--- 

## Retorne para o projeto e siga os passos a passos a seguir -> ArgoCD
➡️ [Ir direto para “Instalar e Configurar ArgoCD”](https://github.com/gustavohossein/hello-app/blob/main/README.md#️-instalar-e-configurar-argocd)


