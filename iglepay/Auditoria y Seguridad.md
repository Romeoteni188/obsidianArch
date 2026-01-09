_____
Esta   area no ayuda a mitigar quienes el acceso quienes entran  al  servidor

- Ver usuarios conectados
```bash
who
```

```bash
w
```

- Historial de conexiones (incluyendo las que serraron)
```bash
last
```

- seciones activas en detalle
```bash
ps aux | grep sshd
```

- conexiones de red
```bash
ss -tnp | grep ssh
```
- esta es la mejor porque muestra quien tiene  establecimiento     con  ssh
```bash
sudo netstat -tnp | grep ssh
```

por  si no esta instaldo netstat
```bash
sudo ss -tnp | grep ssh
```

-  historial  de comando ejecutados
  ```bash
  cat ~/.bash_history
  ```

## blindado
---
ver si el archivo sshd_config tiene esta configuracion
```bash
sudo cat /etc/ssh/sshd_config
```

```bash
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers debian
```

- si no tiene agregarlo 

```bash
sudo nano /etc/ssh/sshd_config
```


### Cambiar de puerto 22 al 222
-----

```bassh
sudo vim /etc/ssh/sshd_config
```

descomentar y cambiar de puerto 

```bash
#Port 22
Port 2222
```

reiniciar

```bash
sudo systemctl restart ssh
```

reglas

```bash
sudo ufw status
```

```bash
sudo ufw allow 2222/tcp
sudo ufw deny 22/tcp
sudo ufw reload

```

Quien logro entrar 

```bash
last -a | head -20
```

Intentos fallidos

```bash
sudo lastb -a | head -20
```

