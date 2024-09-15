# Guía para trabajar con Git en proyectos de Bitcoin Open Source

Trabajar en proyectos open source en Bitcoin, especialmente cuando se hace fork de un repositorio y se contribuye con cambios, requiere un flujo organizado para evitar conflictos y mantener un trabajo limpio. Aquí te dejo una guía de buenas prácticas y algunas aclaraciones, para asegurar que tu trabajo no entre en conflicto con otros colaboradores y que mantengas tu fork y ramas bien gestionadas.

Estas recomendaciones son una guía general para trabajar en proyectos open source, pero cada proyecto puede tener sus propias reglas, flujos de trabajo y convenciones. Siempre es recomendable revisar las directrices específicas del proyecto, como las contribuciones (contributing guidelines), los issues abiertos, PR, y las políticas de revisión de código. Antes de empezar cualquier trabajo importante, es una buena práctica comunicarte con los mantenedores del proyecto o con otros colaboradores, y confirmar que tu enfoque está alineado con los objetivos del proyecto.

## Conceptos
En Git y GitHub, los conceptos de upstream y remote son fundamentales para gestionar repositorios de manera eficiente, especialmente cuando estás colaborando o trabajando con un repositorio "forkeado", bajo estas líneas trato de explicar cada concepto.

### Remote (Remoto)
Un remote es un repositorio que se encuentra alojado en un servidor remoto (como GitHub, GitLab, etc.), fuera de tu máquina local. Cuando clonas o creas un repositorio, automáticamente se crea un remote llamado origin que apunta a ese repositorio remoto original.
- **Origen del término**: El nombre origin es simplemente el nombre por defecto que Git asigna al primer repositorio remoto con el que estás trabajando. Sin embargo, puedes tener varios remotes y nombrarlos de forma personalizada.
- Para mostrar una **lista de todos los repositorios remotos** asociados con tu proyecto puedes hacer esto:
```
git remote -v
```
- **Ejemplo**: Si clonas un repositorio de GitHub, el remote será el URL de dicho repositorio (por ejemplo, `https://github.com/usuario/repo.git`), y el nombre será `origin`. 

### Upstream

Upstream es un tipo de `remote` que hace referencia al repositorio original del cual hiciste un fork. Cuando trabajas con un fork en GitHub, tu repositorio local estará vinculado al `origin` (tu repositorio forkeado en tu cuenta), pero puedes agregar el repositorio original como un `remote` adicional llamado upstream.

Por qué usar **upstream**:
Si has hecho un fork de un proyecto en GitHub, es importante que mantengas tu copia sincronizada con el repositorio original. `Upstream` te permite traer los últimos cambios del proyecto original a tu repositorio para que tu trabajo esté siempre al día.
Cómo agregar un `upstream`:
```
git remote add upstream https://github.com/autor_del_repo/repo_original.git
```
Ejemplo de uso de `upstream`:

Descargar los cambios del repositorio original (sin aplicar):
```
git fetch upstream
```
Fusionar los cambios de `upstream` en tu rama `main`:
```
git merge upstream/main
```
### Diferencia clave entre origin y upstream
- `origin`: Es tu copia forkeada del repositorio, donde subirás tus cambios.
- `upstream`: Es el repositorio original (del cual hiciste el fork) y que puedes usar para obtener los últimos cambios realizados por otros colaboradores.



## Buenas prácticas
### Sincroniza regularmente tu fork con el repositorio original
Para asegurarte de que trabajas con la última versión del proyecto y evitar conflictos futuros:

1. Añade el repositorio original como `upstream`:
- Si no lo has hecho ya, añade el repositorio original como un remoto:
```
git remote add upstream <URL_del_repo_original>
```
- Verifica que lo has añadido correctamente:
```
git remote -v
```
2. Trae los últimos cambios del repositorio original:
    - Antes de empezar a trabajar, asegúrate de traer cualquier cambio reciente:
```
git fetch upstream
```
3. Fusiona o rebase los cambios del repositorio original en tu fork:
    - Para mantener tu rama `main` actualizada con los cambios del original:
    ```
    git merge upstream/main
    ```
    - O, si prefieres un historial más limpio:
    ```
    git rebase upstream/main
    ```

### Trabaja en ramas específicas para cada funcionalidad o corrección

Nunca trabajes directamente en la rama main de tu fork. En su lugar:
1. Crea una nueva rama para cada funcionalidad o corrección:
    - Cambia a main y crea una nueva rama:
    
    ```bash
    git checkout main
    git checkout -b <nombre_de_la_nueva_rama>
    ```

    - Esto asegura que los cambios en main se mantengan separados y limpios.

2. Confirma y sube los cambios a tu fork:
    - Haz commits locales para registrar tu progreso:
      
    ```bash
    git add .
    git commit -m "Descripción de los cambios"
    ```
    
    - Sube tu rama al remoto:
      
    ```
    git push origin <nombre_de_la_nueva_rama>
    ```

### Mantén tus ramas locales organizadas

1. Borra ramas antiguas que ya no uses:
    - Si una rama ya ha sido fusionada o no es necesaria, elimínala:
    ```
    git branch -d <nombre_de_la_rama>
    ```
    - Si no ha sido fusionada pero ya no la necesitas, puedes forzar su eliminación:
    ```
    git branch -D <nombre_de_la_rama>
    ```
2. Sincroniza las ramas remotas eliminadas:
    - Si una rama ha sido eliminada en el repositorio remoto, puedes limpiar las referencias a esas ramas en tu repositorio local:
    ```
    git fetch --prune
    ```

### Comparar tu fork con el original y evitar duplicidad de trabajo

Antes de trabajar en algo, es importante verificar si alguien más ya está trabajando en una funcionalidad similar. Puedes hacerlo de esta manera:

1. Verifica los cambios en el repositorio original:
    - Comprueba si hay cambios en el repositorio original que afecten tu trabajo:
    ```
    git fetch upstream
    git diff upstream/main main
    ```

2. Usa GitHub para comparar:
    - En GitHub, puedes crear un "Pull Request" entre el repositorio original y tu fork para ver si hay cambios en el código que afecten tu trabajo.
    - También revisa las Pull Requests abiertas para ver si alguien más está trabajando en lo mismo que tú.


### Mantén una buena comunicación con la comunidad

Discute tus ideas antes de empezar:
Si estás trabajando en un proyecto colaborativo, es una buena práctica abrir un issue o comentar en un Pull Request antes de empezar a trabajar en una nueva funcionalidad o corrección.
Esto evitará duplicidad de trabajo y te permitirá obtener retroalimentación temprana.
Avisa si estás trabajando en algo:
Cuando empiezas a trabajar en una funcionalidad o corrección, comunícalo en el proyecto para que la comunidad sepa en qué estás trabajando.


## Flujos de trabajo
### ¿Cómo se empieza a trabajar en un proyecto (típicamente)?

1. Fork del repositorio original en GitHub.
2. Clonas tu fork en tu máquina local (el remote origin apunta a tu fork).
3. Agregamos el remote upstream para poder sincronizarse con el repositorio original.
3. Antes de hacer cambios en tu fork, traes los últimos cambios desde `upstream` para asegurarte de que estás trabajando con la versión más actualizada.
5. Haces los cambios en tu fork y luego haces un pull request para contribuir al repositorio original.

### ¿Cómo compruebo si estoy al día de los cambios en mi proyecto?
1. Sincroniza tu fork con el repositorio original
```
git fetch upstream → git merge upstream/main).
```
2. Crea una rama nueva para cada funcionalidad o corrección
```
git checkout -b <rama_nueva>
```
3. Confirma tus cambios localmente
```
git commit -m "Descripción"
```
4. Sube tu rama al remoto
```
git push origin <rama_nueva>
```
5. Abre un Pull Request si quieres contribuir con tus cambios al proyecto original.
6. Borra ramas locales que ya no necesites
```
git branch -d <nombre_de_la_rama>
```
7. Comunica tus cambios o ideas en la comunidad del proyecto (issues, pull requests).
