# 🐍 Clase 08 - Decoradores Personalizados

# Decoradores Personalizados para Métodos

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

<aside>

El código define un **decorador** llamado `log_llamada` que registra todas las llamadas a los métodos de la clase `CuentaBancaria`, mostrando:

- Fecha y hora de la llamada
- El método que se ejecuta
- Los argumentos con los que fue llamado
- El valor retornado por el método

Luego aplica ese decorador a los métodos `depositar` y `retirar`.

🔍 1. Decorador `log_llamada`

1. **Recibe un método** (por ejemplo `depositar` o `retirar`).
2. Define una función interna `wrapper` que **envuelve** al método original.
3. Obtiene un timestamp con la hora actual.
4. Imprime un mensaje indicando que el método fue llamado y sus argumentos.
5. Ejecuta el método real.
6. Imprime el resultado que retornó.
7. Devuelve el resultado para que el funcionamiento del método no cambie.

💡 **Importante:**

`@functools.wraps(metodo)` sirve para que el método resultante no pierda su nombre, docstring, etc.
🏦 2. Clase `CuentaBancaria`

Inicializa los atributos:

- `titular`: nombre del dueño de la cuenta
- `saldo`: balance inicial (por defecto 0)

💰 3. Método `depositar`, decorado con `@log_llamada`

- El decorador imprime que el método fue llamado.
- Se suma el monto al saldo.
- Se imprime el valor retornado.
- El nuevo saldo se devuelve.

🏧 4. Método `retirar`, también decorado

- El decorador registra la llamada.
- Verifica si hay suficiente saldo.
- Si no lo hay → lanza `ValueError`.
- Si todo va bien → resta el monto y devuelve el nuevo saldo.
- El decorador imprime el resultado.
</aside>

<aside>

# 📝 **Ejercicio 1 — Registrar acciones en una Biblioteca**

Crea una clase llamada **Biblioteca** con los siguientes métodos:

- `agregar_libro(titulo)`
- `prestar_libro(titulo)`
- `devolver_libro(titulo)`

Implementa un decorador llamado **`registrar_evento`**, que:

1. Muestre la fecha y hora de la operación
2. Muestre el método llamado y sus argumentos
3. Muestre el estado final de la operación (por ejemplo, `"Libro agregado"`, `"Libro prestado"`, etc.)

### La salida esperada al ejecutar las operaciones:

```python
[2025-01-10 12:15:22] Llamando agregar_libro con args=('Harry Potter',)
[2025-01-10 12:15:22] agregar_libro completado → Total libros: 1

[2025-01-10 12:15:25] Llamando prestar_libro con args=('Harry Potter',)
[2025-01-10 12:15:25] prestar_libro completado → Libros disponibles: 0

```

</aside>

<aside>

# 📝 **Ejercicio 2 — Decorador para validar acceso a métodos privados**

Crea una clase llamada **Servidor** con:

- Un atributo `usuarios_autorizados` (lista de nombres)
- Un método `acceder_recurso(usuario)`
- Un método `eliminar_recurso(usuario)`

Implementa un decorador llamado **`verificar_permiso`**, que:

1. Revise si el usuario está en la lista `usuarios_autorizados`
2. Si **no** está autorizado → Imprima un mensaje de intento fallido
3. Si está autorizado → Ejecute el método normalmente y registre la acción

### Salida esperada al probar:

```python
Intento de acceso por usuario NO autorizado: 'Carlos'
ACCESO DENEGADO

Intento de acceso por 'Ana' → acceso concedido
Recurso accedido correctamente por Ana

```

</aside>

## Decorador de validación de tipos

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

<aside>

1. Importación
Se importa el módulo `functools` para poder usar `functools.wraps`, que sirve para que el decorador **no pierda el nombre y la docstring** del método original.
2. Decorador con parámetros `validar_tipos`

Aquí hay **tres niveles**:

### 2.1. `validar_tipos(*tipos_esperados)`

Es una **fábrica de decoradores**:

- Recibe una lista de tipos esperados (por ejemplo `float`, `int`).
- No decora aún nada; solo **guarda esos tipos** y devuelve otro decorador (`decorador`).

Ejemplo de uso:

```python
@validar_tipos(float, int)
def mi_funcion(x, y):
    ...
```

En ese momento `tipos_esperados = (float, int)`.

### 2.2. `decorador(metodo)`

Esta función recibe el **método/función** que se va a decorar.

- `metodo` será, en este caso, `aplicar_descuento` de la clase `Producto`.
- Dentro define `wrapper`, que es lo que realmente se ejecutará en lugar del método original.

### 2.3. `wrapper(self, *args, **kwargs)`

Aquí está la lógica real del decorador:

```python
def wrapper(self, *args, **kwargs):
    for arg, tipo in zip(args, tipos_esperados):
        if not isinstance(arg, tipo):
            raise TypeError(
                f"Se esperaba {tipo.__name__}, "
                f"se recibió {type(arg).__name__}"
            )
    return metodo(self, *args, **kwargs)
```

- `self` es la instancia de la clase (`Producto`), **no se valida**.
- `args` contiene los argumentos posicionales del método **después de `self`**.
    
    En `aplicar_descuento(self, porcentaje, cantidad)`:
    
    - `args[0]` → `porcentaje`
    - `args[1]` → `cantidad`
- `zip(args, tipos_esperados)` empareja:
    - `porcentaje` con `float`
    - `cantidad` con `int`

Para cada par:

1. Comprueba `isinstance(arg, tipo)`.
2. Si el tipo no coincide, lanza un `TypeError` con un mensaje explicativo:
    - `"Se esperaba float, se recibió str"`, por ejemplo.

Si todos los tipos son correctos → se ejecuta el método original:

```python
return metodo(self, *args, **kwargs)
```

3. Clase `Producto`

```python
class Producto:
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio

```

- `nombre`: nombre del producto (por ejemplo `"Laptop"`).
- `precio`: precio base (por ejemplo `1000`).

4. Método `aplicar_descuento` decorado

```python
@validar_tipos(float, int)
def aplicar_descuento(self, porcentaje, cantidad):
    """Aplica descuento validando tipos."""
    descuento = self.precio * (porcentaje / 100)
    return (self.precio - descuento) * cantidad

```

- El decorador `@validar_tipos(float, int)` indica:
    - `porcentaje` debe ser `float`
    - `cantidad` debe ser `int`

### ¿Qué hace el método?

1. Calcula el descuento:
    
    `descuento = self.precio * (porcentaje / 100)`
    
2. Resta el descuento al precio: `self.precio - descuento`
3. Multiplica por `cantidad`, para un total acumulado.

---

5. Uso del código

```python
# Uso
prod = Producto("Laptop", 1000)
print(prod.aplicar_descuento(10.0, 2))   # 1800.0

# prod.aplicar_descuento("10", 2)  # ¡TypeError!

```

</aside>

<aside>

# 📝 **Ejercicio 3 — Sistema de Reservas de Hotel con validación de tipos**

Crea una clase llamada **ReservaHotel** que tenga:

- Atributos: `cliente`, `noches`, `precio_por_noche`
- Un método `calcular_total(descuento, impuesto)` que calcule:

```python
total = (precio_por_noche * noches) - descuento
total_final = total + impuesto
```

Implementa un decorador llamado **`validar_argumentos`**, que valide los tipos de los parámetros del método:

- `descuento` debe ser `float`
- `impuesto` debe ser `float`

El decorador debe lanzar un `TypeError` si algún tipo no coincide.

### Ejemplo de ejecución (salida esperada):

```python
Total antes de impuestos: 300.0
Total final: 330.0
```

Y si el usuario ejecuta:

```python
reserva.calcular_total("10", 5.0)
```

Debe producir:

```python
TypeError: Se esperaba float, se recibió str
```

</aside>

<aside>

# 📝 **Ejercicio 4 — Sistema de Pedidos en Restaurante con decorador validador**

Crea una clase llamada **PedidoRestaurante** con:

- Atributos: `plato`, `precio_unitario`
- Un método `total_pedido(cantidad, propina)` que calcule:
    
    ```
    subtotal = precio_unitario * cantidad
    total = subtotal + propina
    
    ```
    

Implementa un decorador llamado **`validar_tipos_pedido`**, que valide:

- `cantidad` debe ser `int`
- `propina` debe ser `float`

Si algún tipo no coincide, el decorador debe lanzar un `TypeError`.

### Salida esperada cuando los tipos son correctos:

```python
Subtotal: 45
Total con propina: 50.0
```

Ejecución incorrecta:

```python
pedido.total_pedido(3, "5.0")

```

Debe mostrar:

```python
TypeError: Se esperaba float, se recibió str
```

</aside>

# Decoradores que Modifican Clases Completas

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

### Decorador `Singleton`

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

### Decorador para registrar clases

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

### Decoradores Integrados Adicionales

`@dataclass`

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