# Apuntes Github
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


