
como instalarlo 
```bash
sudo apt install fail2ban -y
```

copiar la base 

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

pude elimanar el contenido del archivo jail se para personalizar y no tener problemas con las configuraciones actules

editar el archivo de configuracion
```bash
 sudo vim /etc/fail2ban/jail.local
```
10 minutos
5 intentos

configuracion
```bash

[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5
banaction = iptables-multiport
# logtarget = /var/log/fail2ban.log   # opcional, deja que use el valor por defecto

[sshd]
enabled  = true
port     = ssh
logpath  = /var/log/auth.log
backend  = systemd

```

habilitar los servicios

```bash
sudo systemctl enable fail2ban
sudo systemctl restart fail2ban
```


ver las ips bloquedas

```bash
sudo iptables -L f2b-sshd -n -v
```

```bash
sudo fail2ban-client status sshd
```

ver en tiempo real

```bash
sudo journalctl -u ssh -f
```

para ver la hora que se conecto y como lo hizo sustituir la ip primero el comando para quien esta conectado:
```bash
sudo netstat -tnp | grep ssh
```

como entro

```bash
 sudo journalctl -u ssh -o short-iso | grep "65.20.251.53"
```

## Configuracion blindada ssh,nginx,mariadb

copiar y pegar
```bash

debian@vps-88bc001d:/etc/fail2ban$ cat jail.local
[DEFAULT]
# Configuración general
bantime  = 10h
findtime = 10m
maxretry = 5
banaction = iptables-multiport
# logtarget = /var/log/fail2ban.log  # opcional, deja que use el valor por defecto

######################################
# SSH
######################################
[sshd]
enabled  = true
port     = ssh
logpath  = /var/log/auth.log
backend  = systemd

######################################
# Nginx: bloquea intentos de login fallidos y bots
######################################
[nginx-http-auth]
enabled  = true
port     = http,https
filter   = nginx-http-auth
logpath  = /var/log/nginx/error.log
maxretry = 3
bantime  = 1h

[nginx-botsearch]
enabled  = true
port     = http,https
filter   = nginx-botsearch
logpath  = /var/log/nginx/access.log
maxretry = 10
bantime  = 2h

######################################
# MariaDB
######################################
[mysqld-auth]
enabled  = true
port     = 3306
filter   = mysql-auth
logpath  = /var/log/mysql/error.log
maxretry = 3
bantime  = 1h

```

para bloquear 1 mes

```bash
[DEFAULT]
# tiempos pueden darse en segundos o con sufijo (m,h,d)
bantime  = 720h        # 720 horas = 30 días
findtime = 10m
maxretry = 3
banaction = iptables-multiport
# logtarget = /var/log/fail2ban.log

[sshd]
enabled  = true
port     = 2222
logpath  = /var/log/auth.log
backend  = systemd
# valores específicos para sshd (sobrescriben DEFAULT si se usan)
#maxretry = 3
#bantime  = 720h
#findtime = 10m
```

3.58
## k3s
bloquear ip

```bash
 sudo iptables -A INPUT -p tcp --dport 6443 ! -s 127.0.0.1 -j DROP
sudo iptables -A INPUT -p tcp --dport 10250 ! -s 127.0.0.1 -j DROP
sudo iptables -A INPUT -p udp --dport 8472 -j DROP
```

```bash
sudo ss -tunap
```

quitar las reglas

```bash
sudo iptables -D INPUT -p tcp --dport 6443 ! -s 127.0.0.1 -j DROP
sudo iptables -D INPUT -p tcp --dport 10250 ! -s 127.0.0.1 -j DROP
sudo iptables -D INPUT -p udp --dport 8472 -j DROP
```

