***
Instalar `lsd` (una versión mejorada de `ls`)

```bash
https://github.com/lsd-rs/lsd/issues/118
```

```bash
sudo pacman -S lsd
```

**Configura un alias en tu `~/.zshrc`:**

```bash
alias ls='lsd'
```

Recarga la configuración:

```bash
source ~/.zshrc
```

### Explicación de los prefijos:

1. **`di=34`** - Directorios:
    
    - **`di`**: Se refiere a los **directorios**.
    - **`34`**: Es el código de color para **azul**.
2. **`ln=36`** - Enlaces simbólicos:
    
    - **`ln`**: Se refiere a los **enlaces simbólicos**.
    - **`36`**: Es el código de color para **cian**.
3. **`so=35`** - Sockets:
    
    - **`so`**: Se refiere a los **sockets** (archivos de tipo socket).
    - **`35`**: Es el código de color para **magenta**.
4. **`pi=33`** - Archivos con permisos especiales (FIFO):
    
    - **`pi`**: Se refiere a los **archivos FIFO** (named pipes o tuberías con nombre).
    - **`33`**: Es el código de color para **amarillo**.
5. **`ex=32`** - Archivos ejecutables:
    
    - **`ex`**: Se refiere a los **archivos ejecutables**.
    - **`32`**: Es el código de color para **verde**.
6. **`bd=93;40`** - Directorios (con texto en negrita y fondo oscuro):
    
    - **`bd`**: Se refiere a **directorios** con **negrita**.
    - **`93`**: Es el código de color para **negrita** (blanco brillante).
    - **`40`**: Es el código para **fondo oscuro** (negro).
7. **`cd=93;40`** - Directorios de cambio (con texto en negrita y fondo oscuro):
    
    - **`cd`**: Se refiere a **directorios de cambio** (posiblemente directorios de trabajo).
    - **`93`**: Es el código de color para **blanco brillante**.
    - **`40`**: Es el código para **fondo oscuro**.
8. **`su=41`** - Archivos de usuario (archivos con permisos suid):
    
    - **`su`**: Se refiere a los **archivos con permisos suid** (set-user-ID).
    - **`41`**: Es el código de color para **fondo rojo**.
9. **`sg=46`** - Archivos de grupo (archivos con permisos sgid):
    
    - **`sg`**: Se refiere a los **archivos con permisos sgid** (set-group-ID).
    - **`46`**: Es el código de color para **fondo verde**.
10. **`tw=42`** - Archivos con permisos de escritura:
    
    - **`tw`**: Se refiere a los **archivos con permisos de escritura** (tuberías).
    - **`42`**: Es el código de color para **fondo verde claro**.
11. **`ow=43`** - Archivos con permisos de ejecución:
    
    - **`ow`**: Se refiere a los **archivos con permisos de ejecución**.
    - **`43`**: Es el código de color para **fondo amarillo**.




Para cambiar el color de las carpetas en tu terminal con Zsh, necesitas ajustar la variable `LS_COLORS` que ya tienes configurada en tu archivo `~/.zshrc`. En tu configuración actual, las carpetas están configuradas con el color azul (`di=34`).

**Localiza el código de color para las carpetas** en la configuración de `LS_COLORS`. Actualmente está configurado como `di=34`, donde:

- `di` representa las carpetas.
- `34` es el código para el color azul.
**Cambia el valor de `di`** para establecer el color que prefieras. Los códigos de color más comunes son:

- `30` - negro
- `31` - rojo
- `32` - verde
- `33` - amarillo
- `34` - azul
- `35` - magenta
- `36` - cian
- `37` - blanco


los mejores son el 36,35,32,31

Cambiar el comportamiento del ls con ls -l


```bash
# Configuración de colores para 'ls'
export LS_COLORS='di=36:ln=36:so=35:pi=33:ex=32:bd=93;40:cd=93;40:su=41:sg=46:tw=42:ow=43'
#alias ls='ls --color=auto'
#herramienta lsd para los colores
#alias ls='lsd'
# Herramienta lsd para los colores y el comportamiento ls -l
alias ls='lsd -l --color=auto'
```







***
## fuentes 

descargar la que mas te guste 

```bash
https://www.nerdfonts.com/
```

crear los directorios si no existe 

```bash
mkdir -p ~/.local/share/fonts
```

copiar en el directorio

```bash
cp ~/Downloads/HackFont/*.ttf ~/.local/share/fonts/
cp ~/Downloads/FiraCodeFont/*.ttf ~/.local/share/fonts/
```

recargar las fuentes 

```bash
fc-cache -fv
```


## instalar bat

```bash
sudo pacman -S bat
```

## Temas

```bash
sudo pacman -S adapta-gtk-theme
sudo pacman -S arc-gtk-theme
```

## iconos

```bash
sudo pacman -S papirus-icon-theme
sudo pacman -S numix-icon-theme
```

## autocompletado 

para instalarlo

```bash
sudo pacman -S zsh
```

ver la version

```bash
zsh --version
```
### Instalar `zsh-syntax-highlighting`
Este plugin resalta la sintaxis en el terminal para que los comandos sean más legibles.

```bash
sudo pacman -S zsh-syntax-highlighting
```

para activarlo

```bash
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

guardar cambios

```bash
source ~/.zshrc
```


### Instalar `zsh-autosuggestions`

Este plugin sugiere comandos en el terminal basándose en tu historial y en lo que escribes.

```bash
sudo pacman -S zsh-autosuggestions
```

Actívalo en tu archivo `~/.zshrc` añadiendo la línea:

```bash
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
```

recargar 

```bash
source ~/.zshrc

```

### zsh-completions

**zsh-completions**: Mejora el autocompletado para herramientas adicionales.

```bash
sudo pacman -S zsh-completions
```

activarlo

```bash
source /usr/share/zsh/plugins/zsh-completions/zsh-completions.zsh
```

**fzf**: Permite búsquedas rápidas en tu historial y archivos.

```bash
sudo pacman -S fzf
```

Luego, activa la integración en tu `~/.zshrc`:

```bash
source /usr/share/fzf/key-bindings.zsh
source /usr/share/fzf/completion.zsh
```

recargar 

```bash
source ~/.zshrc
```