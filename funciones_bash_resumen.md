## 🧩 Funciones en Bash

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
