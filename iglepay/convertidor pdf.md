
```bash
docker build -t libreoffice-server:latest .
```

ejecutar la imagen

```bash
docker run -d \
  --name libreoffice-server \
  -p 2000:2000 \
  libreoffice-server:latest

```

con su version
```bash
docker run -d \
  --name libreoffice-server \
  -p 2000:2000 \
  romeo188/pdf-converter:v1.0.1
```

bash

```bash
docker exec -it libreoffice-server3 sh
```

copiar .env
```bash
docker cp .env libreoffice-server3:/var/www/html/.env
```

```bash

 docker run -d \
  --name libreoffice-server3 \
  -p 2000:2000 \
  --env TOKEN_SECRET=bWljb2Rlc2VjcmV0bwo= \
  libreoffice-server3

```

push

docker build -t romeo188/pdf-converter:v1.0.4 .


```bash
docker push romeo188/pdf-converter:v1.0.1
```

## con varibles en dockerfile
```bash
docker build -t romeo188/pdf-converter:v1.0.4 \
  --build-arg PORT=2000 \
  --build-arg UPLOAD_DIR=uploads \
  --build-arg OUTPUT_DIR=converted \
  --build-arg CONVERT_TOKEN=bWljb2Rlc2VjcmV0bwo= .

```

