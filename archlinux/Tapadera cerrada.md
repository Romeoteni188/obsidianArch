## Tapadera cerrada

editar el archivo de configuracion

```bash
sudo nvim /etc/systemd/logind.conf
```

buscar esta linea y colocar hibernate, no quitar el #

```bash
 #HandleLidSwitch=hibernate
```

