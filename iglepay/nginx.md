***
## Instalacion de nginx

```bash
sudo apt install nginx -y
```

habilitar los servicios

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

- Instalar node y pnpm desde la pagina oficial esta la forma de como instalar

```bash
https://nodejs.org/en/download/
```
en la terminal ejecutar el listado comando para pnpm:

-  pnpm setup
- echo $PNPM_HOME
- export PATH="$PNPM_HOME:$PATH"
- instalar PM2 para manter encendido la app
 ```bash
sudo pnpm i -g pm2  
  ```
- ver la version 
```bash
pm2 -v
```

- instalar tambien el cli de nestjs ya sea gloval o solo proyecto

```bash
pnpm add -D @nestjs/cli
```

- instalacion global
```bash
pnpm add -g @nestjs/cli
```


## configuracion proxy inverso para el servidor 
- Editar el archivo iglepay.conf

```bash
 sudo vim /etc/nginx/sites-available/iglepay.conf
```

- agregar la configuracion y agregar el puerto donde corre nestjs en este caso 4000

```bash
server {
    listen 80;
    server_name api.iglepay.com;

    location / {
        proxy_pass http://127.0.0.1:4000;  
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

- Habilitar  y Recargar Nginx

```bash
sudo ln -s /etc/nginx/sites-available/iglepay.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## HTTPS con Certbot (ssl gratis)

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d api.iglepay.com
```

en namecheap se creo otro A Recordd un subdominio para el backend
el subdominio tiene que apuntar al backend.

| Type     | Host | Value          | TTL       |     |
| :------- | :--- | :------------- | :-------- | --- |
| A Record | @    | 216.198.79.1   | automatic |     |
| A Record | api  | 148.113.203.34 | automatic |     |

## Ruta var/www
crear un nuevo directorio para almacenar el repo clonado el del backend
- **/var/www/iglepay-backend/Calvario/backend**
- pnpm build
- pnpm add -D @types/express
- 


- Levantar el backend con pm2 

```bash
pm2 start dist/src/main.js --name backend
pm2 save
pm2 startup systemd
```

para que sea automatico
- sudo env PATH=$PATH:/home/debian/.nvm/versions/node/v22.19.0/bin pm2 startup systemd -u debian --hp /home/debian
- para confirmar 

```bash
pm2 list
pm2 status
```

firewall

```bash
sudo ufw allow 4000
sudo ufw status
sudo ufw enable
```



```bash
# Backend: api.iglepay.com
server {
    listen 80;
    server_name api.iglepay.com;

    location / {
        proxy_pass http://127.0.0.1:4000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

# Frontend: iglepay.com
server {
    listen 80;
    server_name iglepay.com www.iglepay.com;

    location / {
        proxy_pass http://127.0.0.1:3000;  # Frontend SSR
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    location /api {
        proxy_pass http://127.0.0.1:4000;  # Backend
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```