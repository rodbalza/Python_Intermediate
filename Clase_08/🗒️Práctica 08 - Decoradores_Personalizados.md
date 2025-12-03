# 🗒️Práctica 08 - Decoradores

## Ejercicio 7 — Decorador de logging con hora y retorno

Implementa un decorador llamado `registrar_ejecucion` que:

- Use `functools.wraps`.
- Imprima:
    - La hora actual (puedes usar `datetime.now()`).
    - El nombre de la función.
    - Los argumentos posicionales que recibe.
- Después de ejecutar la función:
    - Imprima el valor de retorno.

Define una función decorada:

- `procesar_datos(lista_numeros)`:
    - Recibe una lista de números.
    - Devuelve la suma de los elementos.

Pruebas:

1. Llama a `procesar_datos([1, 2, 3, 4])`.
2. Observa en consola:
    - Hora en la que se ejecuta.
    - Nombre de la función.
    - Argumentos.
    - Resultado devuelto.

> Objetivo: Practicar decoradores con logging y retorno de valores.
> 

---

## Ejercicio 8 — Decorador con parámetros para validar tipos

Crea un decorador con parámetros llamado `validar_argumentos` que:

- Se use como:
    
    `@validar_argumentos(int, int, int)`
    
- Verifique que los primeros N argumentos posicionales coincidan con los tipos esperados.
- Si algún argumento no coincide:
    - Lance `TypeError` con un mensaje descriptivo.
- Si todo está bien:
    - Ejecute la función original y devuelva su resultado.

Define una clase `CalculadoraAvanzada` con:

- Método de instancia `multiplicar(self, a, b, c)` decorado con `@validar_argumentos(int, int, int)`
    - Devuelve `a * b * c`.

Crea una instancia y prueba:

1. `multiplicar(2, 3, 4)` → debe funcionar.
2. `multiplicar(2, "3", 4)` → debe lanzar `TypeError`.

> Objetivo: Practicar decoradores con parámetros y validación de tipos.
> 

---

## Ejercicio 9 — Decorador de clase para añadir un método `to_dict`

Crea un decorador de clase llamado `añadir_to_dict` que:

- Reciba una clase.
- Añada automáticamente un método `to_dict(self)` a la clase decorada.
- `to_dict` debe:
    - Recorrer `self.__dict__`.
    - Devolver un diccionario con los atributos y sus valores.

Decora una clase `ProductoTienda`:

- Atributos: `nombre`, `categoria`, `precio`.
- Constructor que inicialice estos atributos.

En el código principal:

1. Crea una instancia de `ProductoTienda`.
2. Llama a `obj.to_dict()` y guarda el resultado.
3. Imprime el diccionario devuelto.

> Objetivo: Practicar decoradores aplicados a clases completas y manipulación de __dict__.
> 

---

## Ejercicio 10 — Registro de clases y fábrica de objetos

Implementa un sistema simple de registro de clases:

1. Un diccionario global, por ejemplo `REGISTRO_TAREAS = {}`.
2. Un decorador con parámetros `@registrar_tarea(nombre)` que:
    - Reciba un nombre de registro (string).
    - Agregue la clase al diccionario `REGISTRO_TAREAS` bajo esa clave.
3. Una función `crear_tarea(tipo, *args, **kwargs)` que:
    - Busque la clase en `REGISTRO_TAREAS` usando `tipo`.
    - Devuelva una instancia de la clase correspondiente.

Crea dos clases decoradas:

- `@registrar_tarea("email")`
    
    Clase `TareaEmail` con:
    
    - Atributo `destinatario`.
- `@registrar_tarea("backup")`
    
    Clase `TareaBackup` con:
    
    - Atributo `ruta`.

En el código principal:

1. Crea una instancia usando `crear_tarea("email", "usuario@ejemplo.com")`.
2. Crea otra usando `crear_tarea("backup", "/ruta/backup")`.
3. Imprime el `type()` de cada objeto creado y sus atributos.