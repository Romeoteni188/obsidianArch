***
La especificación de Commits Convencionales es una convención ligera sobre los mensajes de commits. Proporciona un conjunto sencillo de reglas para crear un historial de commits explícito; lo que hace más fácil escribir herramientas automatizadas encima del historial. Esta convención encaja con [SemVer](http://semver.org/lang/es/), al describir en los mensajes de los commits las funcionalidades, arreglos, y cambios de ruptura hechos.

## Formato de las confirmaciones 

```bash
<tipo> (<ambito opcional>):<descripcion>
linea vacia
[cuerpo opcional]
linea vacia
[pie de pagina opcional]

```

### Tipos


- Cambios relevantes en la API
    - `feat`: Un commit de _tipo_ `feat` introduce una nueva funcionalidad en la base del código (se correlaciona con [`MINOR`](http://semver.org/#summary) en el Versionado Semántico).
    - `fix`: Un commit de _tipo_ `fix` corrige un error en la base del código (se correlaciona con [`PATCH`](http://semver.org/#summary) en el Versionado Semántico).
    -  `BREAKING CHANGE`: Un commit que contiene la nota al pie `BREAKING CHANGE:`, o que agrega un `!` después del tipo/ámbito, introduce un cambio de ruptura de API (se correlaciona con [`MAJOR`](http://semver.org/#summary) en el Versionado Semántico). Un BREAKING CHANGE puede ser parte de commits de cualquier _tipo_.
- `refactor`: Confirmaciones que reescriben o reestructuran su código, pero no cambian ningún comportamiento de la API.
    - `perf`: Los commits son `refactor`commits especiales que mejoran el rendimiento.
- `style`: Confirmaciones que no afectan el significado (espacios en blanco, formato, puntos y coma faltantes, etc.)
- `test`: Confirmaciones que agregan pruebas faltantes o corrigen pruebas existentes
- `docs`: Confirmaciones que afectan únicamente a la documentación
- `build`: Confirmaciones que afectan a componentes de compilación como la herramienta de compilación, la canalización CI, las dependencias, la versión del proyecto, etc.
- `ci`: Confirmaciones que afectan a componentes operativos como infraestructura, implementación, respaldo, recuperación, intengracion continua ...
- `chore`: Confirmaciones varias, por ejemplo, modificación`.gitignore`
- `revert`: Se usa cuando se necesita deshacer o revertir un commit previo. Git genera automáticamente un mensaje de commit `revert` al ejecutar el comando `git revert`, pero también se puede personalizar.



_Tipos_ distintos a `fix:` y `feat:` están permitidos, por ejemplo [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional) (basados en [la convención de Angular](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)) que recomienda `build:`, `chore:`, `ci:`, `docs:`, `style:`, `refactor:`, `perf:`, `test:`, y otros.

 _Tipos_ distintos a `fix:` y `feat:` están permitidos, por ejemplo [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional) (basados en [la convención de Angular](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)) que recomienda `build:`, `chore:`, `ci:`, `docs:`, `style:`, `refactor:`, `perf:`, `test:`, y otros.
### Scopes/Ambitos
Proporciona `scope`información contextual adicional.

- Es una parte **opcional** del formato.
- Los alcances permitidos dependen del proyecto específico
- No utilice identificadores de problemas como alcances
###  Breaking Changes Indicator/Indicador de cambios de ruptura
	Los cambios importantes deben indicarse con un `!`antes de `:`en la línea de asunto, 
	por ejemplo`feat(api)!: remove status endpoint`

### Descripción

Contiene una  `description` concisa del cambio.

- Es una parte **obligatoria** del formato
- Utilice el imperativo, tiempo presente: "cambiar", no "cambió" ni "cambia".
    - Piensa `This commit will...`en o`This commit should...`
- No escribas con mayúscula la primera letra
- Sin punto ( `.`) al final

### Cuerpo

Debe `body`incluir la motivación para el cambio y contrastarla con el comportamiento anterior.

- Es una parte **opcional** del formato.
- Utilice el imperativo, tiempo presente: "cambiar", no "cambió" ni "cambia".
- Este es el lugar para mencionar los identificadores de problemas y sus relaciones.

### Pie de página


Debe `footer`contener cualquier información sobre **cambios importantes** y también es el lugar para **hacer referencia a los problemas** a los que se refiere esta confirmación.

- Es una parte **opcional** del formato.
- **Opcionalmente,** haga referencia a un problema por su ID.
- **Los cambios importantes** deben comenzar con la palabra `BREAKING CHANGES:`seguida de un espacio o dos líneas nuevas. El resto del mensaje de confirmación se utiliza para esto.
### Ejemplos

- **feat** (nuevas características):
    
    - `feat: agregar página de perfil de usuario`
    - `feat: implementar el interruptor de modo oscuro`
- **fix** (corrección de errores):
    
    - `fix: corregir error tipográfico en la página de configuración de usuario`
    - `fix: resolver el problema de bloqueo en la pantalla de inicio de sesión`
- **refactor** (reestructuración sin cambios de funcionalidad):
    
    - `refactor: dividir las funciones utilitarias en archivos separados`
    - `refactor: simplificar la gestión de solicitudes a la API`
- **perf** (mejoras de rendimiento):
    
    - `perf: reducir el tiempo de carga optimizando el tamaño de las imágenes`
    - `perf: cachear datos de acceso frecuente para mejorar el rendimiento`
- **style** (cambios de formato y estilo sin afectar el funcionamiento):
    
    - `style: corregir la indentación en main.js`
    - `style: eliminar espacios en blanco adicionales en archivos HTML`
- **test** (agregar o corregir pruebas):
    
    - `test: agregar pruebas unitarias para el componente de inicio de sesión`
    - `test: actualizar casos de prueba para el endpoint de la API`
- **docs** (documentación):
    
    - `docs: añadir ejemplos de uso de la API en el README`
    - `docs: actualizar las instrucciones de instalación`
- **build** (afecta el sistema de compilación o CI/CD):
    
    - `build: agregar soporte de Docker para compilación en producción`
    - `build: actualizar la canalización de CI para construir más rápido`
- **ops** (infraestructura y operaciones):
    
    - `ops: configurar copias de seguridad automáticas para la base de datos`
    - `ops: configurar monitoreo de salud del servidor`
- **chore** (varios cambios menores):
    
    - `chore: actualizar .gitignore para excluir archivos de registro`
    - `chore: eliminar dependencias no utilizadas en package.json`
    
- **rever** (revertir cambios menores):
    - `Revert ops: configurar copias de seguridad automáticas para la base de datos`
    - `Revert ops: configurar monitoreo de salud del servidor`

**feat** (nuevas características)

```bash
git commit -m "tipo: descripción del cambio"

```

```bash
git commit -m "feat(api)!: eliminar el endpoint de estado"
git commit -m "feat(db)!: cambiar estructura de la base de datos para usuarios"
```

**fix** (corrección de errores):

```bash
git commit -m "feat(app): agregar página de perfil de usuario"
git commit -m "feat(app): implementar el interruptor de modo oscuro"
```

```bash
git commit -m "fix(auth)!: cambiar el flujo de autenticación"
git commit -m "fix(payment)!: actualizar proceso de facturación con nuevo API"
```


**Refactor** (reestructuración sin cambios de funcionalidad):

```bash
git commit -m "refactor(api): dividir las funciones utilitarias en archivos separados"
git commit -m "refactor(code): simplificar la gestión de solicitudes a la API"

```

```bash
git commit -m "refactor(config)!: reorganizar configuración para compatibilidad con nuevas versiones"
git commit -m "refactor(storage)!: cambiar la estructura de almacenamiento de archivos"

```


**perf** (mejoras de rendimiento):

```bash
git commit -m "perf(images): reducir el tiempo de carga optimizando el tamaño de las imágenes"
git commit -m "perf(images): cachear datos de acceso frecuente para mejorar el rendimiento"

```
**style** (cambios de formato y estilo sin afectar el funcionamiento):

```bash
git commit -m "style(code): corregir la indentación en main.js"
git commit -m "style(code): eliminar espacios en blanco adicionales en archivos HTML"

```

**test** (agregar o corregir pruebas):

```bash
git commit -m "test(code): agregar pruebas unitarias para el componente de inicio de sesión"
git commit -m "test(code): actualizar casos de prueba para el endpoint de la API"

```

**docs** (documentación):

```bash
git commit -m "docs(readme): añadir ejemplos de uso de la API en el README"
git commit -m "docs(readme): actualizar las instrucciones de instalación"

```
**build** (afecta el sistema de compilación o CI/CD):

```bash
git commit -m "build(code): agregar soporte de Docker para compilación en producción"
git commit -m "build(code): actualizar la canalización de CI para construir más rápido"

```
**ops** (infraestructura y operaciones):

```bash
git commit -m "ops(code): configurar copias de seguridad automáticas para la base de datos"
git commit -m "ops(code): configurar monitoreo de salud del servidor"

```
**chore** (varios cambios):

```bash
git commit -m "chore(code): actualizar .gitignore para excluir archivos de registro"
git commit -m "chore(code): eliminar dependencias no utilizadas en package.json"

```
**rever** (revertir varios cambios):

```bash
git commit -m "revert (code): eliminar configuración de monitoreo de salud del servidor"
git commit -m "revert(code ): eliminar configuración de copias de seguridad automáticas para la base de datos"

```

