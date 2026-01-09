***
Creacion de los archivos de confiugarcion

crear namespace
```bash
kubectl create namespace sat-xml
```

crear imagen



```bash
docker build -t romeo188/frontend-xml:v1.0.0 .

docker push romeo188/frontend-xml:v1.0.0
```


backend crear la imagen

```bash
docker build -t romeo188/backend-xml:v1.0.0 .
```

push
```bash
docker push romeo188/backend-xml:v1.0.0`
```

para entrar en la db en kuberntes
dentro dell pod 
```bash
  psql -U 10enconta -d 10encontaDB
```
