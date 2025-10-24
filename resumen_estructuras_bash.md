## 🧩 Estructuras de Control en Bash

Las **estructuras de control** permiten decidir qué hacer y cuándo hacerlo dentro de un script. En Bash, controlan el **flujo de ejecución** según condiciones, repeticiones o elecciones del usuario.

---

### 🔹 1. Condicional `if`
**Sirve para:** tomar decisiones según una condición (si algo es verdadero o falso).

**Sintaxis básica:**
```bash
if [ condición ]; then
    # código si es verdadero
else
    # código si es falso (opcional)
fi
```

**Ejemplo:**
```bash
if [ -f "$1" ]; then
    echo "El archivo existe."
else
    echo "No existe el archivo."
fi
```

👉 Se usa cuando querés **verificar algo antes de continuar**, por ejemplo, si existe un archivo, si un número es mayor, o si un texto coincide.

---

### 🔹 2. Bucle `for`
**Sirve para:** repetir acciones con una lista de elementos conocidos.

**Sintaxis:**
```bash
for elemento in lista; do
    # código a repetir
done
```

**Ejemplo:**
```bash
for archivo in *.txt; do
    echo "Procesando $archivo"
done
```

👉 Se usa cuando sabés **cuántos elementos tenés** o **ya tenés una lista** (archivos, nombres, números).

---

### 🔹 3. Bucle `while`
**Sirve para:** repetir acciones **mientras se cumpla una condición** (mientras sea verdadera).

**Sintaxis:**
```bash
while [ condición ]; do
    # código a repetir
done
```

**Ejemplo:**
```bash
contador=1
while [ $contador -le 3 ]; do
    echo "Intento $contador"
    contador=$((contador+1))
done
```
📤 Salida:
```
Intento 1
Intento 2
Intento 3
```

👉 Se usa cuando **no sabés cuántas veces vas a repetir**, pero querés seguir hasta que algo cambie (ej: esperar que el usuario ingrese algo correcto).

---

### 🔹 4. Bucle `until`
**Sirve para:** repetir acciones **hasta que la condición sea verdadera.**

**Sintaxis:**
```bash
until [ condición ]; do
    # código a repetir
done
```

**Ejemplo:**
```bash
numero=0
until [ $numero -ge 3 ]; do
    echo "Número: $numero"
    numero=$((numero+1))
done
```
📤 Salida:
```
Número: 0
Número: 1
Número: 2
```

👉 Se usa cuando querés **esperar a que algo pase** (por ejemplo, hasta que exista un archivo o el usuario ingrese una palabra).

🧠 **Diferencia con `while`:**
- `while` repite **mientras la condición sea verdadera**.
- `until` repite **mientras la condición sea falsa**.

📘 Es decir: son opuestos. Lo que detiene a uno, hace continuar al otro.

---

### 🔹 5. `case` (varias opciones)
**Sirve para:** elegir entre varias posibilidades de manera más ordenada que muchos `if`.

**Sintaxis:**
```bash
case variable in
    opcion1)
        # código 1
        ;;
    opcion2)
        # código 2
        ;;
    *)
        # opción por defecto
        ;;
esac
```

**Ejemplo:**
```bash
read -p "Ingrese una letra: " letra
case $letra in
    a) echo "Vocal A" ;;
    e) echo "Vocal E" ;;
    *) echo "Otra letra" ;;
esac
```

👉 Se usa cuando tenés **muchas opciones posibles** (menús o validaciones de texto).

---

### 🔹 6. `select` (menú interactivo)
**Sirve para:** crear menús donde el usuario elige una opción.

**Sintaxis:**
```bash
select variable in opcion1 opcion2 opcion3; do
    # acción según la opción elegida
    break

done
```

**Ejemplo:**
```bash
PS3="Seleccione una opción: "
select animal in perro gato pez; do
    echo "Usted eligió: $animal"
    break
done
```

📤 Salida:
```
1) perro
2) gato
3) pez
Seleccione una opción: 2
Usted eligió: gato
```

👉 Se usa para **pedirle al usuario que elija** entre varias opciones de forma sencilla.

---

### 📘 Resumen final

| Estructura | Qué hace | Cuándo usarla | Palabra final |
|-------------|-----------|----------------|----------------|
| `if` | Evalúa una condición | Tomar decisiones | `fi` |
| `for` | Repite para una lista conocida | Recorrer archivos o valores | `done` |
| `while` | Repite mientras sea verdadero | Bucle dependiente de una condición | `done` |
| `until` | Repite hasta que sea verdadero | Esperar que algo ocurra | `done` |
| `case` | Evalúa múltiples opciones | Menús o validaciones de texto | `esac` |
| `select` | Crea menús interactivos | Pedir al usuario que elija | `done` |

---

📗 **En el apunte:** Capítulo **8.5 – Estructuras de Control (pág. 166–172)**.

Estas estructuras permiten escribir scripts Bash claros, ordenados y fáciles de mantener.

