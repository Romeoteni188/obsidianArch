instalar cargo 

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

enlace de la instalacion 

https://github.com/martin-olivier/airgorah/wiki/Installation

comando para instalar 

```bash
sudo pacman -Syu && sudo pacman -U ./airgorah_`arch`.pkg.tar.zst
```



para ejecutar airgorah con sway

```bash
sudo --preserve-env=WAYLAND_DISPLAY,XDG_RUNTIME_DIR /usr/bin/airgorah

```
