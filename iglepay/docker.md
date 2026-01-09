## frontend

construir la  imagen
```bash
docker build -t frontend:latest ./frontend
docker run -p 3000:3000 frontend:latest
```

para instalar curl temporal

```bash
apk add --no-cache curl
```

## backend
```bash
docker build -t backend:latest ./backend 
docker run -p 4000:4000 backend:latest
```

```bash
docker run --env-file ./backend/.env -p 4000:4000 backend:latest
```



## mariadb

Entrar en shell

```bash
docker exec -it mariadb mariadb -u root -p

```

dar privilegios a iglepay

```bash
GRANT ALL PRIVILEGES ON *.* TO 'iglepay'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```

ejecutar la migracion
```bash
 docker compose run --rm backend pnpm dlx prisma migrate dev --name init

```

para entrar nuevamente a contendor 

```bash
docker exec -it mariadb mariadb -u iglepay -piglepay.com#
```

- Crear un user

```bash
INSERT INTO tb_users (id_tenant, nombre, email, contrasena, id_rol)
VALUES (1, 'Romeo Teni', 'romeoteni093@gmail.com', '$2b$12$1jqpCmhP0/xZ56GJnj3GgudFcMkKQhC6mGMOcGkomf5vTAaRUN2Uu', 1);
```

- crear permisos

 ```bash
 INSERT INTO tb_permission (nombre, descripcion) VALUES
('escritorio', 'Acceso al escritorio'),
('ver_miembros', 'Ver miembros'),
('ver_actividades', 'Ver actividades'),
('ver_calendario', 'Ver calendario'),
('ver_colaborador', 'Ver colaborador'),
('ver_grupos', 'Ver grupos'),
('ver_finanzas', 'Ver finanzas'),
('ver_familias', 'Ver familias'),
('ver_horario', 'Ver horario'),
('Ver_asistencia', 'Ver asistencia'),
('ver_usuario', 'Ver usuario'),
('ver_tenant', 'Ver datos de la iglesia'),
('ver_asistenciafamilia','Accceso a asistencia de familias'),
('ver_rolpermiso', 'Ver roles y permisos');
 ```
insertar los permisos al rol Admin

```bash
INSERT INTO tb_role_permissions (role_id, permission_id) VALUES
(1, 1),
(1, 2),
(1, 3),
(1, 4),
(1, 5),
(1, 6),
(1, 7),
(1, 8),
(1, 9),
(1, 10),
(1, 11),
(1, 12),
(1, 13);
```

| MariaDB  | StatefulSet + Service | StatefulSet para mantener los datos persistentes, y un PVC para almacenamiento.      |
|----------|-----------------------|--------------------------------------------------------------------------------------|
| Backend  | Deployment + Service  | Un Deployment con replicas, conectado a la base de datos.                            |
| Frontend | Deployment + Service  | Un Deployment que sirve la SPA.                                                      |
| Nginx    | Deployment + Service  | Nginx actúa de reverse proxy hacia backend y sirve el frontend si quieres.           |
| Certbot  | Job o sidecar         | Para generar/renovar certificados HTTPS. Alternativa: usar ingress con cert-manager. |
| Ingress  | Ingress Controller    | Ideal para exponer frontend y backend en un dominio con HTTPS automáticamente.       |

cambiar el tag
```bash
docker build -t romeo188/iglepay-backend:v1.0.1 .
docker push romeo188/iglepay-backend:v1.0.1
```

```bash
docker build -t romeo188/iglepay-frontend:v1.0.1 .
docker push romeo188/iglepay-frontend:v1.0.1
```


en kubernentes trear los cambios es 

```bash
kubectl rollout restart deployment backend
kubectl rollout restart deployment frontend  # si también subiste frontend
```

version 2  frontend
```bash
docker build -t romeo188/iglepay-frontend:v1.0.2 ./frontend
docker push romeo188/iglepay-frontend:v1.0.2
```

con variables
```bash
docker build \
  -t romeo188/iglepay-frontend:v1.0.10 \
  --build-arg NEXT_PUBLIC_API_URL=https://api.iglepay.com \
  --build-arg NEXT_PUBLIC_GA_TRACKING_ID=G-QX8LHCRKQD \
  --build-arg NEXT_PUBLIC_CONVERT_API=https://converter.iglepay.com \
  --build-arg NEXT_PUBLIC_CONVERT_TOKEN=bWljb2Rlc2VjcmV0bwo= \
  ./frontend
```

configMaps

```bash
kubectl get configmaps
```

describir los configmap
```bash
kubectl describe configmap frontend-config
```

