## 🧩 Funciones en Bash — Resumen docente

### 🔹 ¿Qué son las funciones?
Las **funciones** en Bash permiten agrupar comandos relacionados para reutilizarlos en distintas partes de un script. Facilitan la **organización**, **mantenimiento** y **modularidad** del código.

> 💬 En palabras simples: una función es como un mini-programa dentro de un script.

---

### 🔹 Sintaxis básica
```bash
nombre_funcion() {
    # Comandos a ejecutar
}
```
Para ejecutarla:
```bash
nombre_funcion
```

---

### 🔹 Funciones con argumentos
Las funciones pueden recibir parámetros igual que los scripts. Se accede con variables posicionales:

| Variable | Significado |
|-----------|-------------|
| `$1` | Primer argumento |
| `$2` | Segundo argumento |
| `$3` | Tercer argumento |
| `$@` | Todos los argumentos |

**Ejemplo:**
```bash
saludar() {
    echo "Hola, $1."
}

NOMBRE="Damian"
saludar $NOMBRE
```
📤 **Salida:** `Hola, Damian.`

---

### 🔹 Ventajas
✅ Evitan repetir código.  
✅ Mejoran la estructura del script.  
✅ Facilitan las pruebas.  
✅ Permiten reutilización de lógica.

---

### 🔹 Dónde se colocan
Por convención, las funciones se **definen al inicio** del script y se **llaman después**, en el cuerpo principal o dentro de bucles/condicionales.

**Ejemplo típico:**
```bash
#!/bin/bash

# --- Definición ---
get_extension() {
    ext="${1##*.}"
    case "$ext" in
        txt) echo "txt" ;;
        doc) echo "doc" ;;
        ppt) echo "ppt" ;;
        *)   echo "unknown" ;;
    esac
}

# --- Cuerpo principal ---
for FILE in "$1"/*; do
    EXT=$(get_extension "$FILE")
    mv "$FILE" "$EXT/"
done
```

---

### 🔹 Conceptos clave
| Elemento | Explicación |
|-----------|-------------|
| `()` | Indica definición de función |
| `{}` | Delimita el bloque de comandos |
| `$1`, `$2`, ... | Acceden a los argumentos recibidos |
| `echo` | Devuelve un valor (se captura con `$(...)`) |
| `return` | Devuelve un código de salida (no texto) |

---

### 🔹 Relación con el apunte
Este tema aparece en el **Capítulo 8.5.6 – “Funciones” (pág. 172)** del apunte de *Entorno de Programación*, con el ejemplo `saludar()` y la explicación de argumentos posicionales.

---

### 🧠 Ejercicio práctico sugerido
Crear una función que reciba un nombre de archivo y muestre su extensión:
```bash
mostrar_extension() {
    echo "La extensión es: ${1##*.}"
}
mostrar_extension informe.doc
```
📤 **Salida:** `La extensión es: doc`

---

### 💡 Resumen final
| Concepto | Descripción | Ejemplo |
|-----------|--------------|----------|
| Definición | Agrupa comandos reutilizables | `mi_funcion() { echo hola; }` |
| Llamada | Ejecuta la función | `mi_funcion` |
| Argumentos | Parámetros posicionales | `$1`, `$2`, `$@` |
| Aplicación práctica | Modularizar scripts Bash | Funciones como `get_extension`, `check_conectividad` |

---

## 🌍 Variables de entorno (8.2.2)

Las **variables de entorno** son variables especiales que están disponibles para todos los procesos y comandos ejecutados en ese entorno. Guardan configuraciones del sistema y del usuario (por ejemplo, rutas, nombres o preferencias).

### 🔹 Variables comunes y su significado
| Variable | Descripción | Ejemplo de valor |
|-----------|-------------|------------------|
| `HOME` | Ruta al directorio personal del usuario | `/home/alumna` |
| `PATH` | Rutas donde Bash busca los ejecutables | `/usr/local/bin:/usr/bin:/bin` |
| `USER` | Nombre del usuario conectado | `alumna` |
| `HOSTNAME` | Nombre del equipo | `PC-UAI` |
| `SHELL` | Ruta al intérprete de comandos | `/bin/bash` |
| `PWD` | Directorio de trabajo actual | `/home/alumna/scripts` |
| `OLDPWD` | Directorio anterior | `/home/alumna` |
| `PS1` | Formato del prompt de la terminal | `[\u@\h \W]\$ ` |

### 🔹 Cómo funcionan
Cuando escribís un comando como `ls`, Bash **busca su ubicación** dentro de los directorios listados en la variable `PATH`.  
Por eso podés escribir solo `ls` en vez de `/usr/bin/ls`.

🔍 Podés ver todas las variables de entorno con:
```bash
env
```
O consultar una específica con:
```bash
echo $PATH
echo $HOME
```

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

