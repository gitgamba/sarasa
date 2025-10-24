# Tabla de símbolos y operadores en Bash

---

## 1) Símbolos esenciales

| Símbolo | Nombre | ¿Qué hace? | Ejemplo (comando) | Salida esperada |
|---|---|---|---|---|
| `#` | Comentario | Ignora el resto de la línea | `# esto no se ejecuta` | *(sin salida)* |
| `#!/bin/bash` | Shebang | Define el intérprete del script | (1° línea del script) | *(sin salida)* |
| `$VAR` | Expansión de variable | Inserta el valor de `VAR` | `echo $USER` | `admin` (por ej.) |
| `$(cmd)` | Sustitución de comando | Ejecuta `cmd` y pega su salida | `echo "Hoy: $(date +%Y-%m-%d)"` | `Hoy: 2025-10-14` |
| `` `cmd` `` | Sustitución antigua | Igual a `$(...)` (no recomendado) | ``echo `pwd` `` | `/ruta/actual` |
| `\` | Escape | Evita que el próximo carácter se interprete | `echo "Fecha $(date +%Y-%m-%d\ %H:%M:%S)"` | `Fecha 2025-10-14 10:00:00` |
| `;` | Separador | Ejecuta varios comandos en una línea | `cd /tmp; ls` | Lista de archivos |
| `&&` | AND lógico/seq. | Ejecuta 2° comando si el 1° tuvo éxito | `mkdir datos && echo "OK"` | `OK` |
| `||` | OR lógico/seq. | Ejecuta 2° comando si el 1° falló | `cat x || echo "Error"` | `Error` |

**Errores típicos**: olvidar espacios en tests: `if [ -d "$DIR" ]` ✅ no `if [ -d"$DIR"]` ❌.

---

## 2) Condicionales y pruebas

| Sintaxis | Uso | Ejemplo correcto | Observación |
|---|---|---|---|
| `[ ... ]` | Test básico | `[ -f archivo ]` | Requiere espacios internos |
| `[[ ... ]]` | Test avanzado | `[[ $n -gt 0 && -d $DIR ]]` | Más seguro y expresivo |
| `!` | Negación | `[ ! -d "$DIR" ]` | Invierte el resultado |

**Pruebas comunes**: `-f` (archivo regular), `-d` (directorio), `-x` (ejecutable), `-n`/`-z` (longitud de cadena), comparaciones `-eq -ne -gt -lt`.

---

## 3) Redirección y tuberías

| Símbolo | Explicación | Ejemplo | Resultado |
|---|---|---|---|
| `>` | Sobrescribe salida a archivo | `echo hola > a.txt` | Crea/sobrescribe `a.txt` |
| `>>` | Agrega al final | `echo mundo >> a.txt` | Añade a `a.txt` |
| `<` | Entrada desde archivo | `cat < a.txt` | Muestra contenido |
| `|` | Pipe | `cat a.txt | grep o` | Filtra líneas con `o` |

---

## 4) Wildcards y expansiones

| Símbolo | Explicación | Ejemplo | Coincidencias |
|---|---|---|---|
| `*` | 0 o más caracteres | `ls *.txt` | `apunte.txt`, `log.txt` |
| `?` | 1 carácter | `ls archivo?.txt` | `archivo1.txt` |
| `{a,b}` | Lista/brace expansion | `echo {rojo,verde}.txt` | `rojo.txt verde.txt` |
| `./` | Ejecutar local | `./script.sh` | Ejecuta el script |
| `/*` | Todos dentro de dir | `for F in $DIR/*; do ...; done` | Itera archivos |

---

## 5) Parámetros especiales y aritmética

| Símbolo | Significado | Ejemplo | Salida |
|---|---|---|---|
| `$#` | Cantidad de argumentos | `echo $#` (`./s.sh a b`) | `2` ( Indica el número de argumentos pasados al script o comando.)|
| `$@` | Todos los argumentos | `echo $@` | `a b` (Contiene todos los argumentos pasados al script o comando como una lista separada por espacios. Es útil para iterar sobre todos los argumentos en un bucle.)|
| `$0` | Nombre del script | `echo $0` | `./s.sh` |
| `$1`, `$2` | Arg. posicionales | `echo $1` | Primer argumento |
| `$?` | Exit status previo | `false; echo $?` | `1` (código de salida del último comando ejecutado. Un valor de 0 generalmente indica éxito, mientras que un valor distinto de 0 indica un fallo o error.)|
| `$(( ... ))` | Aritmética | `echo $((5+3))` | `8` |

---

## 6) `find -exec` y placeholders

| Pieza | ¿Qué es? | Ejemplo | Explicación |
|---|---|---|---|
| `{}` | Placeholder | `-exec rm -f {} \;` | Sustituye cada archivo hallado |
| `\;` | Escapar `;` | `... \;` | Evita que el `;` lo consuma la shell |
| `-type f` | Tipo archivo regular | `find . -type f` | Solo archivos |
| `-size -100M` | Tamaño | `-size -100M` | Menor que 100 MiB |

**Patrón clásico de parcial**: `find RUTA -type f -size -100M -exec rm -f {} \;` — elimina archivos regulares menores a 100 MiB.

---

## 7) Fragmentos típicos de examen (didácticos)

### a) `chmod` numérico y simbólico
- Numérico: `chmod 540 file.txt` → `r-x` (propietario), `r--` (grupo), `---` (otros).
- Simbólico equivalente desde `-rw-r--r--`: `chmod u=rx,g=r,o= file.txt`.

### b) Expresión regular de legajo `L-NNNN/N`
- RegEx POSIX extendido: `^[A-Za-z]-[0-9]{4}/[0-9]$`
- Ejemplo de verificador:
```bash
#!/bin/bash
[[ $1 =~ ^[A-Za-z]-[0-9]{4}/[0-9]$ ]] && echo "Legajo válido" || echo "Legajo inválido"
```

### c) `date` con espacios (escape correcto)
```bash
echo "$(date +%Y-%m-%d\ %H:%M:%S)"
```

### d) jobs y disown (control de tareas)
```bash
chromium &
firefox &
jobs
# [1] - running chromium
# [2] + running firefox
disown firefox  # Desasocia firefox de la shell actual
```

---

## 8) Errores frecuentes y cómo evitarlos
- **Espacios en `[` `]`**: siempre `if [ -d "$DIR" ]; then ... fi`.
- **Citas**: comillas dobles protegen espacios y expanden variables; simples no.
- **`$(...)` mejor que acentos graves**: anida y es más legible.
- **Redirecciones**: `>` sobrescribe, `>>` agrega.
- **Operadores secuenciales**: `cmd1 && cmd2` (solo si OK), `cmd1 || cmd2` (solo si falla).

---

## 9) Mini-guía de práctica
1. Crear archivo y filtrar con `grep` y `|`.
2. Probar `find` con `-type`, `-size`, `-exec` (usar `echo` en lugar de `rm` primero).
3. Escribir un `verificador_legajo.sh` y testear casos válidos/invalidos.
4. Cambiar permisos con `chmod` numérico y simbólico y verificar con `ls -l`.

> Consejo docente: simular la salida que esperan ver en el parcial; por ejemplo, tras `chmod` correr `ls -l` y explicar cada tripleta `rwx`.

---

## 🧩 El comando `test` y los corchetes `[ ]`

El comando **`test`** se utiliza para **evaluar condiciones** (comparaciones numéricas, de texto o de archivos).  
Si la condición es verdadera devuelve **0** (éxito), si es falsa devuelve **1** (error).

👉 Forma equivalente:
```bash
test expresion
# o
[ expresion ]
```

Ambas son iguales, solo que los corchetes son una **forma abreviada** más común en los scripts.

### 🔹 Operadores de comparación de cadenas
| Operador | Significado | Ejemplo | Resultado |
|-----------|--------------|----------|-------------|
| `=` | Igualdad de cadenas | `[ "$a" = "$b" ]` | true si iguales |
| `!=` | Desigualdad | `[ "$a" != "$b" ]` | true si distintas |
| `-z` | Cadena vacía | `[ -z "$a" ]` | true si $a está vacía |
| `-n` | Cadena no vacía | `[ -n "$a" ]` | true si tiene contenido |

### 🔹 Operadores numéricos
| Operador | Significado | Ejemplo | Resultado |
|-----------|--------------|----------|-------------|
| `-eq` | Igual a | `[ "$a" -eq "$b" ]` | true si son iguales |
| `-ne` | Distinto de | `[ "$a" -ne "$b" ]` | true si distintos |
| `-gt` | Mayor que | `[ "$a" -gt "$b" ]` | true si $a > $b |
| `-lt` | Menor que | `[ "$a" -lt "$b" ]` | true si $a < $b |
| `-ge` | Mayor o igual | `[ "$a" -ge "$b" ]` | true si $a ≥ $b |
| `-le` | Menor o igual | `[ "$a" -le "$b" ]` | true si $a ≤ $b |

### 🔹 Operadores de archivos
| Operador | Qué verifica | Ejemplo | Resultado |
|-----------|--------------|----------|-------------|
| `-e` | Si el archivo existe | `[ -e archivo.txt ]` | true si existe |
| `-f` | Si es un archivo regular | `[ -f archivo.txt ]` | true si es archivo |
| `-d` | Si es un directorio | `[ -d carpeta ]` | true si es carpeta |
| `-r` | Si tiene permiso de lectura | `[ -r archivo.txt ]` | true si se puede leer |
| `-w` | Si tiene permiso de escritura | `[ -w archivo.txt ]` | true si se puede escribir |
| `-x` | Si es ejecutable | `[ -x script.sh ]` | true si se puede ejecutar |

### 🔹 Ejemplo práctico
```bash
#!/bin/bash

if [ -d "$1" ]; then
    echo "La carpeta existe."
else
    echo "No existe."
fi
```
📤 **Salida:** depende de si el directorio pasado como argumento existe o no.

---

📘 **Fuente:** Apunte de Entorno de Programación — Secciones *8.2.2 Variables de entorno* y *8.5.6 Funciones*, pág. 172 (T.U.I.A. – U.N.R.)


