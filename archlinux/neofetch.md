como se instala neofech en arch linux y lolcat para tener bonito color

```bash
sudo pacman -S neofetch
```

para instalar lolcat 

```bash
yay -S lolcat
```

con el color 

```bash
neofetch | lolcat
```

editar 

```bash
nano ~/.zshrc
```

añadir un alias 

```bash
alias neofetch='neofetch | lolcat'
```

guardar cambios

```bash
source ~/.zshrc
```

