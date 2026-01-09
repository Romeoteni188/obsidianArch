***
![[Pasted image 20251209111017.png]]

https://www.xme.digital/post/product-update-schema-based-multi-tenancy

docker compose

entrar al contendor 

```bash
docker exec -it postgres_db psql -U 10enconta -d 10encontaDB
```

para el backup
```bash
docker exec -it postgres_db pg_dump -U 10enconta 10encontaDB > ./dumps/backup.sql
```

para restaurar el dump
```bash
docker exec -i postgres_db psql -U 10enconta -d 10encontaDB < ./dumps/backup.sql
```


## Prisma

iniciar 
```bash
 pnpm prisma init
```

```bash
pnpm prisma generate
```

## docker 

construccion de la imagen

```bash
docker build -t romeo188/iglepay:factxmlfrontend-v1.0.0 .
```

subir imagen
```bash
docker push romeo188/iglepay:factxmlfrontend-v1.0.0
```



