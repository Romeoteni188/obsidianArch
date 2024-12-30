***
## Instalaciona de neovim y kitty
```bash
sudo pacman -S neovim
```

```bash
sudo pacman -S kitty
```

```bash
sudo pacman -S ripgrep
```

```bash
sudo pacman -S fzf
```
## instalacion de lazyvim

```bash
git clone https://github.com/LazyVim/starter ~/.config/nvim
```

```bash
rm -rf ~/.config/nvim/.git
```

```bash
nvim
```

buscarcar archivos  instalar grep preview
space + f+f 
file broser
space +s+f 
find files
space + space 
para seleccionar 
e+y 
para pegar 
p
para serrar ventanas 
space +w+d

## modo normal

### movimientos horizontales no se usan fechas |
h izquirda
l para la derecha
0 cero para ir al principio de linea de code 
$ dolar para ir al final de la linea 
w para saltar las palabras 
e para saltar 
g e es para retroceder 
 f para selecionar una letra  f para buscar mas similiares
 shift f para regresar 
 s letra o las dos palabras iniciales  , se seleciona a letra que quiere llegar 
 u para retrocedor de un cambio
 shift + k dar informacion de alguna funcion , componente
 g+ d para entrar en una funcion importada 
 control + o para regresar 
 g+c comentar
 g+d descomentar 
### movimientos verticales ->

contrl +d bajar media pantalla abajo 
shift+{} para saltar unidades logicas, pararrafos abajo y arriba
chift : 1 numero para ir a una linea especifica 
numero k o j para movernos a una linea 

### modo visual
shift + v modo visual y para saleccionar  
esc para salir modo visual

### modo bloque 
contrl + v


### modo insertar
i   atras de una letra 
a adelante de una letra
shift +v + c  con modo visual selecionar la palabra c para borrar y editar el archivo
o un salto de linea abajo 
shift + o salto arriba 
control + w borrar las palabras
control +  u borrar la linea completa
control + n autocompletado 
control+ space autocompletado/ sugerencias
control+n mover abajoj para una sugerencia
control + p arriba para selecionar una sugerencia
### NVIM Motions
//number + direcion + coman


dd borrar 
10 k dd  borrar dias lineas abajo o j siempre colocamos el numero 
10 k yy para copiar lineas abajo 

### registros repasar
space + ""

### macros  recordins 
Q para registrar 
Q  para guardar macro

en comand cmdline
%s/palabra/palabranueva/gc 

## grep
control :  vimgrep 
## ver lenguajes disponibles 
mason 
control +f 
MasonInstall ty.... para instalar 
space + c +a nos da opciones o sugerencias para los errores 
space + c + shift +a  no estar parado en el error 




















