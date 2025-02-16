***
Instalacion de bspwm arch

```bash
sudo pacman -S bspwm sxhkd
```

crear directorios

```bash
mkdir -p ~/.config/bspwm
mkdir -p ~/.config/sxhkd
```

generar archivos de configuracion

```bash
cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/
cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/
```

permisos 

```bash
chmod +x ~/.config/bspwm/bspwmrc
```

inicio

```bash
echo "exec bspwm" > ~/.xinitrc
```

