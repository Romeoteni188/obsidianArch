***

**Actualizar el sistema**: Antes de instalar cualquier paquete, es recomendable actualizar el sistema:

```bash
sudo pacman -Syu
```


**Instalar Docker**: Ejecuta el siguiente comando para instalar Docker:

```bash
sudo pacman -S docker
```

**Iniciar y habilitar el servicio Docker**: Una vez que Docker esté instalado, necesitarás iniciar y habilitar el servicio para que se inicie automáticamente al arrancar el sistema:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

**Verificar la instalación**: Para comprobar que Docker está funcionando correctamente, puedes ejecutar:

```bash
sudo docker --version
```

**(Opcional) Añadir tu usuario al grupo Docker**: Si no quieres tener que usar `sudo` cada vez que ejecutas comandos de Docker, puedes agregar tu usuario al grupo `docker`:

```bash
sudo usermod -aG docker $USER
```


## **Instalar Zsh en Arch Linux**

**Instalar Zsh**: Ejecuta el siguiente comando:

```bash
sudo pacman -S zsh

```

**Cambiar el shell predeterminado a Zsh**: Una vez instalado Zsh, puedes cambiar tu shell predeterminado ejecutando:

```bash
chsh -s /usr/bin/zsh
```

## **Opcional) Instalar Oh My Zsh**: Si deseas una configuración más completa de Zsh, puedes instalar "Oh My Zsh", que es un framework para gestionar la configuración de Zsh.

Para instalarlo, puedes ejecutar el siguiente comando:

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## **Instalar Obsidian en Arch Linux**

```bash
sudo pacman -S git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

**Instalar Obsidian desde AUR**: Ahora que tienes un AUR helper como `yay`, puedes instalar Obsidian:

```bash
yay -S obsidian
```

**Iniciar Obsidian**: Una vez que la instalación esté completada, puedes abrir Obsidian ejecutando:

```bash
obsidian
```

## vscode 

**Instalar Visual Studio Code (VSCode)** desde AUR

```bash
yay -S visual-studio-code-bin
```

Si prefieres la versión de código abierto de **VSCode**, conocida como **VSCodium**, puedes instalarla con:

```bash
yay -S vscodium
```

## Docker-Compose 

```bash
sudo pacman -S docker-compose
```

para ver la version

```bash
docker-compose --version
```

## Instalar **docker-buildx**:

```bash

```