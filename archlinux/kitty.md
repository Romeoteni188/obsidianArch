****
## kitty

instalar 
```bash

```

configurar

```bash

```


***
# Ranger

visualizar imagenes 

copiar el archivo de configuracion
```bash
 ranger --copy-config=all
```

configurar el archivo rc.conf
en /.config/ranger/
```bash
#imagen previewer
set preview_method kitty
set preview_images true
```
para cambiar el color letras y iconos 

```bash
sudo pacman -S eza
```

en configuracion de .zshrc

```bash
alias ls="eza -l --icons --color=auto"
```

recargar la shell
```bash
 source .zshrc
```

para cambiar de tema 
`kitten themes`

esocojer el tema
enter
despues M para modificar



