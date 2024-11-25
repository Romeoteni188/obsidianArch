***
Estado de la base de datos

```bash
sudo systemctl status mariadb
```

Activar la base de datos

```bash
sudo systemctl start mariadb
```

para aceder a la base de datos

```bash
sudo mariadb
```

crear la base de datos 

```bash
CREATE DATABASE dbtienda;
```

crear un usuario

```bash
CREATE USER 'romeo188'@'localhost' IDENTIFIED BY 'rome188+';
```

asignar permisos el usuario a la db

```bash
GRANT ALL PRIVILEGES ON dbtienda.* TO 'romeo188'@'localhost';
```
aplicar cambios de permisos

```bash
FLUSH PRIVILEGES;
```

salir 

```bash
exit
```


![[Pasted image 20241112210325.png]]


