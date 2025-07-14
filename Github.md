# Apuntes Github
Instrucciones para llevar a cabo navegacion de git
## 1. Registrar credenciales y configuraciones

### 1.1 Registro de usuario
Luego de verificar que no hay credenciales registradas con `git config --global -l` se configuran mis credenciales: 
```bash
git config --global user.name "Tatiana Villamizar"
git config --global user.email "tm8497077@gmail.com"
```

### 1.2 Configuraciones adicionales
- Se indica que la rama principal es main
`git config --global init.defaultBranch main`
- Se configura visual como editor
`git config --global core.editor "code --wait"`
- Permite ver y modificar las configuraciones de github
`git config --global -e`
## 2. Inicializar repositorio
Una vez se tienen las configuraciones hecha y ya se empezado ha trabjar con los archivos, se debe empezar a hacer commit.

- Se inicializa el repositorio
`git init`
- Se puede visualizar el estado de los archivos
`git status`
- Se agrega nombre de archivo o .\archivo en windows (ej. home.html)
`git add .\home.txt`
o para agregar todo lo creado
`git add .`
- Se crea el commit, el :(es un emogi segun lo que haga el commit):
`git commit -m ":tada: proyecto inicial"`
- Se ven los commit creados
`git log`

## 3. Crear y trabajar con una rama
- Se crea nueva rama
`git checkout -b nombrederama`
- Se hace nuevo commit con lo cambios que se van a agregar a esta rama
- Se mueve a la rama main
`git checkout main`
- Si en la rama main se hace nuevos cambios y en la nueva rama tambien, pueden tener conflictos, para integrar lo nuevos cambios de la nueva rama a la rama principal se hace.
`git merge nombre-de-rama-que-quiere-integrar`

## 4. Subir a git
- Se indica el link del repositorio creado, la rama llamada main y se empuja
```bash
git remote add origin https://github.com/Tatii22/info-GIT.git
git branch -M main
git push -u origin main
```
- se sube la rama que desee
`git push -u origin nom-rama`

# Comandos para tener en cuenta
`git pull` es un comando que descarga contenido de un repositorio remoto y lo combina con el repositorio local, actualizando el estado de este último con los cambios más recientes. 

# Comandos esenciales para trabajar con ramas en Git

| Acción                                  | Comando                                      | Descripción breve                                  |
|----------------------------------------|----------------------------------------------|----------------------------------------------------|
| Crear una nueva rama                   | `git branch nombre-rama`                     | Solo la crea, no cambia a ella.                    |
| Cambiarse de rama                      | `git checkout nombre-rama`                   | Cambia a la rama especificada.                     |
| Crear y cambiarse a una rama           | `git checkout -b nombre-rama`                | Crea la rama y cambia a ella (todo en uno).        |
| Cambiarse con switch (Git moderno)     | `git switch nombre-rama`                     | Alternativa a `checkout`.                  |
| Crear y cambiarse con switch           | `git switch -c nombre-rama`                  | Alternativa a `checkout -b`.               |
| Ver todas las ramas locales            | `git branch`                                 | Lista las ramas locales.                           |
| Fusionar otra rama a la actual         | `git merge nombre-rama`                      | Mezcla los cambios de esa rama en la actual.       |
| Subir rama nueva a GitHub              | `git push -u origin nombre-rama`             | Sube y vincula con GitHub.                         |
| Traer cambios de una rama remota       | `git pull origin nombre-rama`                | Baja cambios de GitHub para esa rama.              |
| Cambiar nombre a la rama actual        | `git branch -m nuevo-nombre`                 | Renombra la rama en la que estás.                  |
| Borrar una rama local (fusionada)      | `git branch -d nombre-rama`                  | Borra la rama si ya fue fusionada.                 |
| Borrar una rama local (forzado)        | `git branch -D nombre-rama`                  | Borra sin importar si fue fusionada.               |
| Borrar una rama remota                 | `git push origin --delete nombre-rama`       | Elimina la rama en el repositorio remoto.          |

