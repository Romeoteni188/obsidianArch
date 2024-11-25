***

Crear una imagen

```bash
docker build -t nombre_de_imagen .
```

ejemplo:

docker build -t node_prettier .

Ejecutar el contenedor 

```bash
docker run -it node_prettier
```

etiquetarlo a tag

docker tag <image_id> <username>/<repository_name>:<tag>

```bash
docker tag node_prettier:latest romeo188/node_prettier:latest
```
push a dockerhub

```bash
docker push romeo188/node_prettier:latest
```

![[Pasted image 20241113164416.png]]

traerme la imagen del docker pull

```bash
docker pull romeo188/node_prettier
```
para levantar el contenedor

```bash
docker run -d romeo188/node_prettier:latest
```

docker usar una bash

```bash
docker run -it romeo188/node_prettier:latest bash
```
