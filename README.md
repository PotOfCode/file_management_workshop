## 📚 Guía Completa de Manejo de Archivos en Python

## 🌟 Introducción  
Los archivos son esenciales para almacenar y gestionar datos en programas. En Python, podemos trabajar con diversos tipos de archivos para guardar configuraciones, procesar datos o interactuar con APIs. ¡Aprendamos cómo!

---

## 📌 Fase 1: Tipos de Archivos  

### **1.1 Archivos de Texto**  
- **Extensión común**: `.txt`, `.log`, `.csv`  
- **Características**:  
  - Almacenan texto legible por humanos.  
  - Ideales para configuraciones, registros o datos simples.  
```python
# Ejemplo: Crear un archivo de texto
with open("notas.txt", "w") as archivo:
    archivo.write("Hoy aprendí Python\n")
```

### **1.2 Archivos Binarios**  
- **Extensión común**: `.jpg`, `.png`, `.exe`  
- **Características**:  
  - Guardan datos en formato binario (no legible directamente).  
  - Usados para imágenes, ejecutables o datos serializados.  
```python
# Ejemplo: Leer una imagen
with open("foto.jpg", "rb") as archivo:
    datos = archivo.read()
```

### **1.3 Archivos CSV**  
- **Extensión**: `.csv`  
- **Características**:  
  - Almacenan datos tabulares (filas y columnas).  
  - Cada valor se separa por comas u otros delimitadores.  

### **1.4 Archivos JSON**  
- **Extensión**: `.json`  
- **Características**:  
  - Guardan datos estructurados como diccionarios o listas.  
  - Ampliamente usados en APIs y configuraciones.  

---

## 📌 Fase 2: Operaciones Básicas  

### **2.1 Apertura y Cierre**  
- **Función `open()`**: Abre un archivo.  
- **Método `close()`**: Cierra el archivo manualmente.  
```python
archivo = open("datos.txt", "r")
contenido = archivo.read()
archivo.close()  # ¡No olvides cerrar!
```

### **2.2 Lectura de Archivos**  
| Método          | Descripción                          |  
|-----------------|--------------------------------------|  
| `read()`        | Lee todo el contenido como un string |  
| `readline()`    | Lee una línea a la vez               |  
| `readlines()`   | Devuelve una lista de líneas         |  

**Ejemplo**:  
```python
with open("datos.txt", "r") as archivo:
    lineas = archivo.readlines()
    for linea in lineas:
        print(linea.strip())  # .strip() elimina espacios/saltos
```

### **2.3 Escritura de Archivos**  
| Modo   | Descripción                          |  
|--------|--------------------------------------|  
| `w`    | Crea/sobrescribe un archivo          |  
| `a`    | Añade contenido al final             |  
| `r+`   | Lectura y escritura simultánea       |  

**Ejemplo**:  
```python
with open("registro.log", "a") as archivo:
    archivo.write("Nueva entrada: Usuario registrado\n")
```

---

## 📌 Fase 3: Manejo de Errores  

### **3.1 Uso de `try` y `except`**  
Evita que el programa se detenga por errores:  
```python
try:
    with open("datos.csv", "r") as archivo:
        print(archivo.read())
except FileNotFoundError:
    print("⚠️ Error: El archivo no existe")
except Exception as e:
    print(f"❌ Error inesperado: {e}")
```

### **3.2 Comprobar Existencia**  
Usa `os.path.exists()` para verificar archivos:  
```python
import os

if os.path.exists("config.json"):
    print("✅ Archivo encontrado")
else:
    print("❌ Archivo no existe")
```

---

## 📌 Fase 4: Trabajo con CSV  

### **4.1 Lectura**  
```python
import csv

with open("empleados.csv", "r") as archivo:
    lector = csv.reader(archivo)
    for fila in lector:
        print(f"Nombre: {fila[0]}, Edad: {fila[1]}")
```

### **4.2 Escritura**  
```python
with open("productos.csv", "w", newline="") as archivo:
    escritor = csv.writer(archivo)
    escritor.writerow(["Producto", "Precio"])
    escritor.writerow(["Laptop", 1200])
```

**Ejemplo Práctico**: Calcular edad promedio desde un CSV:  
```python
total_edad = 0
contador = 0

with open("empleados.csv", "r") as archivo:
    lector = csv.reader(archivo)
    next(lector)  # Saltar cabecera
    for fila in lector:
        total_edad += int(fila[1])
        contador += 1

print(f"Edad promedio: {total_edad / contador}")
```

---

## 📌 Fase 5: Trabajo con JSON

### 5.1 Introducción a JSON  
**JSON (JavaScript Object Notation)** es un formato ligero para intercambiar datos. Es ideal para:  
- Almacenar configuraciones  
- Comunicarse con APIs web  
- Guardar datos estructurados  

**Características clave**:  
- Usa pares `clave: valor` (como diccionarios de Python)  
- Soporta listas, números, strings, booleanos y `null`  
- Es legible por humanos y máquinas  

---

### 5.2 Serialización vs. Deserialización  
| Término          | Definición                              | Método JSON       |  
|------------------|-----------------------------------------|-------------------|  
| **Serialización** | Convertir objetos Python → texto JSON   | `json.dump()`     |  
| **Deserialización** | Convertir texto JSON → objetos Python | `json.load()`     |  

---

### 5.3 Ejemplos Prácticos con JSON  

#### Guardar datos (Serialización):  
```python
import json

datos_usuario = {
    "nombre": "Ana",
    "edad": 28,
    "hobbies": ["programar", "leer"],
    "premium": True
}

with open("usuario.json", "w") as archivo:
    json.dump(datos_usuario, archivo, indent=4)  # indent para formato bonito
```

#### Cargar datos (Deserialización):  
```python
with open("usuario.json", "r") as archivo:
    datos = json.load(archivo)
    print(f"{datos['nombre']} tiene {datos['edad']} años")
    # Output: "Ana tiene 28 años"
```

#### Caso Real: Guardar múltiples usuarios  
```python
usuarios = [
    {"id": 1, "nombre": "Carlos", "puntos": 150},
    {"id": 2, "nombre": "María", "puntos": 300}
]

with open("usuarios.json", "w") as archivo:
    json.dump(usuarios, archivo)
```

---

## 📌 Fase 6: Gestión Segura con `with`

### 6.1 ¿Por Qué Usar `with`?  
- **Cierre automático**: El archivo se cierra al salir del bloque, incluso si hay errores  
- **Código más limpio**: Reduce líneas de código  
- **Previene corrupción**: Evita que archivos queden abiertos por accidente  

**Ejemplo Comparativo**:  
```python
# Sin with (puede olvidarse close())
archivo = open("datos.txt", "r")
contenido = archivo.read()
archivo.close()

# Con with (recomendado)
with open("datos.txt", "r") as archivo:
    contenido = archivo.read()
# ¡Archivo cerrado automáticamente!
```

---

## 📌 Fase 7: Organización de Proyectos

### 7.1 Estructura Recomendada  
```
mi_proyecto/  
├── data/                  # Datos (CSV, JSON, etc)  
│   ├── input/             # Datos crudos  
│   └── output/            # Resultados procesados  
├── src/                   # Código fuente  
│   ├── utils/             # Funciones auxiliares  
│   └── main.py            # Script principal  
├── docs/                  # Documentación  
├── config/                # Archivos de configuración  
└── README.md              # Guía del proyecto  
```

### 7.2 Buenas Prácticas  
1. **Separa datos de código**: Nunca mezcles archivos `.py` con `.csv` en la misma carpeta  
2. **Usa rutas relativas**:  
```python
import os

CARPETA_DATA = os.path.join("data", "input")  # ✔️ Mejor que "C:/proyecto/data"
```  
3. **Versiona tus datos**: Usa nombres como `ventas_2023_v1.csv`  
4. **Documenta la estructura**: Explica en el README cómo organizar los archivos  

---

## 🚀 Ejemplo Integrado: Sistema de Usuarios  

```python
import json
import os

# Crear estructura de carpetas
os.makedirs("data/users", exist_ok=True)

def guardar_usuario(usuario):
    ruta = os.path.join("data/users", f"{usuario['id']}.json")
    with open(ruta, "w") as archivo:
        json.dump(usuario, archivo)

def cargar_usuario(user_id):
    ruta = os.path.join("data/users", f"{user_id}.json")
    try:
        with open(ruta, "r") as archivo:
            return json.load(archivo)
    except FileNotFoundError:
        return None

# Uso
nuevo_usuario = {"id": 101, "nombre": "Luisa", "email": "luisa@ejemplo.com"}
guardar_usuario(nuevo_usuario)

usuario = cargar_usuario(101)
print(usuario["nombre"])  # Output: "Luisa"
```

---