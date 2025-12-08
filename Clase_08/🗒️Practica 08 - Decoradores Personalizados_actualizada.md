# 🗒️Practica 08 - Decoradores Personalizados

<aside>

### Ejercicios del 1 al 5 propuestos de la clase 8

</aside>

## Partiendo del ejemplo explicado en clase:

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
		    """ Este metodo aumenta el saldo de la cuenta"""
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

## Ejercicio 1: Modificar el código anterior de forma tal que los “logs” se guarden en un archivo y se incluya la fecha de alta de una cuenta bancaria.

<aside>

# 📝 **Ejercicio 2 — Registrar acciones en una Biblioteca**

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

# 📝 **Ejercicio 3 — Decorador para validar acceso a métodos privados**

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

<aside>

# 📝 **Ejercicio 4 — Sistema de Reservas de Hotel con validación de tipos**

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

# 📝 **Ejercicio 5 — Sistema de Pedidos en Restaurante con decorador validador**

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