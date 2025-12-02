# 🗒️Práctica 07 - Decoradores en Python (Parte 2)

## Ejercicio 1 — Decorador sin `wraps` y metadatos perdidos

Crea un decorador llamado `trazar_llamada` que:

- Imprima el mensaje:
    
    `>>> Ejecutando función decorada...`
    
    **antes** de llamar a la función original.
    
- Acepte cualquier cantidad de argumentos y parámetros nombrados.

Aplica este decorador a **dos funciones**:

1. `enviar_mensaje()`
    - Imprime un mensaje como: `"Mensaje enviado correctamente"`.
    - Tiene una docstring que explique que envía un mensaje.
2. `calcular_suma(a, b)`
    - Imprime la suma de `a` y `b`.
    - Tiene una docstring que explique que calcula la suma de dos números.

Luego, en el mismo script:

1. Llama a `enviar_mensaje()` y `calcular_suma(3, 7)`.
2. Imprime en pantalla los metadatos:
    - `enviar_mensaje.__name__`
    - `enviar_mensaje.__doc__`
    - `calcular_suma.__name__`
    - `calcular_suma.__doc__`

> Condición: No uses functools.wraps en este ejercicio.
> 

---

## Ejercicio 2 — Decorador con `wraps` y conservación de metadatos

Toma la idea del **Ejercicio 1**, pero ahora:

- Crea un decorador llamado `trazar_llamada_wraps` que haga lo mismo que `trazar_llamada`, pero:
    - Use `from functools import wraps`.
    - Aplique `@wraps(func)` a la función interna del decorador.

Vuelve a definir:

1. `enviar_mensaje()` con su docstring correspondiente.
2. `calcular_suma(a, b)` con su docstring correspondiente.

Decora ambas con `@trazar_llamada_wraps` y:

1. Llama a `enviar_mensaje()` y `calcular_suma(3, 7)`.
2. Imprime:
    - `enviar_mensaje.__name__`
    - `enviar_mensaje.__doc__`
    - `calcular_suma.__name__`
    - `calcular_suma.__doc__`

> Objetivo: Comparar el resultado con el Ejercicio 1 y verificar que ahora los metadatos sí se conservan.
> 

---

## Ejercicio 3 — Decorador para métodos de instancia (logging simple)

Crea un decorador llamado `log_metodo` que:

- Esté pensado para decorar **métodos de instancia** (reciben `self`).
- Antes de ejecutar el método, imprima:
    
    `Llamando método <nombre_del_método> de la clase <nombre_de_la_clase>`.
    
- Después de ejecutarlo, imprima:
    
    `Método <nombre_del_método> finalizado`.
    

Define una clase `Sensor` con:

- Atributo de instancia `nombre`.
- Método `leer_valor(self)` decorado con `@log_metodo` que:
    - Imprima `"Leyendo valor del sensor..."`.

Requisitos:

1. Crea una instancia `s = Sensor("Temperatura")`.
2. Llama `s.leer_valor()`.
3. Observa el orden de los mensajes por pantalla.

> Objetivo: Practicar decoradores aplicados a métodos y el uso de self dentro de wrapper.
> 

---

## Ejercicio 4 — Uso completo de `@property` con validación

Define una clase `CuentaBancaria` con:

- Atributo privado `_saldo`.
- Un constructor que reciba un saldo inicial.

Implementa:

1. Una propiedad `saldo` con:
    - **Getter** que devuelva el saldo actual.
    - **Setter** que:
        - No permita establecer un saldo negativo.
        - Lance `ValueError` si el valor es menor que 0.
    - **Deleter** que:
        - Imprima `"Reiniciando saldo a 0..."`.
        - Elimine el atributo `_saldo`.

En el código principal:

1. Crea una cuenta con saldo inicial de `1000`.
2. Imprime el saldo usando la propiedad.
3. Modifica el saldo a `500` usando el setter.
4. Intenta asignar un saldo negativo y captura la excepción.
5. Usa el `del` sobre la propiedad `saldo`.

> Objetivo: Practicar @property, @<prop>.setter y @<prop>.deleter con validación.
> 

---

## Ejercicio 5 — `@classmethod` como constructor alternativo

Crea una clase `Libro` con:

- Atributos: `titulo`, `autor`, `precio`.
- Constructor estándar `__init__` que reciba esos tres parámetros.

Añade un método de clase:

- `@classmethod`
- Nombre sugerido: `desde_cadena(cls, cadena_libro)`
- La cadena tendrá el formato: `"titulo;autor;precio"`
- Debe:
    - Separar la cadena.
    - Convertir el precio a `float`.
    - Devolver una instancia de `Libro`.

En el código principal:

1. Crea dos libros usando el constructor normal.
2. Crea un tercer libro usando:
    
    `Libro.desde_cadena("Python Avanzado;Juan Pérez;39.99")`.
    
3. Imprime los atributos de los tres libros.

> Objetivo: Practicar @classmethod como constructor alternativo y manejo de cadenas.
> 

---

## Ejercicio 6 — `@staticmethod` para utilidades dentro de una clase

Define una clase `AnalizadorTexto` con:

- Método estático `contar_palabras(texto)`:
    - Recibe un `str` y devuelve el número de palabras (separando por espacios).
- Método estático `es_palindromo(texto)`:
    - Devuelve `True` si el texto es palíndromo ignorando espacios y mayúsculas/minúsculas.
- Método normal `resumen(self, texto)`:
    - Llama internamente a `contar_palabras(texto)` y `es_palindromo(texto)`.
    - Imprime un pequeño informe, por ejemplo:
        - `"Palabras: 5"`
        - `"¿Es palíndromo?: False"`

Requisitos:

1. Llama a los métodos estáticos desde la clase, por ejemplo:
    
    `AnalizadorTexto.contar_palabras("hola mundo desde python")`.
    
2. Crea una instancia y llama a `resumen(texto)` con al menos dos frases distintas.

> Objetivo: Practicar @staticmethod y su diferencia con métodos de instancia.
> 

---

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