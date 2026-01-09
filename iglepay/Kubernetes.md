____
## Instalar k3s

```bash
curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s
```

verificar nodo

```bash
sudo k3s kubectl get nodes
```

### helm chart

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

helm version
```


listar todos los pods

```bash
kubectl get pods -A
```

listar solo los pods de un namespace

```bash
kubectl get pods
```


es con sudo

```bash
sudo k3s kubectl get pods -A
```

## clonar 

se clona del repositorio las plantillas, pero primero crear las claves ssh de la vps, y despues con crea el directorio
helm-charts en donde en var

despues darle permisos

```bash
 sudo chown -R debian:debian /var/helm-charts/
 sudo chmod -R 755 /var/helm-charts/
```


despues crear secret.yml en la vsp donde estan los chars copiar las varibles que estan en local



Depues aplicar los cambios con el comando

```bash
kubectl apply -f secrets.yaml
```

### permisos ruta 

si no tiene permiso en directorio crear un directorio
```bash
/etc/rancher/k3s/k3s.yaml
```

crear directorio

```bash
mkdir -p $HOME/.kube
```

copiar la configuracion de k3s a nuestro home 

```bash
sudo cp /etc/rancher/k3s/k3s.yaml $HOME/.kube/config
```

cambiar de propietario

```bash
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
exportar 

```bash
export KUBECONFIG=$HOME/.kube/config
```

aprovar si funciona

```bash
kubectl get nodes
```

dentro de plantilla helchar donde estas los archivo Chart.yaml  secrets.yaml  templates  values.yaml alli se ejecuta el siguiente comando para crear

```bash
helm upgrade --install iglepay . --namespace iglepay --create-namespace
```

para ver si estan corriendo 

```bash
kubectl get pods -n iglepay
kubectl get svc -n iglepay
kubectl get statefulsets -n iglepay
```

ver logs

```bash
kubectl describe pod backend-f8cccd7dc-bwh48 -n iglepay
```

## Errrores

ver  si secret exite en el namespace de iglepay

```bash
kubectl get secrets -n iglepay
```

asi se ve cuando no esta secrets
```bash

NAME                            TYPE                             DATA   AGE
regcred                         kubernetes.io/dockerconfigjson   1      25m
sh.helm.release.v1.iglepay.v1   helm.sh/release.v1               1      74m
sh.helm.release.v1.iglepay.v2   helm.sh/release.v1               1      60m
sh.helm.release.v1.iglepay.v3   helm.sh/release.v1               1      54m
sh.helm.release.v1.iglepay.v4   helm.sh/release.v1               1      39m
sh.helm.release.v1.iglepay.v5   helm.sh/release.v1               1      22m
sh.helm.release.v1.iglepay.v6   helm.sh/release.v1               1      3m4s
```

asi se agrega
```bash
kubectl apply -f secrets.yaml -n iglepay
```

asi ve cuando tiene

```bash

NAME                            TYPE                             DATA   AGE
iglepay-secrets                 Opaque                           5      7s
regcred                         kubernetes.io/dockerconfigjson   1      26m
sh.helm.release.v1.iglepay.v1   helm.sh/release.v1               1      75m
sh.helm.release.v1.iglepay.v2   helm.sh/release.v1               1      61m
sh.helm.release.v1.iglepay.v3   helm.sh/release.v1               1      55m
sh.helm.release.v1.iglepay.v4   helm.sh/release.v1               1      40m
sh.helm.release.v1.iglepay.v5   helm.sh/release.v1               1      23m
sh.helm.release.v1.iglepay.v6   helm.sh/release.v1               1      3m56s
```

## Reiniciar los deploymets

```bash
kubectl rollout restart deployment backend -n iglepay
kubectl rollout restart deployment frontend -n iglepay
```



```bash
sudo helm upgrade iglepay . -n iglepay
sudo kubectl rollout restart deployment nginx -n iglepay
```

```bash
sudo kubectl logs -n iglepay -l app=nginx -c certbot --follow
```

# cerbot en la vps

```bash
sudo apt update
sudo apt install certbot -
```

```bash
sudo apt update
sudo apt install nginx -y
```

2. Crea el webroot que Certbot usará:

```bash
sudo mkdir -p /var/www/iglepay
sudo chown -R $USER:$USER /var/www/iglepay
```


```bash
sudo systemctl start nginx
```

```bash
sudo certbot certonly --webroot -w /var/www/iglepay -d iglepay.com --email romeoteni093@gmail.com --agree-tos --no-eff-email --non-interactive
```

```bash
sudo vim /etc/nginx/sites-available/iglepay
```

```bash
server {
    listen 80;
    server_name iglepay.com www.iglepay.com;

    root /var/www/iglepay;
    index index.html;

    location /.well-known/acme-challenge/ {
        root /var/www/iglepay;
        allow all;
    }

    location / {
        try_files $uri $uri/ =404;
    }
}

```



eliminar el fabrica

```bash
sudo rm /etc/nginx/sites-enabled/default
sudo systemctl reload nginx
```

---------


# con puro .yaml

descargar certificados

```bash
sudo kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.15.3/cert-manager.yaml
```


Todo está en orden:

✅ `cert-manager` corriendo  
✅ `ClusterIssuer` creado  
✅ `traefik` presente como clase de Ingress

Ahora viene el paso clave → **configurar tu `ingress.yaml`** para usar:

- tu dominio `iglepay.com`
    
- Traefik como controlador
    
- y certificados automáticos de Let's Encrypt




backend
para las variables
```bash
sudo kubectl create secret generic backend-secrets \
  --from-literal=DATABASE_URL="mysql://iglepay:iglepay.com%23@mariadb:3306/IglePayDB" \
  --from-literal=JWT_SECRET="f34fb6f1361e3db95500929b1b964a3d08ea302144cf6fe0d6b6217e6c6771e8"

```


copiar el backup a pod

```bash
kubectl cp IglePayDB_2025-10-05_10-00.sql mariadb-86d7c7bbdd-r56pl:/tmp/backup.sql
kubectl exec -it mariadb-86d7c7bbdd-r56pl -- bash
mariadb -u root -p IglePayDB < /tmp/backup.sql
```

para ver el tamani de   disco mariadb
```bash
kubectl get pvc
```

para ver si expandible mi  mariad 

```bash
kubectl get storageclass
```

##  Namespace

```bash
kubectl get pods --all-namespaces
```

## VER pods

```bash
kubectl get pods
```

```bash
kubectl get pods -o wide
```

## Ver Certificados

```bash
kubectl get certificates
```

Ver el Secret (donde está guardado el certificado real)

```bash
kubectl get secret iglepay-tls
```

ver el contenido codificado

```bash
kubectl describe secret iglepay-tls
```


el archivo ingress-redirect 
se encarga de las redireccion http a https

y para aplicar los cambios 

```bash
kubectl apply -f ingress-redirect.yaml
```

## Renovar certificado

```bash
kubectl cert-manager renew iglepay-tls -n default
```

entrar en maria db en pod
k9s
```bash
mariadb -u iglepay -p
```
