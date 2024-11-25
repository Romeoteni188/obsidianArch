***
## **Claves ssh**

Se instala el CI de github
GitHub está en la línea de comandos. Lleva solicitudes de incorporación de cambios, problemas y otros conceptos de GitHub a la terminal, junto a donde ya estás trabajando con `git`tu código.

Los siguientes comandos ejecutarlo en la terminal.

```bash
sudo apt install gh -y        
```

Para ver la version de CL.
```bash
 gh --version 
```

Iniciar seccion.
```bash
gh auth login 
```

Crea una solicitud de extracción en GitHub.
```bash
 gh pr create 
```


Se tiene que instarlar el agente ssh.

```bash
sudo apt install keychain

```


```bash
 ssh-keygen -t ed25519 -C "user945@gmail.com"
```


```bash
eval "$(keychain -q --eval --agents ssh ~/.ssh/id_ed25519242424)"
```

Ir al repositorio en settings para la configuracion ssh

![[Pasted image 20241111120055.png]]


Despues ir a la parte SSH Y GPG

![[Pasted image 20241111120220.png]]


Se agrega la clave ssh.

![[Pasted image 20241111123105.png]]

Se crea otra para la firma de la ssh.

![[Pasted image 20241111123149.png]]


















