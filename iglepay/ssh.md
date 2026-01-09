# Configuración de SSH sin contraseña

## 1. Generar clave SSH (o usar la existente) en la máquina local

-  **Generar una clave nueva**

```bash
ssh-keygen -t ed25519 -C "tu_email@ejemplo.com"
```

-  **Si ya tienes una clave, revisa**

```bash
ls ~/.ssh/id_ed25519 ~/.ssh/id_ed25519.pub
```
## 2. copiar la clave publica al servidor 

-  **Opción manual: copiar y pegar el contenido de id_ed25519.pub**

```bash
cat ~/.ssh/id_ed25519.pub
```

- **Pegar el contenido en el archivo **

```bash
~/.ssh/authorized_keys del servidor
```

- **Opción automática con ssh-copy-id**

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@ip_del_servidor
```

## 3. Ajustar permisos en el servidor

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

## 4. Conectarse desde la maquina local

```bash
ssh -i ~/.ssh/id_ed25519 usuario@ip_del_servidor
```

- Opcional: agregar la clave al agente SSH para no ingresar passphrase cada vez:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## Notas de seguridad

- Nunca subir la clave privada (`id_ed25519`) al servidor.
    
- `authorized_keys` debe contener solo las claves públicas que quieras autorizar.
    
- Permisos estrictos en `~/.ssh` y `authorized_keys` son obligatorios para que funcione.

## Creacion alias en la teminal zshrc o bashrc

- En la terminal abrir el archivo ~/.zshrc con nano , vim , vscode y colocar al final el pedezo de codigo

```bash
# para vps
alias vps=' ssh -i ~/.ssh/id_ed25519 debian@148.113.203.34'
```

- Recargar la teminal
```bash
source ~/.zshrc
```

- Para luego poner solo vps automaticamente conectar a la servidor

# ssh Mariadb

Tunel ssh

```bash
 ssh -p 2222 -i ~/.ssh/id_ed25519 debian@148.113.203.34 \
  -L 3336:localhost:3306 \
  'KUBECONFIG=/home/debian/.kube/config kubectl port-forward pod/mariadb-86d7c7bbdd-r56pl 3306:3306'

```

serrar seccion
