# 🐍 Clase 06 - Decoradores

## 🌟 Definición Conceptual Intuitiva

> 💡 Analogía de la vida diaria: Imagina que tienes un regalo (tu función) y quieres envolverlo con papel de regalo (el decorador). El regalo sigue siendo el mismo por dentro, pero ahora tiene una presentación diferente y quizás funcionalidades adicionales como una tarjeta o un moño.
> 

Los decoradores en Python son como **"envolturas"** que modifican o extienden el comportamiento de funciones sin cambiar su código interno. Es como agregar capas de funcionalidad a algo que ya existe.

### Ejemplo cotidiano:

- **Función original**: Preparar café ☕
- **Decorador 1**: Agregar azúcar 🍯
- **Decorador 2**: Servir en taza elegante 🏺
- **Resultado**: El mismo café, pero con mejoras adicionales

---

## 📚 Definición Formal

> 📖 Definición: Un decorador es una función que toma otra función como argumento y devuelve una nueva función que generalmente extiende o modifica el comportamiento de la función original sin alterar explícitamente su código fuente.
> 

### Características principales:

- **Función de orden superior**: Acepta funciones como parámetros
- **Retorna una función**: Devuelve una función modificada o mejorada
- **Preserva la interfaz**: Mantiene la signatura de la función original
- **Sintaxis elegante**: Usa el símbolo `@` para aplicación limpia

## 🔧 Sintaxis general

```python
def decorador(funcion_original):
    def nueva_funcion(*args, **kwargs):
        # Código antes
        resultado = funcion_original(*args, **kwargs)
        # Código después
        return resultado
    return nueva_funcion
```

Uso del decorador:

```python
@decorador
def saludar():
    print("Hola")

saludar()
```

### ✅ ¿Cuál es la función original?

### ✅ ¿Cuál es el decorador?

La función **original** es aquella que será decorada, es decir, la que pasamos como argumento al decorador.

```python
@decorador
def saludar():
    print("Hola")
```

El **decorador** es la función que **recibe otra función como argumento**, la envuelve con lógica adicional, y devuelve una nueva función.

```python
def decorador(funcion_original):
    def nueva_funcion(*args, **kwargs):
        # Código antes
        resultado = funcion_original(*args, **kwargs)
        # Código después
        return resultado
    return nueva_funcion

```

Cuando se escribe:

Es equivalente a:

```python
@decorador
def saludar():
    print('Hola')
```

```python
saludar = decorador(saludar)

```

### Ejemplo simple:

```python
# Esto es una función normal
def imprimirNumeros(n):
    for i in range(n):
        print(i)

imprimirNumeros(10)
```

Supongamos que se quiere medir el tiempo de ejecución de esa función. Sin decoradores se modificaría la función de la siguiente forma:

```python
import time

def imprimirNumeros(n):
    inicio = time.time()
    for i in range(n):
        print(i)
    final = time.time()
    print(f'Tiempo de ejecución: {final - inicio}')

imprimirNumeros(100000)
```

Mediante el uso de decoradores seria de la siguiente forma:

```python
import time
# Definimos el decorador:
def calcularTiempo(funcion):
    def funcionModificada(*args, **kwargs):
        inicio = time.time()
        funcion(*args, **kwargs)
        final = time.time()
        print(f'Tiempo de ejecución: {final - inicio} segundos')
    return funcionModificada

@calcularTiempo
def imprimirNumeros(n):
    for i in range(n):
        print(i)

imprimirNumeros(1000)
```

Salida:

```
1
2
3
.
.
.
997
998
999
Tiempo de ejecución: 0.022598981857299805 segundos
```

Otro ejemplo con sumas y restas:

```python
import time
# Definimos el decorador:
def calcularTiempo(funcion):
    def funcionModificada(*args, **kwargs):
        inicio = time.time()
        funcion(*args, **kwargs)
        final = time.time()
        print(f'Tiempo de ejecución: {final - inicio} segundos')
    return funcionModificada

@calcularTiempo
def suma(a):
    return a + 100
@calcularTiempo
def resta(a):
    return a - 100

suma(10000)
resta(10000)
```

Salida:

```
Tiempo de ejecución: 1.1920928955078125e-06 segundos
Tiempo de ejecución: 1.430511474609375e-06 segundos
```

## 🔧 Tipos de Decoradores

### 1. **Decoradores de Función**

### 2. **Decoradores con Parámetros**

```python
@decorador
def mi_funcion():
    pass

```

```python
@decorador(parametro)
def mi_funcion():
    pass

```

### 3. **Decoradores de Clase**

### 4. **Decoradores Anidados**

```python
@decorador
class MiClase:
    pass

```

```python
@decorador1
@decorador2
def mi_funcion():
    pass
```

---

## 🎯 Ejemplos de tipos de decoradores comunes

### 1. ✅ Decorador simple para funciones

```python
def decorador(func):
    def wrapper():
        print("Antes de ejecutar la función")
        func()
        print("Después de ejecutar la función")
    return wrapper

@decorador
def decir_hola():
    print("Hola mundo")

decir_hola()
```

Output:

```python
Antes de ejecutar la función
Hola mundo
Después de ejecutar la función
```

### 2. 📦 Decorador con argumentos variables

```python
def decorador(func):
    def wrapper(*args, **kwargs):
        print(f"Llamando a {func.__name__} con args={args} kwargs={kwargs}")
        return func(*args, **kwargs)
    return wrapper

@decorador
def sumar(a, b):
    return a + b

resultado = sumar(3, 5)
print(resultado)
```

Output:

```
Llamando a sumar con args=(3, 5) kwargs={}
8
```

Con argumentos de clave valor:

```python
def decorador(func):
    def wrapper(*args, **kwargs):
        print(f"Llamando a {func.__name__} con args={args} kwargs={kwargs}")
        return func(*args, **kwargs)
    return wrapper

@decorador
def sumar(a, b, c=8, d=4):
    return a + b + d + c

resultado = sumar(3, 5, c=6, d=7) 
print(resultado)
```

Output:

```
Llamando a sumar con args=(3, 5) kwargs={'c': 6, 'd': 7}
21
```

### 3. 🛠 Decorador con argumentos propios

```python
def decorador_con_parametros(prefix):
    def decorador(func):
        def wrapper(*args, **kwargs):
            print(f"{prefix} - Ejecutando {func.__name__}")
            return func(*args, **kwargs)
        return wrapper
    return decorador

@decorador_con_parametros("LOG")
def saludar(nombre):
    print(f"Hola, {nombre}")

saludar("Ana")

```

Output:

```
LOG - Ejecutando saludar
Hola, Ana
```

# Linea por linea

### Definición del decorador principal

```python
def decorador_con_parametros(prefix):
```

**Línea 1:** Define una función que acepta un parámetro `prefix`. Esta es la función externa que nos permitirá configurar el decorador.

### Primera función interna

```python
def decorador(func):
```

**Línea 2:** Define la función interna que actuará como el decorador real. Recibe `func` (la función que será decorada).

### Segunda función interna (wrapper)

```python
def wrapper(*args, **kwargs):
```

**Línea 3:** Define la función que reemplazará a la función original. Acepta cualquier cantidad de argumentos posicionales (`*args`) y argumentos con nombre o clave valor (`**kwargs`).

### Lógica del decorador

```python
print(f"{prefix} - Ejecutando {func.__name__}")
```

**Línea 4:** Imprime un mensaje personalizado usando el `prefix` configurado y el nombre de la función original (`func.__name__`).

```python
return func(*args, **kwargs)
```

**Línea 5:** Ejecuta la función original pasándole todos los argumentos recibidos y retorna su resultado.

### Retornos de las funciones anidadas

```python
return wrapper
```

**Línea 6:** La función `decorador` retorna la función `wrapper`.

```python
return decorador
```

**Línea 7:** La función `decorador_con_parametros` retorna la función `decorador`.

### Uso del decorador

```python
@decorador_con_parametros("LOG")
```

**Línea 8:** Aplica el decorador a la función siguiente, configurándolo con el prefijo "LOG".

```python
def saludar(nombre):
```

**Línea 9:** Define una función simple que acepta un parámetro `nombre`.

```python
print(f"Hola, {nombre}")
```

**Línea 10:** Imprime un saludo personalizado.

### Llamada a la función decorada

```python
saludar("Ana")
```

**Línea 11:** Llama a la función decorada.

### Salida esperada:

```python
LOG - Ejecutando saludar
Hola, Ana
```

<aside>
<img src="https://www.notion.so/icons/laptop_green.svg" alt="https://www.notion.so/icons/laptop_green.svg" width="40px" />

**Nota importante:** Este patrón de tres funciones anidadas permite crear decoradores configurables que pueden personalizarse al momento de aplicarlos.

</aside>

## 4. 📚 Decoradores con `functools.wraps`

> Permite mantener el nombre y docstring de la función original tras aplicar el decorador.
> 

```python
from functools import wraps

def decorador(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print("Decorando con wraps")
        return func(*args, **kwargs)
    return wrapper

@decorador
def ejemplo():
    """Esta es la docstring de ejemplo."""
    print("Función original")

print(ejemplo.__name__)      # 'ejemplo'
print(ejemplo.__doc__)       # 'Esta es la docstring de ejemplo.'
```

## ¿Qué es functools.wraps?

`functools.wraps` es una utilidad de Python que resuelve un problema común con los decoradores: **la pérdida de metadatos de la función original**. Cuando creamos un decorador, la función wrapper reemplaza a la función original, pero esto hace que se pierdan propiedades importantes como:

- `__name__` (nombre de la función)
- `__doc__` (docstring):  es una cadena de texto literal que se usa en Python para documentar módulos, clases, métodos y funciones. Es la forma estándar y oficial de documentar código en Python.
- `__module__` (módulo donde se define)
- `__annotations__` (anotaciones de tipo)

**Sin `@wraps`:**

```python
def mi_decorador(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@mi_decorador
def ejemplo():
    """Docstring original"""
    pass

print(ejemplo.__name__)  # Salida: 'wrapper' ❌
print(ejemplo.__doc__)   # Salida: None ❌
```

**Con `@wraps`:**

```python
from functools import wraps
def mi_decorador(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@mi_decorador
def ejemplo():
    """Docstring original"""
    pass

print(ejemplo.__name__)  # Salida: 'ejemplo' ✅
print(ejemplo.__doc__)   # Salida: 'Docstring original' ✅
```

## Explicación línea por línea

### Importación

```python
from functools import wraps
```

**Línea 1:** Importa la función `wraps` del módulo `functools`.

### Definición del decorador

```python
def decorador(func):
```

**Línea 2:** Define la función decorador que recibe la función a decorar como parámetro.

### Aplicación de @wraps

```python
@wraps(func)

```

**Línea 3:** Aplica el decorador `@wraps(func)` a la función wrapper. Esto copia los metadatos de `func` (la función original) a `wrapper`.

### Función wrapper

```python
def wrapper(*args, **kwargs):

```

**Línea 4:** Define la función wrapper que reemplazará a la función original. Acepta cualquier combinación de argumentos.

### Lógica del decorador

```python
print("Decorando con wraps")
```

**Línea 5:** Imprime un mensaje indicando que el decorador está funcionando.

```python
return func(*args, **kwargs)
```

**Línea 6:** Ejecuta la función original con todos sus argumentos y retorna el resultado.

### Retorno del wrapper

```python
return wrapper

```

**Línea 7:** La función decorador retorna la función wrapper (que ahora mantiene los metadatos originales).

### Aplicación del decorador

```python
@decorador
```

**Línea 8:** Aplica el decorador a la función siguiente.

### Función de ejemplo

```python
def ejemplo():
```

**Línea 9:** Define una función simple llamada `ejemplo`.

```python
"""Esta es la docstring de ejemplo."""
```

**Línea 10:** Docstring de la función ejemplo.

```python
print("Función original")
```

**Línea 11:** Código que ejecuta la función ejemplo.

### Verificación de metadatos

```python
print(ejemplo.__name__)# 'ejemplo'
```

**Línea 12:** Imprime el nombre de la función. Gracias a `@wraps`, mantiene el nombre original "ejemplo".

```python
print(ejemplo.__doc__)# 'Esta es la docstring de ejemplo.'
```

**Línea 13:** Imprime el docstring de la función. Gracias a `@wraps`, mantiene el docstring original.

---

## Salida esperada:

```
Decorando con wraps
Función original
ejemplo
Esta es la docstring de ejemplo.
```

### 5. 🧱 Decoradores para métodos (clases) → Lo veremos en OOP

```python
def decorador(func):
    def wrapper(self, *args, **kwargs):
        print(f"Llamando al método {func.__name__}")
        return func(self, *args, **kwargs)
    return wrapper

class Persona:
    def __init__(self, nombre):
        self.nombre = nombre

    @decorador
    def saludar(self):
        print(f"Hola, soy {self.nombre}")

p = Persona("Lucía")
p.saludar()
```

### 🧪 Decoradores integrados en Python → Lo veremos en OOP

| Decorador | Descripción |
| --- | --- |
| `@staticmethod` | Define un método estático en una clase |
| `@classmethod` | Define un método de clase que recibe `cls` en lugar de `self` |
| `@property` | Convierte un método en una propiedad de solo lectura |