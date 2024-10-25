# Outils de développement et conteneurisation avancée

Récup les slides de cours sur myGES.

## Rapels Git et Docker

### Git

VCS : Version Controller System

### Docker

Dans `Dockerfile`, configuration pour docker.

```docker
FROM node:20-alpine

WORKDIR /app

COPY package*.json .

RUN npm install

COPY main.js .

ENV PORT=80

EXPOSE 80

# Process ID 1 script en cours
# Process ID 2 node main.js

# shell form
#CMD node main.js

# exec form
CMD [ "node", "run", "dev ]
```

Dans `compose.yaml`, permet de ne pas utiliser une grand commande en CLI.

```yaml
name : echo.js

services:
    app:
        build: .
        image: enzo/echojs:3.0.0
        ports:
            - "3000:80"
        volumes:
            - ./main.js:app/main.js

    db:
        image: postgres:17:alpine
        environnement:
            POSTGRES_USER: toto
            POSTGRES_PASSWORD: tot
            POSTGRES_DB: app
        volumes:
            - db_data:/var/lib/postgresql/data
    
    volumes:
        db_date:
            driver: local
```

## Introduction à Kubernetes

Créer un cluster.

```bash
minikube start
minikube dashboard
kubectl run web-server --image=nginx:1.27-bookworm
expose pod web-server --port=80
kubectl get pods
kubectl get services
kubectl port-forward service/web-server 8000:80
kubectl applu -f web-server.yaml
```

Dans `web-server.yaml`, permet de créer un pod sans faire une grand commande CLI. C'est un déclaration de ressources.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: web-server
    label:
        app: web-server
spec:
    containers:
        - name: nginx
          image: nginx:1.27-bookworm
          ports:
            - containerPort: 80
---

apiVersion: v1
kind: Service
metadata:
    name: web-server
spec:
    selector:
        app: web-server
    ports:
        - port: 80
          targetPort: 80
```
