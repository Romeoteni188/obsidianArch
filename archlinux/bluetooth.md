***
## *Instalacion de Bluetooth*

```bash
 sudo pacman -S usbutils
```

```bash
lsusb | grep -i bluetooth
```

```bash
sudo pacman -Syu bluez bluez-utils
```

habilitar el servicio

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

ver el estado del servicio 

```bash
systemctl status bluetooth
```

```bash
sudo pacman -S linux-firmware
```

verificar el adpatador

```bash
bluetoothctl
```

interfaz grafica 

```bash
sudo pacman -S blueman
```








