# 🐍 Clase 07 - Decoradores Parte 2

# Docstrings `__doc__`

### 📌 Función con docstring

### 📌  **Función sin docstring**

```python
def pares():
    """Esta función regresa números pares"""
    pass

print(pares.__doc__)
```

```python
# Salida
Esta función regresa números pares
```

```python
def pares():
    pass

print(pares.__doc__)
```

```python
None
```

### 📌 Clase con docstring

```python
class numeros:
    """Esta función hace cálculos"""
    
    def operaciones(self):
        pass

print(numeros.__doc__)
```

```python
Esta función hace cálculos
```

### Algunos cambios de los decoradores que no se ven

### Decorador con wraps:

```python

def eldecorador(func):
    def wrapper(*args, **kwargs):
        """La función wrapper"""
        func()
    return wrapper
    
@eldecorador
def funcionuno():
    """Función uno: presente"""
    print("Esta es la primera función")
    
@eldecorador
def funciondos(a):
    """Función dos: presente"""
    print("Esta es la segunda función")

# Impresión de metadatos

print(funcionuno.__name__)
print(funcionuno.__doc__)
print(funciondos.__name__)
print(funciondos.__doc__)

```

```python
from functools import wraps

def eldecorador(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        """La función wrapper"""
        func()
    return wrapper

@eldecorador
def funcionuno():
    """Función uno: presente"""
    print("Esta es la primera función")

@eldecorador
def funciondos(a):
    """Función dos: presente"""
    print("Esta es la segunda función")
    
# Impresión de metadatos

print(funcionuno.__name__)
print(funcionuno.__doc__)
print(funciondos.__name__)
print(funciondos.__doc__)
```

```python
wrapper
La función wrapper
wrapper
La función wrapper
```

```python
funcionuno
Función uno: presente
funciondos
Función dos: presente
```

# 1. 📚 Decoradores con `functools.wraps`

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

<aside>

## ¿Qué es functools.wraps?

`functools.wraps` es una utilidad de Python que resuelve un problema común con los decoradores: **la pérdida de metadatos de la función original**. Cuando creamos un decorador, la función wrapper reemplaza a la función original, pero esto hace que se pierdan propiedades importantes como:

- `__name__` (nombre de la función)
- `__doc__` (docstring):  es una cadena de texto literal que se usa en Python para documentar módulos, clases, métodos y funciones. Es la forma estándar y oficial de documentar código en Python.
- `__module__` (módulo donde se define)
- `__annotations__` (anotaciones de tipo)
    
    ![image.png](image.png)
    
</aside>

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

### Otro ejemplo

```python
def decorador(func):

    def interna(x, y):
        """Función Wrapper"""
        print("¡Aquí decoramos!")
        x = x*3
        y = y*3
        return (func(x, y))
    return interna

@decorador
def funcion(x, y):
    """Doble de los números"""
    x = 2*x
    y = 2*y
    return (x, y)

print(funcion.__doc__)
print(funcion.__name__)

```

```python
Función Wrapper
interna
```

```python
from functools import wraps

def decorador(func):
    @wraps(func)
    def interna(x, y):
        """Función Wrapper"""
        print("¡Aquí decoramos!")
        x = x*3
        y = y*3
        return (func(x, y))
    return interna

@decorador
def funcion(x, y):
    """Doble de los números"""
    x = 2*x
    y = 2*y
    return (x, y)

print(funcion.__doc__)
print(funcion.__name__)
```

```python
Doble de los números
funcion
```

## Ejercicios de refuerzo:

### Ejercicio 7.1 — Decorador sin `wraps`

Crea un decorador llamado `registrar_llamada` que:

- Imprima el mensaje:
    
    > Llamando a la función...
    > 
    > 
    > antes de ejecutar la función decorada.
    > 

Aplícalo a dos funciones:

1. `saludar()`
    - Debe imprimir un saludo por pantalla.
    - Debe tener una docstring que describa que imprime un saludo.
2. `despedir(nombre)`
    - Debe imprimir una despedida personalizada usando el nombre recibido.
    - Debe tener una docstring que describa que imprime una despedida personalizada.

Después de definir y decorar las funciones:

1. Llama a `saludar()` y luego a `despedir("Ana")`.
2. Imprime los siguientes atributos de metadatos:
    - `saludar.__name__`
    - `saludar.__doc__`
    - `despedir.__name__`
    - `despedir.__doc__`

No uses `functools.wraps` en este ejercicio.

Salida esperada:

```python
Llamando a la función...
Hola a todos
Llamando a la función...
Adiós Ana
wrapper
None
wrapper
None

```

### Ejercicio 7.2 — Decorador con `wraps`

Toma el ejercicio anterior y mantén la misma idea:

- Un decorador `registrar_llamada` que:
    - Imprima `Llamando a la función...` antes de ejecutar la función original.
- Dos funciones decoradas:
    1. `saludar()` con una docstring que explique que imprime un saludo.
    2. `despedir(nombre)` con una docstring que explique que imprime una despedida personalizada.
- Llamadas a:
    - `saludar()`
    - `despedir("Ana")`
- Impresión de los atributos:
    - `saludar.__name__`
    - `saludar.__doc__`
    - `despedir.__name__`
    - `despedir.__doc__`

Pero en este ejercicio **sí debes usar** `from functools import wraps` y aplicar `@wraps` en el decorador para conservar los metadatos de las funciones originales.

Salida esperada:

```python
Llamando a la función...
Hola a todos
Llamando a la función...
Adiós Ana
saludar
Función que imprime un saludo
despedir
Función que imprime una despedida personalizada
```

# 2. 🧱 Decoradores para métodos (clases)

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

### 🧪 Decoradores integrados en Python

| Decorador | Descripción |
| --- | --- |
| `@staticmethod` | Define un método estático en una clase |
| `@classmethod` | Define un método de clase que recibe `cls` en lugar de `self` |
| `@property` | Convierte un método en una propiedad de solo lectura |

### El decorador `@property`

Permite acceder a un método como si fuera un atributo, encapsulando la lógica de acceso.

```python
class Rectangulo:
    def __init__(self, base, altura):
        self._base = base
        self._altura = altura
    
    @property
    def area(self):
        """Calcula el área del rectángulo."""
        return self._base * self._altura
    
    @property
    def base(self):
        """Getter para base."""
        return self._base
    
    @base.setter
    def base(self, valor):
        """Setter para base con validación."""
        if valor <= 0:
            raise ValueError("La base debe ser positiva")
        self._base = valor
    
    @base.deleter
    def base(self):
        """Deleter para base."""
        print("Eliminando base...")
        del self._base

# Uso
rect = Rectangulo(5, 3)
print(rect.area)      # 15 - Se accede sin paréntesis
print(rect.base)      # 5

rect.base = 10        # Usa el setter
print(rect.area)      # 30

# rect.base = -5      # ¡Lanzaría ValueError!
```

```python
15
5
30
```

### El decorador `@classmethod`

Define métodos que reciben la clase como primer argumento (`cls`) en lugar de la instancia (`self`).

```python
class Empleado:
    aumento_anual = 1.05  # Atributo de clase
    total_empleados = 0
    
    def __init__(self, nombre, salario):
        self.nombre = nombre
        self.salario = salario
        Empleado.total_empleados += 1
    
    def aplicar_aumento(self):
        """Método de instancia: usa self."""
        self.salario *= self.aumento_anual
    
    @classmethod
    def establecer_aumento(cls, porcentaje):
        """Método de clase: modifica atributo de clase."""
        cls.aumento_anual = 1 + porcentaje / 100
    
    @classmethod
    def desde_string(cls, cadena_empleado):
        """Constructor alternativo usando método de clase."""
        nombre, salario = cadena_empleado.split("-")
        return cls(nombre, float(salario))
    
    @classmethod
    def obtener_total(cls):
        """Accede a atributos de clase."""
        return cls.total_empleados

# Uso de métodos de clase
Empleado.establecer_aumento(10)  # Cambia para TODOS
print(Empleado.aumento_anual)    # 1.10

# Constructor alternativo
emp1 = Empleado.desde_string("María-50000")
emp2 = Empleado.desde_string("Carlos-45000")

print(emp1.nombre, emp1.salario)     # María 50000.0
print(Empleado.obtener_total())      # 2
```

```python
1.1
María 50000.0
2
```

### El decorador `@staticmethod`

Define métodos que no reciben ni la instancia ni la clase automáticamente. Son funciones regulares dentro del namespace de la clase.

```python
class Calculadora:
    
    @staticmethod
    def sumar(a, b):
        """No necesita self ni cls."""
        return a + b
    
    @staticmethod
    def es_par(numero):
        """Utilidad relacionada con la clase."""
        return numero % 2 == 0
    
    @staticmethod
    def validar_operandos(*args):
        """Valida que todos sean números."""
        return all(isinstance(x, (int, float)) for x in args)

# Se puede llamar desde la clase o desde instancias
print(Calculadora.sumar(5, 3))           # 8
print(Calculadora.es_par(10))            # True

calc = Calculadora()
print(calc.sumar(2, 2))                  # 4 - También funciona
```

 Comparativa: Cuándo Usar Cada Uno

```python
class Demostrador:
    atributo_clase = "Compartido"
    
    def __init__(self, valor):
        self.atributo_instancia = valor
    
    def metodo_instancia(self):
        # ✅ Accede a self (instancia)
        # ✅ Accede a atributos de clase vía self.__class__
        return f"Instancia: {self.atributo_instancia}"
    
    @classmethod
    def metodo_clase(cls):
        # ❌ NO tiene acceso a self
        # ✅ Accede a cls (la clase)
        return f"Clase: {cls.atributo_clase}"
    
    @staticmethod
    def metodo_estatico():
        # ❌ NO tiene acceso a self
        # ❌ NO tiene acceso a cls
        return "Sin acceso automático a nada"
```

| Característica | Método Normal | @classmethod | @staticmethod |
| --- | --- | --- | --- |
| Primer parámetro | `self` | `cls` | Ninguno |
| Acceso a instancia | ✅ Sí | ❌ No | ❌ No |
| Acceso a clase | ✅ Vía self | ✅ Directo | ❌ No |
| Puede modificar instancia | ✅ Sí | ❌ No | ❌ No |
| Puede modificar clase | ✅ Sí | ✅ Sí | ❌ No |

## Decoradores Personalizados para Métodos

Podemos crear nuestros propios decoradores para añadir funcionalidad a los métodos de clase.

 Decorador de logging

```python
import functools
from datetime import datetime

def log_llamada(metodo):
    """Registra cada llamada a un método."""
    @functools.wraps(metodo)
    def wrapper(self, *args, **kwargs):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        print(f"[{timestamp}] Llamando {metodo.__name__} con args={args}")
        resultado = metodo(self, *args, **kwargs)
        print(f"[{timestamp}] {metodo.__name__} retornó {resultado}")
        return resultado
    return wrapper

class CuentaBancaria:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo
    
    @log_llamada
    def depositar(self, monto):
        self.saldo += monto
        return self.saldo
    
    @log_llamada
    def retirar(self, monto):
        if monto > self.saldo:
            raise ValueError("Fondos insuficientes")
        self.saldo -= monto
        return self.saldo

# Uso
cuenta = CuentaBancaria("Ana", 1000)
cuenta.depositar(500)
cuenta.retirar(200)
```

```python
[2025-12-01 21:34:10] Llamando depositar con args=(500,)
[2025-12-01 21:34:10] depositar retornó 1500
[2025-12-01 21:34:10] Llamando retirar con args=(200,)
[2025-12-01 21:34:10] retirar retornó 1300
1300
```

Decorador de validación de tipos

```python
import functools

def validar_tipos(*tipos_esperados):
    """Decorador con parámetros para validar tipos de argumentos."""
    def decorador(metodo):
        @functools.wraps(metodo)
        def wrapper(self, *args, **kwargs):
            for arg, tipo in zip(args, tipos_esperados):
                if not isinstance(arg, tipo):
                    raise TypeError(
                        f"Se esperaba {tipo.__name__}, "
                        f"se recibió {type(arg).__name__}"
                    )
            return metodo(self, *args, **kwargs)
        return wrapper
    return decorador

class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio
    
    @validar_tipos(float, int)
    def aplicar_descuento(self, porcentaje, cantidad):
        """Aplica descuento validando tipos."""
        descuento = self.precio * (porcentaje / 100)
        return (self.precio - descuento) * cantidad

# Uso
prod = Producto("Laptop", 1000)
print(prod.aplicar_descuento(10.0, 2))    # 1800.0

# prod.aplicar_descuento("10", 2)  # ¡TypeError!
```

```python
1800.0
```

## Decoradores que Modifican Clases Completas

Los decoradores también pueden aplicarse a clases enteras para modificar su estructura o comportamiento.

 Añadir métodos automáticamente

```python
def agregar_representacion(cls):
    """Añade __repr__ automáticamente basado en __init__."""
    
    def __repr__(self):
        atributos = ", ".join(
            f"{k}={v!r}" for k, v in self.__dict__.items()
        )
        return f"{cls.__name__}({atributos})"
    
    cls.__repr__ = __repr__
    return cls

@agregar_representacion
class Persona:
    def __init__(self, nombre, edad):
        self.nombre = nombre
        self.edad = edad

p = Persona("Carlos", 30)
print(p)  # Persona(nombre='Carlos', edad=30)
```

```python
Persona(nombre='Carlos', edad=30)
```

 Decorador Singleton

```python
def singleton(cls):
    """Garantiza que solo exista una instancia de la clase."""
    instancias = {}
    
    def obtener_instancia(*args, **kwargs):
        if cls not in instancias:
            instancias[cls] = cls(*args, **kwargs)
        return instancias[cls]
    
    return obtener_instancia

@singleton
class ConfiguracionApp:
    def __init__(self):
        self.debug = False
        self.version = "1.0.0"
        print("Configuración inicializada")

# Uso
config1 = ConfiguracionApp()  # "Configuración inicializada"
config2 = ConfiguracionApp()  # No imprime nada (misma instancia)

print(config1 is config2)  # True
config1.debug = True
print(config2.debug)       # True - Es la misma instancia
```

```python
Configuración inicializada
True
True
```

Decorador para registrar clases

```python
registro_clases = {}

def registrar(nombre_registro):
    """Registra clases en un diccionario global."""
    def decorador(cls):
        registro_clases[nombre_registro] = cls
        return cls
    return decorador

@registrar("usuario")
class Usuario:
    def __init__(self, nombre):
        self.nombre = nombre

@registrar("admin")
class Administrador:
    def __init__(self, nombre, permisos):
        self.nombre = nombre
        self.permisos = permisos

# Factory basado en registro
def crear_entidad(tipo, *args, **kwargs):
    if tipo not in registro_clases:
        raise ValueError(f"Tipo '{tipo}' no registrado")
    return registro_clases[tipo](*args, **kwargs)

# Uso
usuario = crear_entidad("usuario", "Ana")
admin = crear_entidad("admin", "Carlos", ["leer", "escribir"])

print(registro_clases)
# {'usuario': <class 'Usuario'>, 'admin': <class 'Administrador'>}
```

```python
{'usuario': <class '__main__.Usuario'>, 'admin': <class '__main__.Administrador'>}
```

Decoradores Integrados Adicionales

@dataclass

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Estudiante:
    nombre: str
    edad: int
    cursos: List[str] = field(default_factory=list)
    activo: bool = True
    
    # Los métodos __init__, __repr__, __eq__ se generan automáticamente

est1 = Estudiante("María", 22, ["Python", "SQL"])
est2 = Estudiante("María", 22, ["Python", "SQL"])

print(est1)           # Estudiante(nombre='María', edad=22, ...)
print(est1 == est2)   # True - Comparación automática
```

```python
Estudiante(nombre='María', edad=22, cursos=['Python', 'SQL'], activo=True)
True
```

`@abstractmethod` para clases abstractas

```python
from abc import ABC, abstractmethod

class FiguraGeometrica(ABC):
    
    @abstractmethod
    def area(self):
        """Debe ser implementado por subclases."""
        pass
    
    @abstractmethod
    def perimetro(self):
        """Debe ser implementado por subclases."""
        pass
    
    def descripcion(self):
        """Método concreto heredable."""
        return f"Área: {self.area()}, Perímetro: {self.perimetro()}"

class Cuadrado(FiguraGeometrica):
    def __init__(self, lado):
        self.lado = lado
    
    def area(self):
        return self.lado ** 2
    
    def perimetro(self):
        return 4 * self.lado

# figura = FiguraGeometrica()  # ¡Error! No se puede instanciar
cuadrado = Cuadrado(5)
print(cuadrado.descripcion())  # Área: 25, Perímetro: 20
```

```python
Área: 25, Perímetro: 20
```