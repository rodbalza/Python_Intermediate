# 🗒️Práctica 06 - Decoradores

# Parte 1. Ejercicios resueltos

> **Instrucciones**: A continuación se presentan una serie de ejercicios resueltos en los que debes ejecutar cada código y verificar que la salida sea correcta. Cambia un poco los valores de los parámetros y vuelve a ejecutar el código.  Además debes explicar paso a paso que es lo hace cada código.
> 

## Decorador de Tiempo

```python
import time
import functools

def medir_tiempo(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fin = time.time()
        print(f"{func.__name__} tardó {fin - inicio:.4f} segundos")
        return resultado
    return wrapper

@medir_tiempo
def operacion_lenta():
    time.sleep(1)
    return "Operación completada"

resultado = operacion_lenta()
print(resultado)

```

**Salida:**

```
operacion_lenta tardó 1.0045 segundos
Operación completada

```

### Decorador de Logging

```python
import functools
from datetime import datetime

def log_llamadas(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        print(f"[{timestamp}] Llamando a {func.__name__} con args={args}, kwargs={kwargs}")
        resultado = func(*args, **kwargs)
        print(f"[{timestamp}] {func.__name__} retornó: {resultado}")
        return resultado
    return wrapper

@log_llamadas
def sumar(a, b):
    return a + b

resultado = sumar(5, 3)

```

**Salida:**

```
[2025-06-06 10:30:15] Llamando a sumar con args=(5, 3), kwargs={}
[2025-06-06 10:30:15] sumar retornó: 8

```

### Decorador de Repetición

```python
import functools

def repetir(veces):
    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i in range(veces):
                resultado = func(*args, **kwargs)
                if i < veces - 1:  # No imprimir en la última iteración
                    print(f"Ejecución {i + 1}: {resultado}")
            return resultado
        return wrapper
    return decorador

@repetir(3)
def saludar(nombre):
    return f"¡Hola, {nombre}!"

resultado_final = saludar("Ana")
print(f"Resultado final: {resultado_final}")

```

**Salida:**

```
Ejecución 1: ¡Hola, Ana!
Ejecución 2: ¡Hola, Ana!
Resultado final: ¡Hola, Ana!

```

### Decorador de Validación de Tipos

```python
import functools

def validar_tipos(*tipos_esperados):
    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i, (arg, tipo_esperado) in enumerate(zip(args, tipos_esperados)):
                if not isinstance(arg, tipo_esperado):
                    raise TypeError(f"Argumento {i+1} debe ser de tipo {tipo_esperado.__name__}, recibido {type(arg).__name__}")
            return func(*args, **kwargs)
        return wrapper
    return decorador

@validar_tipos(int, int)
def multiplicar(a, b):
    return a * b

resultado = multiplicar(4, 5)
print(resultado)

# Esto lanzará un error:
# multiplicar("4", 5)

```

**Salida:**

```
20

```

---

### Decorador de Caché

```python
def cache_simple(func):
    """Implementa un cache simple para funciones"""
    cache = {}

    @functools.wraps(func)
    def wrapper(*args):
        if args in cache:
            print(f"Cache hit para {args}")
            return cache[args]
        print(f"Calculando para {args}")
        resultado = func(*args)
        cache[args] = resultado
        return resultado
    return wrapper

@cache_simple
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Prueba
print(fibonacci(10))
print(fibonacci(10))  # Segunda llamada debería usar cache

```

**Salida:**

```
Calculando para (10,)
Calculando para (9,)
Calculando para (8,)
...
55
Cache hit para (10,)
55

```

### Decorador de Retry

```python
import random
import time

def retry(max_intentos=3, delay=1):
    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for intento in range(max_intentos):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if intento == max_intentos - 1:
                        print(f"Falló después de {max_intentos} intentos")
                        raise e
                    print(f"Intento {intento + 1} falló: {e}. Reintentando en {delay}s...")
                    time.sleep(delay)
        return wrapper
    return decorador

@retry(max_intentos=3, delay=0.5)
def operacion_inestable():
    if random.random() < 0.7:  # 70% de probabilidad de fallo
        raise Exception("Operación falló")
    return "¡Éxito!"

resultado = operacion_inestable()
print(resultado)

```

### Decorador de Rate Limiting

```python
import time
from collections import defaultdict

def rate_limit(max_llamadas, ventana_tiempo):
    llamadas = defaultdict(list)

    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            ahora = time.time()
            clave = func.__name__

            # Limpiar llamadas antiguas
            llamadas[clave] = [t for t in llamadas[clave] if ahora - t < ventana_tiempo]

            if len(llamadas[clave]) >= max_llamadas:
                tiempo_espera = ventana_tiempo - (ahora - llamadas[clave][0])
                raise Exception(f"Rate limit excedido. Espere {tiempo_espera:.2f} segundos")

            llamadas[clave].append(ahora)
            return func(*args, **kwargs)
        return wrapper
    return decorador

@rate_limit(max_llamadas=3, ventana_tiempo=10)
def api_call():
    return "Datos de la API"

# Prueba (ejecutar varias veces rápidamente)
for i in range(5):
    try:
        print(f"Llamada {i+1}: {api_call()}")
    except Exception as e:
        print(f"Error en llamada {i+1}: {e}")

```

### Decorador de Autenticación

```python
usuarios_autenticados = {"admin": True, "user1": True, "guest": False}

def requiere_auth(rol_minimo="user"):
    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            # Simulamos obtener el usuario actual
            usuario_actual = kwargs.get('usuario', 'guest')

            if not usuarios_autenticados.get(usuario_actual, False):
                raise PermissionError(f"Usuario '{usuario_actual}' no autenticado")

            if rol_minimo == "admin" and usuario_actual != "admin":
                raise PermissionError(f"Se requiere rol de admin")

            return func(*args, **kwargs)
        return wrapper
    return decorador

@requiere_auth(rol_minimo="admin")
def eliminar_usuario(nombre_usuario, usuario=None):
    return f"Usuario {nombre_usuario} eliminado por {usuario}"

@requiere_auth()
def ver_perfil(usuario=None):
    return f"Perfil de {usuario}"

# Pruebas
print(ver_perfil(usuario="user1"))
print(eliminar_usuario("test_user", usuario="admin"))

```

**Salida:**

```
Perfil de user1
Usuario test_user eliminado por admin

```

### Decorador de Conversión de Unidades

```python
def convertir_unidades(unidad_entrada, unidad_salida):
    conversiones = {
        ('celsius', 'fahrenheit'): lambda x: x * 9/5 + 32,
        ('fahrenheit', 'celsius'): lambda x: (x - 32) * 5/9,
        ('metros', 'pies'): lambda x: x * 3.28084,
        ('pies', 'metros'): lambda x: x / 3.28084,
        ('kg', 'libras'): lambda x: x * 2.20462,
        ('libras', 'kg'): lambda x: x / 2.20462
    }

    def decorador(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            resultado = func(*args, **kwargs)
            conversion = conversiones.get((unidad_entrada, unidad_salida))
            if conversion:
                resultado_convertido = conversion(resultado)
                print(f"Convertido: {resultado} {unidad_entrada} = {resultado_convertido:.2f} {unidad_salida}")
                return resultado_convertido
            return resultado
        return wrapper
    return decorador

@convertir_unidades('celsius', 'fahrenheit')
def temperatura_agua_hirviendo():
    return 100

@convertir_unidades('metros', 'pies')
def altura_edificio():
    return 50

print(f"Agua hierve a: {temperatura_agua_hirviendo()}°F")
print(f"Altura del edificio: {altura_edificio()} pies")

```

**Salida:**

```
Convertido: 100 celsius = 212.00 fahrenheit
Agua hierve a: 212.0°F
Convertido: 50 metros = 164.04 pies
Altura del edificio: 164.04 pies

```

---

# Parte 2. Caso de uso empresarial

> Instrucciones: Ejecuta cada código y corrige los posibles errores que tenga. Verifica las salidas y cambia los parámetros de cada función o decorador y vuelve a ejecutar.
> 

## 📋 Contexto del Problema

**Empresa:** TechCommerce - Plataforma de comercio electrónico

**Problema:** Necesitan un sistema robusto para procesar pedidos que incluya:

- Registro de todas las operaciones (logging)
- Control de tiempo de ejecución
- Validación de datos de entrada
- Manejo de errores y reintentos automáticos
- Autenticación de usuarios

**Solución:** Implementar decoradores para manejar estas funcionalidades transversales sin duplicar código.

---

## 🛠️ Implementación Completa

### 1. Decorador de Logging

```python
from functools import wraps
import time
from datetime import datetime

def log_operacion(nivel="INFO"):
    """
    Decorador que registra las operaciones realizadas.

    Args:
        nivel (str): Nivel de logging (INFO, WARNING, ERROR).
    """
    def decorador(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            print(f"[{nivel}] {timestamp} - Ejecutando: {func.__name__}")

            try:
                resultado = func(*args, **kwargs)
                print(f"[{nivel}] {timestamp} - {func.__name__} completado exitosamente")
                return resultado
            except Exception as e:
                print(f"[ERROR] {timestamp} - Error en {func.__name__}: {str(e)}")
                raise
        return wrapper
    return decorador
```

### 2. Decorador de Medición de Tiempo

```python
def medir_tiempo(func):
    """
    Decorador que mide el tiempo de ejecución de una función.
    """
    @wraps(func)
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fin = time.time()
        tiempo_ejecucion = fin - inicio
        print(f"⏱️  {func.__name__} ejecutado en {tiempo_ejecucion:.4f} segundos")
        return resultado
    return wrapper

```

### 3. Decorador de Validación

```python
def validar_datos(**validaciones):
    """
    Decorador que valida los argumentos de entrada.

    Args:
        **validaciones: Diccionario con las validaciones a aplicar.
    """
    def decorador(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Obtener nombres de parámetros de la función
            import inspect
            parametros = inspect.signature(func).parameters
            nombres_parametros = list(parametros.keys())

            # Crear diccionario de argumentos
            argumentos = {}
            for i, valor in enumerate(args):
                if i < len(nombres_parametros):
                    argumentos[nombres_parametros[i]] = valor
            argumentos.update(kwargs)

            # Aplicar validaciones
            for param, condiciones in validaciones.items():
                if param in argumentos:
                    valor = argumentos[param]

                    if 'tipo' in condiciones and not isinstance(valor, condiciones['tipo']):
                        raise TypeError(f"{param} debe ser de tipo {condiciones['tipo'].__name__}")

                    if 'min_valor' in condiciones and valor < condiciones['min_valor']:
                        raise ValueError(f"{param} debe ser mayor o igual a {condiciones['min_valor']}")

                    if 'no_vacio' in condiciones and condiciones['no_vacio'] and not valor:
                        raise ValueError(f"{param} no puede estar vacío")

            return func(*args, **kwargs)
        return wrapper
    return decorador

```

### 4. Decorador de Reintentos

```python
def reintentar(max_intentos=3, delay=1):
    """
    Decorador que reintenta la ejecución en caso de error.

    Args:
        max_intentos (int): Número máximo de intentos.
        delay (int): Segundos de espera entre intentos.
    """
    def decorador(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for intento in range(max_intentos):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if intento == max_intentos - 1:
                        print(f"❌ Falló después de {max_intentos} intentos: {str(e)}")
                        raise
                    else:
                        print(f"🔄 Intento {intento + 1} falló, reintentando en {delay}s...")
                        time.sleep(delay)
        return wrapper
    return decorador

```

### 5. Decorador de Autenticación

```python
# Simulamos una base de datos de usuarios
USUARIOS_AUTORIZADOS = {
    "admin": ["procesar_pedido", "cancelar_pedido", "obtener_inventario"],
    "operador": ["procesar_pedido", "obtener_inventario"],
    "cliente": ["realizar_pedido"]
}

usuario_actual = None  # Variable global para simular sesión

def requiere_autenticacion(roles_permitidos):
    """
    Decorador que verifica la autenticación y autorización.

    Args:
        roles_permitidos (list): Lista de roles que pueden ejecutar la función.
    """
    def decorador(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            if not usuario_actual:
                raise PermissionError("Usuario no autenticado")

            if usuario_actual not in roles_permitidos:
                raise PermissionError(f"Usuario '{usuario_actual}' no autorizado para {func.__name__}")

            funciones_permitidas = USUARIOS_AUTORIZADOS.get(usuario_actual, [])
            if func.__name__ not in funciones_permitidas:
                raise PermissionError(f"Función '{func.__name__}' no permitida para rol '{usuario_actual}'")

            return func(*args, **kwargs)
        return wrapper
    return decorador

```

---

## 🎯 Funciones de Negocio

### Función para Procesar Pedidos

```python
@log_operacion("INFO")
@medir_tiempo
@validar_datos(
    pedido_id={'tipo': str, 'no_vacio': True},
    cantidad={'tipo': int, 'min_valor': 1},
    precio={'tipo': float, 'min_valor': 0.01}
)
@reintentar(max_intentos=3, delay=2)
@requiere_autenticacion(["admin", "operador"])
def procesar_pedido(pedido_id, producto, cantidad, precio):
    """
    Procesa un pedido de compra.

    Args:
        pedido_id (str): ID único del pedido.
        producto (str): Nombre del producto.
        cantidad (int): Cantidad solicitada.
        precio (float): Precio unitario.

    Returns:
        dict: Información del pedido procesado.
    """
    # Simular procesamiento que puede fallar ocasionalmente
    import random
    if random.random() < 0.3:  # 30% probabilidad de fallo
        raise Exception("Error temporal en el sistema de pagos")

    # Simular tiempo de procesamiento
    time.sleep(0.5)

    total = cantidad * precio
    pedido = {
        "pedido_id": pedido_id,
        "producto": producto,
        "cantidad": cantidad,
        "precio_unitario": precio,
        "total": total,
        "estado": "procesado",
        "timestamp": datetime.now().isoformat()
    }

    print(f"✅ Pedido {pedido_id} procesado: {cantidad}x {producto} = ${total:.2f}")
    return pedido

```

### Función para Obtener Inventario

```python
@log_operacion("INFO")
@medir_tiempo
@requiere_autenticacion(["admin", "operador"])
def obtener_inventario(categoria=None):
    """
    Obtiene el inventario disponible.

    Args:
        categoria (str, optional): Filtrar por categoría.

    Returns:
        dict: Inventario disponible.
    """
    # Simular consulta a base de datos
    time.sleep(0.3)

    inventario_completo = {
        "electronica": {"laptop": 15, "smartphone": 32, "tablet": 8},
        "ropa": {"camisa": 45, "pantalon": 28, "zapatos": 12},
        "hogar": {"mesa": 5, "silla": 20, "lampara": 18}
    }

    if categoria and categoria in inventario_completo:
        resultado = {categoria: inventario_completo[categoria]}
    else:
        resultado = inventario_completo

    print(f"📦 Inventario consultado: {len(resultado)} categorías")
    return resultado

```

### Función para Cancelar Pedidos

```python
@log_operacion("WARNING")
@medir_tiempo
@validar_datos(pedido_id={'tipo': str, 'no_vacio': True})
@requiere_autenticacion(["admin"])
def cancelar_pedido(pedido_id, motivo="No especificado"):
    """
    Cancela un pedido existente.

    Args:
        pedido_id (str): ID del pedido a cancelar.
        motivo (str): Motivo de la cancelación.

    Returns:
        dict: Información de la cancelación.
    """
    # Simular cancelación
    time.sleep(0.2)

    cancelacion = {
        "pedido_id": pedido_id,
        "estado": "cancelado",
        "motivo": motivo,
        "timestamp": datetime.now().isoformat()
    }

    print(f"🚫 Pedido {pedido_id} cancelado: {motivo}")
    return cancelacion

```

---

## 🧪 Demostración del Sistema

### Función de Autenticación

```python
def autenticar_usuario(usuario):
    """Simula la autenticación de un usuario."""
    global usuario_actual
    if usuario in USUARIOS_AUTORIZADOS:
        usuario_actual = usuario
        print(f"🔐 Usuario '{usuario}' autenticado correctamente")
        return True
    else:
        print(f"❌ Usuario '{usuario}' no encontrado")
        return False

def cerrar_sesion():
    """Cierra la sesión actual."""
    global usuario_actual
    if usuario_actual:
        print(f"👋 Sesión cerrada para '{usuario_actual}'")
        usuario_actual = None

```

### Escenarios de Uso

```python
def demo_sistema():
    """Demuestra el funcionamiento del sistema."""

    print("=" * 60)
    print("🏪 SISTEMA DE GESTIÓN DE PEDIDOS - TECHCOMMERCE")
    print("=" * 60)

    # Escenario 1: Usuario Admin
    print("\n--- ESCENARIO 1: USUARIO ADMIN ---")
    autenticar_usuario("admin")

    try:
        # Procesar pedido válido
        pedido1 = procesar_pedido("PED-001", "Laptop Gaming", 2, 1200.99)

        # Consultar inventario
        inventario = obtener_inventario("electronica")

        # Cancelar pedido
        cancelacion = cancelar_pedido("PED-002", "Cliente cambió de opinión")

    except Exception as e:
        print(f"Error: {e}")

    cerrar_sesion()

    # Escenario 2: Usuario Operador
    print("\n--- ESCENARIO 2: USUARIO OPERADOR ---")
    autenticar_usuario("operador")

    try:
        # Procesar pedido válido
        pedido2 = procesar_pedido("PED-003", "Smartphone", 1, 699.99)

        # Intentar cancelar pedido (sin permisos)
        cancelar_pedido("PED-003", "Test")

    except PermissionError as e:
        print(f"❌ Error de permisos: {e}")
    except Exception as e:
        print(f"Error: {e}")

    cerrar_sesion()

    # Escenario 3: Validaciones
    print("\n--- ESCENARIO 3: VALIDACIONES ---")
    autenticar_usuario("admin")

    try:
        # Datos inválidos
        procesar_pedido("", "Producto", -1, 0)
    except (ValueError, TypeError) as e:
        print(f"❌ Error de validación: {e}")

    cerrar_sesion()

    # Escenario 4: Sin autenticación
    print("\n--- ESCENARIO 4: SIN AUTENTICACIÓN ---")
    try:
        obtener_inventario()
    except PermissionError as e:
        print(f"❌ Error de autenticación: {e}")

# Ejecutar demostración
if __name__ == "__main__":
    demo_sistema()

```

---

## 📊 Resultados Esperados

### Salida del Sistema

```
============================================================
🏪 SISTEMA DE GESTIÓN DE PEDIDOS - TECHCOMMERCE
============================================================

--- ESCENARIO 1: USUARIO ADMIN ---
🔐 Usuario 'admin' autenticado correctamente
[INFO] 2024-12-10 15:30:15 - Ejecutando: procesar_pedido
✅ Pedido PED-001 procesado: 2x Laptop Gaming = $2401.98
[INFO] 2024-12-10 15:30:15 - procesar_pedido completado exitosamente
⏱️  procesar_pedido ejecutado en 0.5234 segundos

[INFO] 2024-12-10 15:30:16 - Ejecutando: obtener_inventario
📦 Inventario consultado: 1 categorías
[INFO] 2024-12-10 15:30:16 - obtener_inventario completado exitosamente
⏱️  obtener_inventario ejecutado en 0.3012 segundos

[WARNING] 2024-12-10 15:30:16 - Ejecutando: cancelar_pedido
🚫 Pedido PED-002 cancelado: Cliente cambió de opinión
[WARNING] 2024-12-10 15:30:16 - cancelar_pedido completado exitosamente
⏱️  cancelar_pedido ejecutado en 0.2001 segundos

👋 Sesión cerrada para 'admin'

--- ESCENARIO 2: USUARIO OPERADOR ---
🔐 Usuario 'operador' autenticado correctamente
[INFO] 2024-12-10 15:30:17 - Ejecutando: procesar_pedido
✅ Pedido PED-003 procesado: 1x Smartphone = $699.99
[INFO] 2024-12-10 15:30:17 - procesar_pedido completado exitosamente
⏱️  procesar_pedido ejecutado en 0.5123 segundos

❌ Error de permisos: Función 'cancelar_pedido' no permitida para rol 'operador'
👋 Sesión cerrada para 'operador'

```

---