
Manera de instalar 

```bash
paru -S popeye-bin
```

comados para usar 
popeye -A -s cm

## Generar Reporte

En formato yaml

```bash
popeye -0 yaml > reporet.yml
```

en formato json

```bash
popeye -0 json > reporet.json
```

```bash
¡Claro! Aquí tienes la explicación **traducida al español** y adaptada para mayor claridad:

---

```bash
# Mostrar la versión de Popeye y la ubicación de los logs
popeye version

# Ejecutar Popeye en el clúster usando tu entorno kubeconfig actual.
# NOTA: Esto ejecuta Popeye en el namespace actual si está definido,
# o usa el namespace "default", igual que kubectl.
popeye

# Ejecutar Popeye en el namespace llamado `fred`
popeye -n fred

# Ejecutar Popeye en **todos los namespaces**
popeye -A

# Ejecutar Popeye usando un archivo de configuración de spinach (¡por supuesto!)
# también conocido como spinach.yaml
popeye -f spinach.yaml

# Ejecutar Popeye en el clúster usando un contexto específico del kubeconfig.
popeye --context olive

# Ejecutar Popeye con linters específicos (solo para pod y svc en este caso)
# y mostrar los logs en la consola.
popeye -n ns1 -s pod,svc --logs none

# Ejecutar Popeye para un namespace específico,
# guardando el log en un archivo y activando logs de depuración (nivel 4)
popeye -n ns1 --logs /tmp/fred.log -v4

# ¿Necesitas ayuda?
popeye help
```

---

**Algunas aclaraciones:**

- `-n`: namespace.
    
- `-A`: todos los namespaces.
    
- `-f`: archivo de configuración personalizado.
    
- `--context`: para especificar un contexto del kubeconfig.
    
- `-s`: seleccionar qué tipos de recursos auditar (por ejemplo, `pod,svc`).
    
- `--logs`: especifica dónde escribir los logs (`none` para ninguno, o una ruta).
    
- `-v`: nivel de detalle del log (`-v4` es muy detallado).
    

¿Te gustaría traducir también algún archivo de configuración o algún ejemplo práctico? 🚀


