![[Pasted image 20241113153100.png]]

construir la imagen

```bash
docker build -t node-lts-prettier .
```

ejecutar la imagen

```bash
docker run -it --rm node-lts-prettier
```

ejecutar la version de pretter en contenedor

```bash
docker run -it --rm node-lts-prettier prettier -v
```

subir al dockerhub

iniciar seccion

```bash
docker login
```

etiquetarlo 

```bash
docker tag node-lts-prettier:latest romeo188/node-lts-prettier:latest
```

subir con la etiqueta

```bash
docker push romeo188/node-lts-prettier:latest
```

feat_node_prettier


node-prettier
se creo la rama

```bash
git checkout -b ci-node-prettier
```

despues de hacer los cambios , y mandarlos a la rama remota
crear un pull requestt.
dockerbuillx


