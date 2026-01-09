crear las imagenes  y taguerlas

```bash
docker build -t romeo188/iglepay-frontend:latest .
```

```bash
docker build -t romeo188/iglepay-backend:latest .
```


en dockerhub crear un repo privado porque solo el personal solo deja crear uno lo llame iglepay

taguear las imagenes para subir en dockerhub

```bash
docker tag romeo188/iglepay-backend:latest romeo188/iglepay:backend-latest

docker tag romeo188/iglepay-frontend:latest romeo188/iglepay:frontend-latest

```

subir a dockerhub

```bash

docker push romeo188/iglepay:backend-latest

docker push romeo188/iglepay:frontend-latest

```

cada cambio del proyecto en la raiz donde esta flake

```bash
# Backend
docker build -t romeo188/iglepay-backend:latest ./backend

# Frontend
docker build -t romeo188/iglepay-frontend:latest ./frontend
```

taguear

```bash
docker tag romeo188/iglepay-backend:latest romeo188/iglepay:backend-latest
docker tag romeo188/iglepay-frontend:latest romeo188/iglepay:frontend-latest
```

push

```bash
docker push romeo188/iglepay:backend-latest
docker push romeo188/iglepay:frontend-latest
```