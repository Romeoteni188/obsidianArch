como instalarlo en Arch linux 

```bash
helm create nombreplantilla
```

entrar al directorio de plantilla 
con el comanodo
```bash
helm install --dry-run debug .
```


o con este comando 
```bash
KUBECONFIG=/etc/rancher/k3s/k3s.yaml helm install --dry-run debug .
```

kubectl get pod --namesapce default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}"


darle permisos

```bash
sudo chown $USER:$USER /etc/rancher/k3s/k3s.yaml
chmod 600 /etc/rancher/k3s/k3s.yaml
```


## Instalacion de k3s 


```bash
curl -sfL https://get.k3s.io | sh -
```

despues verificar si se instalo correctamente 

```bash
k3s --version
```

ver el estado de k3s 

```bash
sudo systemctl status k3s
```

para acitvar 

```bash
sudo systemctl star k3s
```

vericar ubectl

```bash
kubectl get nodes
```

si da error, cambiar y darle permiso a usuario

```bash
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```


## Empaquetera y hacer un push a dockerhub

comando para empaquetar

plantilla fue la cree

```bash
❯ helm package Plantilla 
```

para hacer un push a docker hub, lo primero es loquearte en dockehub se obtiene un doken de seguridad dockehub darle permisos

```bash
 helm push  plantilla-0.1.0.tgz oci://registry-1.docker.io/romeo188
```

----
## comandos a usar 
helm list -A 
helm list
helm install nombre-realease ./michart/
helm upgrade nombre-realease /michart/
helm unistall nombre-realease 
helm test nombre-realease

## Enumerar, agregar, eliminar y actualizar repositorios

ljssskkkk




kubectl get pods                    # Ver todos los pods
kubectl get svc                     # Ver los servicios (services)
kubectl get deployments             # Ver los deployments
kubectl get all                     # Todo: pods, services, deployments, etc.
kubectl describe pod <nombre>      # Ver detalles completos de un pod
kubectl logs <nombre>              # Ver logs de un pod

















