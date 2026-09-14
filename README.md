# Ejercicio: Git y Java en un Dev Container

Este ejercicio practica los comandos básicos de Git dentro del contenedor de desarrollo de este repositorio. Completa las actividades en orden y marca cada casilla solamente después de comprobar el resultado.

## Objetivo y forma de trabajo

Al terminar, deberás demostrar que puedes compilar y ejecutar Java, preparar cambios, crear y corregir commits, combinar una rama, sincronizarte con un remoto, generar un parche y excluir archivos que no deben versionarse.

En cada etapa:

1. Ejecuta los comandos indicados.
2. Realiza la modificación solicitada.
3. Comprueba la evidencia indicada.
4. Marca la casilla de completado.

No marques una casilla si el comando produjo un error o si no puedes explicar qué cambió.

## 1. ¿Qué hay en este entorno?

El contenedor está basado en Debian Trixie y tiene disponibles:

- Java OpenJDK 25 (incluye `java` y `javac`).
- Maven instalado mediante la configuración del Dev Container.
- Git.
- Visual Studio Code con extensiones para Java, Maven, Spring Boot y Mermaid.

Comprueba las versiones desde la terminal:

```bash
git --version
java -version
javac -version
mvn --version
```

**Actividad:** copia las versiones que obtuviste en tus apuntes o en el reporte de la práctica.

- [ ] Confirmé que Git, `java`, `javac` y Maven están disponibles.

Los ejemplos están en `src/`. La carpeta `bin/` se usa solamente para archivos compilados y está excluida por `.gitignore`.

## 2. Compilar y ejecutar los ejemplos

Desde la raíz del repositorio, ejecuta:

```bash
mkdir -p bin
javac -d bin src/Saludo.java src/Calculadora.java
java -cp bin Saludo
java -cp bin Calculadora
```

Observa que `javac` genera archivos `.class` en `bin/`, mientras que `java` ejecuta la clase compilada. Los archivos fuente que se deben compartir son los `.java`; los resultados compilados no se agregan al repositorio.

**Actividad:** modifica el mensaje de `Saludo.java` o agrega una operación a `Calculadora.java`, vuelve a compilar y ejecuta nuevamente la clase correspondiente.

- [ ] Compilé y ejecuté los dos ejemplos.
- [ ] Realicé una modificación en un archivo `.java` y comprobé su resultado.

## 3. Preparar el repositorio

Consulta qué archivos reconoce Git:

```bash
git status
git status --short
```

Agrega el ejercicio al área de preparación y revisa el cambio:

```bash
git add README.md .gitignore src/Saludo.java src/Calculadora.java
git diff --cached
```

`git add` prepara una fotografía de los cambios para el siguiente commit. `git diff --cached` muestra exactamente lo que está preparado.

**Actividad:** prepara únicamente los cuatro archivos indicados. Antes de continuar, verifica que `git diff --cached` muestre tus cambios y que no incluya archivos `.class`.

- [ ] Revisé el estado inicial con `git status`.
- [ ] Agregué los archivos fuente y de configuración con `git add`.
- [ ] Revisé el contenido preparado con `git diff --cached`.

## 4. Crear y corregir commits

Crea el primer commit:

```bash
git commit -m "Agrega ejercicio de Git y Java"
git log --oneline -1
```

Ahora modifica una línea de `README.md` o de uno de los ejemplos. Revisa la diferencia antes de preparar el cambio:

```bash
git diff
git add README.md
git diff --cached
```

Si todavía no has compartido el commit, puedes incorporar ese cambio al commit anterior:

```bash
git commit --amend --no-edit
git log --oneline -1
```

`--amend` reemplaza el commit más reciente. No lo uses para reescribir un commit que otras personas ya descargaron sin acordarlo con el equipo.

**Actividad:** después del primer commit, cambia una línea del README, ejecuta `git diff`, prepara el cambio y usa `git commit --amend --no-edit`. Comprueba que el último commit contiene la modificación.

- [ ] Creé el primer commit con un mensaje descriptivo.
- [ ] Revisé un cambio con `git diff` antes de prepararlo.
- [ ] Incorporé el cambio al commit anterior con `git commit --amend`.

## 5. Crear una rama y combinarla con `merge`

Practica una modificación aislada en una rama:

```bash
git switch -c mejora-instrucciones
```

Edita `README.md`, guarda el cambio y ejecuta:

```bash
git add README.md
git commit -m "Aclara las instrucciones del ejercicio"
git switch main
git merge mejora-instrucciones
```

Si Git informa un conflicto, abre los archivos marcados, conserva la versión correcta, elimina los marcadores `<<<<<<<`, `=======` y `>>>>>>>`, y después ejecuta:

```bash
git add <archivo-resuelto>
git commit
```

**Actividad:** en `mejora-instrucciones`, agrega una aclaración breve al README. Después integra la rama en `main` y comprueba el historial con `git log --oneline --all --decorate`.

- [ ] Creé la rama `mejora-instrucciones`.
- [ ] Creé un commit dentro de esa rama.
- [ ] Regresé a `main` y combiné la rama con `git merge`.

## 6. Compartir y recibir cambios

Revisa el remoto configurado y las ramas disponibles:

```bash
git remote -v
git branch --all
```

Para enviar la rama actual al repositorio remoto:

```bash
git push -u origin main
```

Para traer cambios del remoto y combinarlos en la rama actual:

```bash
git pull
```

Antes de usar `push` o `pull`, confirma que tienes permiso de acceso y que sabes cuál es la rama compartida. En un repositorio de clase, trabaja preferentemente en una rama propia y coordina el `merge` con tu equipo.

**Actividad:** no ejecutes `push` sobre un repositorio ajeno sin autorización. Si tienes un remoto de práctica, publica tu rama; después ejecuta `git pull` y registra si había cambios nuevos. Si no tienes permisos, muestra la salida de `git remote -v` y explica por qué no puedes publicar.

- [ ] Revisé el remoto y las ramas disponibles.
- [ ] Intenté o realicé un `push` con autorización.
- [ ] Ejecuté `git pull` y comprendí su resultado.

## 7. Generar un archivo de parche

Un parche es un archivo de texto con diferencias que otra persona puede revisar o aplicar. Primero realiza y guarda un cambio, y luego genera el parche contra el commit anterior:

```bash
git diff HEAD^ HEAD > cambio.patch
```

Para inspeccionar el archivo:

```bash
git diff --no-index /dev/null cambio.patch
```

También puedes generar un parche de cambios que aún no has preparado:

```bash
git diff > cambios-sin-commit.patch
```

**Actividad:** genera `cambio.patch` a partir de dos commits y ábrelo como texto. Identifica las líneas que comienzan con `+` y `-` y explica qué representan.

- [ ] Generé un archivo `.patch` con `git diff`.
- [ ] Inspeccioné el parche y expliqué sus diferencias.

Los archivos `.patch` de este ejercicio también están excluidos por `.gitignore`, porque son productos temporales de práctica. Si necesitas entregarlo, agrégalo explícitamente con `git add -f nombre.patch`.

## 8. Comprobar el `.gitignore`

El archivo `.gitignore` evita que Git muestre salidas compiladas y archivos temporales. Deben ignorarse, entre otros:

- Todos los `.class` y la carpeta `bin/`.
- Archivos `.bin` y `.javac`.
- Carpetas comunes de compilación como `target/`, `out/` y `build/`.
- Archivos temporales de editores y parches generados durante la práctica.

Comprueba una regla concreta con:

```bash
git check-ignore -v bin/Saludo.class ejemplo.bin ejemplo.javac
git status --ignored --short
```

El objetivo es que el repositorio conserve el código fuente `.java`, el README y la configuración, pero no los resultados generados por la compilación.

**Actividad:** crea archivos de prueba para `ejemplo.bin` y `ejemplo.javac`, y comprueba que Git los ignore. Ejecuta `git status --short` y confirma que no aparecen como archivos sin seguimiento.

- [ ] Confirmé que se ignoran `bin/`, `.class`, `.bin` y `.javac`.
- [ ] Confirmé que los archivos compilados no aparecen como cambios pendientes.


## Lista final de comprobación

Antes de entregar, verifica todo lo siguiente:

- [ ] `git status` muestra solamente cambios intencionales o está limpio.
- [ ] El historial contiene commits descriptivos.
- [ ] Existe una rama integrada mediante `merge`.
- [ ] El repositorio conserva los archivos `.java`, pero no archivos `.class`, `.bin` ni `.javac` versionados.
- [ ] El README conserva todas las casillas marcadas como evidencia de la práctica.

## Notita :)
Este ejercicio me ayudó a entender la diferencia entre `git add`, `git commit` y `git commit --amend`. 