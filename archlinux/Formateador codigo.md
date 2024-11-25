***

Instalacion 

```bash
sudo pacman -S nodejs npm
sudo npm install -g prettier
```

verifica si los archivos en el directorio actual están formateados según las reglas de Prettier, sin modificar nada.

```bash
prettier --check .
```

formatear codigo

```bash
 prettier --write .
```

comando completo

```bash
prettier --check . --write
```

