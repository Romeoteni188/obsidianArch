---
cssclasses:
---
Verificar si tenemos cronie  con el comando

```bash
pacman -Q cronie
```

ver el estado el servicio 

```bash
systemctl status cronie
```

habilitar los servicios 

```bash
 systemctl enable cronie
 systemctl start cronie
```

ver el estado 
```bash
systemctl status cronie
```

exportar editor de text

```bash
export VISUAL=nvim
export EDITOR=nvim
crontab -e

```

```bash
crontab -e
```


```bash
crontab -l

```

para ver los logs 

```bash
journalctl -u cronie --since "1 hour ago"
```


