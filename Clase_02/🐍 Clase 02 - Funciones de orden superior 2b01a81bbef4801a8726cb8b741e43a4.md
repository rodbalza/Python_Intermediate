# 🐍 Clase 02 - Funciones de orden superior

# Funciones de orden superior.

Una función de orden superior es una función que puede recibir otras funciones como argumentos o devolverlos:

```python
def incrementar(x):
    return x + 1

def func_orden_sup(x, func):
    return x + func(x)

resultado = func_orden_sup(2, incrementar)
# 2 + (2 + 1)
print(resultado)
```

salida:

```
5
```

¿Cómo sería con lambda?

```python
incrementar = lambda x: x + 1

func_orden_sup = lambda x, func: x + func(x)

resultado = func_orden_sup(2, incrementar)
print(resultado)
```

salida:

```
5
```

Otro ejemplo, podemos pasar una función lambda a la función `built-in` llamada `sorted()` para ordenar un diccionario por valores:

```python
d = {'Aceite': 230, 'Pan': 150, 'Salmón': 175, 'Jamón': 35}
d1 = sorted(d.items(), key = lambda a: a[1])
print(d1)
```

salida:

```
[('Jamón', 35), ('Pan', 150), ('Salmón', 175), ('Aceite', 230)
```

<aside>
💡

Ejercicios de refuerzo:

### ✅ **Ejercicio 1: Aplicar una función a cada elemento**

Crea una función de orden superior llamada `aplicar_funcion` que reciba una lista de números y una función, y devuelva una nueva lista con la función aplicada a cada elemento.

- Solución
    
    ```python
    # Entrada:
    numeros = [1, 2, 3, 4]
    # Usando una función lambda para duplicar cada número
    resultado = aplicar_funcion(numeros, lambda x: x * 2)
    
    # Salida esperada:
    [2, 4, 6, 8]
    ```
    

### ✅ **Ejercicio 2: Filtrar elementos con lambda**

Escribe una función llamada `filtrar_lista` que reciba una lista y una función de condición (lambda) y devuelva una lista con los elementos que cumplen la condición.

- Solución
    
    ```python
    # Entrada:
    numeros = [10, 15, 20, 25, 30]
    # Filtrar números mayores a 20
    resultado = filtrar_lista(numeros, lambda x: x > 20)
    
    # Salida esperada:
    [25, 30]
    ```
    

### ✅ **Ejercicio 3: Ordenar por longitud usando lambda**

Dado un diccionario donde las claves son palabras y los valores son su frecuencia, ordena el diccionario por la longitud de las palabras usando `sorted()` y una función lambda.

- Solución
    
    ```python
    # Entrada:
    palabras = {'sol': 5, 'estrella': 3, 'luz': 8, 'universo': 2}
    # Ordenar por longitud de la clave
    resultado = sorted(palabras.items(), key=lambda x: len(x[0]))
    
    # Salida esperada:
    
    ```
    

</aside>

### Para facilitar la programación funcional, Python proporciona 3 funciones de orden superior: `map()`, `filter()` y `reduce()`.

# map()

### `map()`: su objetivo principal es hacer transformaciones a una lista dada de elementos.

🧾 **Sintaxis general de `map()`**

```python
map(funcion, iterable)
```

### ✅ **Parámetros:**

- `funcion`: Una función que se aplicará a cada elemento del iterable. Puede ser:
    - una función definida con `def`
    - una función anónima `lambda`
- `iterable`: Una secuencia como una lista, tupla, conjunto, etc.

> Nota: Si usas map() en Python 3, devuelve un objeto iterable tipo map, por lo que normalmente se convierte a lista con list().
> 

![image.png](image.png)

map() con listas: `list()`

![image.png](image%201.png)

![image.png](image%202.png)

```python
numeros = [1, 2, 3, 4]
numeros_2 = []
for i in numeros:
    numeros_2.append(i * 2)
print(numeros)
print(numeros_2)
```

salida:

```
[1, 2, 3, 4]
[2, 4, 6, 8]
```

Esto se puede hacer en una sola línea usando `map()` y `lambda`:

```python
numeros = [1, 2, 3, 4]
numeros_3 = list(map(lambda i: i*2, numeros))
print(numeros_3)
```

salida:

```
[2, 4, 6, 8]
```

con set()

```python
numeros = [1, 2, 3, 4]
numeros_3 = set(map(lambda i: i*2, numeros))
print(numeros_3)

```

salida:

```
{8, 2, 4, 6}
```

Con tuplas

```python
numeros_3 = tuple(map(lambda i: i*2, numeros))
print(numeros_3)

```

salida:

```
(2, 4, 6, 8)
```

Mas ejemplos con `map()`

```python
numeros_4 = [1, 2, 3, 4]
numeros_5 = [5, 6, 7]
print(numeros_4)
print(numeros_5)
print()
resultado = list(map(lambda x, y: x + y, numeros_4, numeros_5))
print(resultado)
```

salida:

```
[1, 2, 3, 4]
[5, 6, 7]

[6, 8, 10]
```

Uso de `map()` con diccionarios, compara con el codigo sin usar lambda y map

```python
# Items ordenes de compra almacenada en una lista de diccionarios
items = [
    {
        'producto': 'camisa',
        'precio': 100,
    },
    {
        'producto': 'pantalones',
        'precio': 300
    },
    {
        'producto': 'pantalones 2',
        'precio': 200
    }
]

# Quiero transformar esa información a una lista que
# solo contenga los precios: precios = [100, 200, 300]

precios = list(map(lambda item: item['precio'], items))
print(precios)
```

salida:

```
[100, 300, 200]
```

Sin lambda y map

```python
# Items ordenes de compra almacenadas en una lista de diccionarios
items = [
    {
        'producto': 'camisa',
        'precio': 100,
    },
    {
        'producto': 'pantalones',
        'precio': 300,
    },
    {
        'producto': 'pantalones 2',
        'precio': 200
    }
]

# Función que recibe la lista de items y extrae los precios
def obtener_precios(lista_items):
    precios = []
    for item in lista_items:
        precios.append(item['precio'])
    return precios

# Usamos la función
precios = obtener_precios(items)
print(precios)
```

Supongamos que a la lista original le falta añadir un atributo, digamos: impuestos. En este caso no se puede hacer solo con lambda ya que la operación de añadir esa clave impuestos a la lista no se hace en una sola línea:

```python
def añadir_impuestos(item):
    item['impuestos'] = item['precio'] * .15
    return item

new_items = list(map(añadir_impuestos, items))
print(new_items)
```

Salida:

```
[{'producto': 'camisa', 'precio': 100, 'impuestos': 15.0}, {'producto': 'pantalones', 'precio': 300, 'impuestos': 45.0}, {'producto': 'pantalones 2', 'precio': 200, 'impuestos': 30.0}]
```

<aside>
💡

Ejercicios de refuerzo

### ✅ **Ejercicio 4: Convertir temperaturas**

Tienes una lista de temperaturas en grados Celsius. Usa `map()` y `lambda` para convertirlas a grados Fahrenheit.

**Entrada:**

```python
temperaturas_celsius = [0, 20, 37, 100]
```

Salida esperada:

```python
[32.0, 68.0, 98.6, 212.0]
```

- Solución
    
    ```python
    temperaturas_celsius = [0, 20, 37, 100]
    
    # Usamos map() y lambda para aplicar la fórmula F = C * 9/5 + 32
    temperaturas_fahrenheit = list(map(lambda c: c * 9/5 + 32, temperaturas_celsius))
    
    print(temperaturas_fahrenheit)
    
    ```
    

### ✅ **Ejercicio 5: Calcular áreas de círculos**

Dada una lista de radios, usa `map()` y `lambda` para calcular el área de cada círculo (usa π = 3.1416).

**Entrada:**

```python
radios = [1, 2, 3, 4]
```

Salida esperada:

```python
1
```

- Solución
    
    ```python
    radios = [1, 2, 3, 4]
    pi = 3.1416
    
    # Usamos map() y lambda para calcular el área: A = π * r^2
    areas = list(map(lambda r: pi * (r ** 2), radios))
    
    print(areas)
    
    ```
    

### ✅ **Ejercicio 6: Extraer nombres de productos**

Tienes una lista de diccionarios que representan productos con nombre y precio. Usa `map()` y `lambda` para obtener solo los nombres.

**Entrada:**

```python
productos = [
    {'nombre': 'Laptop', 'precio': 1200},
    {'nombre': 'Mouse', 'precio': 25},
    {'nombre': 'Teclado', 'precio': 45}
]
```

Salida esperada:

```python
['Laptop', 'Mouse', 'Teclado']
```

- Solución
    
    ```python
    productos = [
        {'nombre': 'Laptop', 'precio': 1200},
        {'nombre': 'Mouse', 'precio': 25},
        {'nombre': 'Teclado', 'precio': 45}
    ]
    
    # Usamos map() y lambda para extraer solo el valor de la clave 'nombre'
    nombres = list(map(lambda item: item['nombre'], productos))
    
    print(nombres)
    ```
    
</aside>

# Filter()

### `filter()`. Filtrar elementos de una lista. Se seleccionan ciertos elementos para que pertenezcan a una nueva lista.

🧾 **Sintaxis de `filter()`**

```python
filter(funcion, iterable)
```

### ✅ **Parámetros:**

- `funcion`: Una función que devuelve `True` o `False` para cada elemento. Solo los elementos que hagan que esta función devuelva `True` serán incluidos en el resultado.
    - Puede ser una función definida con `def` o una `lambda`.
- `iterable`: Una secuencia como una lista, tupla, etc.

> 🔁 Al igual que map(), filter() devuelve un objeto iterable tipo filter, por lo tanto, se suele convertir en list() para ver el resultado.
> 

![image.png](image%203.png)

![image.png](image%204.png)

![image.png](image%205.png)

```python
numbers = [1,2,3,4,5]
print(numbers)
new_numbers = list(filter(lambda x: x % 2 == 0, numbers))
print(new_numbers)
```

salida:

```
[1, 2, 3, 4, 5]
[2, 4]
```

sin funciones de orden superior

```python
numbers = [1, 2, 3, 4, 5]
print(numbers)

def es_par(x):
    return x % 2 == 0

new_numbers = []
for numero in numbers:
    if es_par(numero):
        new_numbers.append(numero)

print(new_numbers)

```

Otro ejemplo:

Dada la siguiente lista de diccionarios:

```python
matches = [
    {
        'home_team': 'España',
        'away_team': 'Italia',
        'home_team_score': 3,
        'away_team_score': 1,
        'home_team_result': 'Win'
    },
    {
        'home_team': 'Francia',
        'away_team': 'Alemania',
        'home_team_score': 1,
        'away_team_score': 1,
        'home_team_result': 'Draw'
    },
    {
        'home_team': 'Portugal',
        'away_team': 'Suiza',
        'home_team_score': 5,
        'away_team_score': 0,
        'home_team_result': 'Win'
    },
]
# Necesitamos filtrar solo los equipos locales que ganaron
```

### Necesitamos filtrar solo los equipos locales que ganaron

---

Veamos el estado original

```python
# Veamos el estado original
print(matches)
```

salida:

```
[{'home_team': 'España', 'away_team': 'Italia', 'home_team_score': 3, 'away_team_score': 1, 'home_team_result': 'Win'}, {'home_team': 'Francia', 'away_team': 'Alemania', 'home_team_score': 1, 'away_team_score': 1, 'home_team_result': 'Draw'}, {'home_team': 'Portugal', 'away_team': 'Suiza', 'home_team_score': 5, 'away_team_score': 0, 'home_team_result': 'Win'}]
```

---

Veamos el nro de partidos

```python
print(len(matches))

```

salida:

```
3
```

---

```python
new_list = list(filter(lambda item: item['home_team_result'] == 'Win', matches))
print(new_list)
print(len(new_list))
```

salida:

```
[{'home_team': 'España', 'away_team': 'Italia', 'home_team_score': 3, 'away_team_score': 1, 'home_team_result': 'Win'}, {'home_team': 'Portugal', 'away_team': 'Suiza', 'home_team_score': 5, 'away_team_score': 0, 'home_team_result': 'Win'}]
2
```

---

```python
lst1 = ['A', 'X', 'Y', '3', 'M', '4', 'D']
f1 = filter(str.isalpha, lst1)
print(list(f1))
```

salida:

```
['A', 'X', 'Y', 'M', 'D']
```

---

Código con filter y def (sin lambda)

```python
def fun(n):
    if n % 5 == 0:
        return True
    else:
        return False

lst2 = [5, 10, 18, 27, 25]
f2 = filter(fun, lst2)
print(list(f2))
```

salida:

```
[5, 10, 25]
```

Código con lambda + filter

```python
lst2 = [5, 10, 18, 27, 25]

# Usamos lambda en lugar de la función 'fun'
f2 = filter(lambda n: n % 5 == 0, lst2)

print(list(f2))
```

salida:

```
[5, 10, 25]
```

<aside>
💡

Ejercicios de refuerzo

### ✅ **Ejercicio 7: Filtrar números impares**

Dada una lista de números enteros, usa `filter()` y `lambda` para obtener solo los números impares.

**Entrada:**

```python
numeros = [10, 15, 20, 25, 30, 35]
```

Salida  esperada:

```python
[15, 25, 35]
```

- Solución
    
    ```python
    numeros = [10, 15, 20, 25, 30, 35]
    
    # Usamos filter() y lambda para quedarnos con los impares
    numeros_impares = list(filter(lambda x: x % 2 != 0, numeros))
    
    print(numeros_impares)
    ```
    

### ✅ **Ejercicio 8: Filtrar palabras que empiezan con vocal**

Dada una lista de palabras, usa `filter()` para obtener solo aquellas que comienzan con una vocal (a, e, i, o, u).

**Entrada:**

```python
palabras = ['manzana', 'uva', 'pera', 'banana', 'aguacate', 'kiwi']
```

Salida esperada:

```python
['uva', 'aguacate']
```

- Solución
    
    ```python
    palabras = ['manzana', 'uva', 'pera', 'banana', 'aguacate', 'kiwi']
    
    # Usamos filter() y lambda para seleccionar palabras que empiezan con vocal
    palabras_vocal = list(filter(lambda w: w[0].lower() in 'aeiou', palabras))
    
    print(palabras_vocal)
    ```
    

### ✅ **Ejercicio 9: Filtrar productos caros**

Dada una lista de diccionarios que representan productos con nombre y precio, usa `filter()` para obtener solo los productos cuyo precio sea mayor a 100.

**Entrada:**

```python
productos = [
    {'nombre': 'Laptop', 'precio': 1200},
    {'nombre': 'Mouse', 'precio': 25},
    {'nombre': 'Teclado', 'precio': 45},
    {'nombre': 'Monitor', 'precio': 300}
]
```

Salida esperada:

```python
[{'nombre': 'Laptop', 'precio': 1200}, {'nombre': 'Monitor', 'precio': 300}]
```

- Solución
    
    ```python
    productos = [
        {'nombre': 'Laptop', 'precio': 1200},
        {'nombre': 'Mouse', 'precio': 25},
        {'nombre': 'Teclado', 'precio': 45},
        {'nombre': 'Monitor', 'precio': 300}
    ]
    
    # Usamos filter() y lambda para quedarnos con productos cuyo precio > 100
    productos_caros = list(filter(lambda p: p['precio'] > 100, productos))
    
    print(productos_caros)
    ```
    

</aside>

---

# Reduce()

### `reduce()`. "Reducir a un único valor".

🧾 **Sintaxis general de `reduce()`**

```python
from functools import reduce

reduce(funcion, iterable[, valor_inicial])
```

### ✅ **Parámetros:**

1. **`funcion`**:
    
    Una función que toma dos argumentos y devuelve un valor. Esta función se aplica acumulativamente a los elementos del iterable.
    
2. **`iterable`**:
    
    Una secuencia como lista, tupla, etc.
    
3. **`valor_inicial`** (opcional):
    
    Valor inicial que se usa como primer argumento en la primera llamada.
    

---

---

![image.png](image%206.png)

![image.png](image%207.png)

![image.png](image%208.png)

---

![image.png](image%209.png)

![image.png](image%2010.png)

![image.png](image%2011.png)

**Ejemplo**: Se tiene una lista de números enteros  `numbers = [1, 2, 3, 4]`
Utiliza la función `reduce()` del módulo `functools` junto con una función `lambda` para calcular la **suma acumulada** de todos los elementos de la lista. La función `lambda` debe recibir dos parámetros: un **acumulador (`counter`)** y un **elemento actual (`item`)**, y devolver la suma de ambos.

```python
from functools import reduce

numbers = [1, 2, 3, 4]
result = reduce(lambda counter, item: counter + item, numbers)
print(result)
```

salida:

```
10
```

| Iteration | Counter | Item | Return |
| --- | --- | --- | --- |
| 1 | 0 | 1 | 1 |
| 2 | 1 | 2 | 3 |
| 3 | 3 | 3 | 6 |
| 4 | 6 | 4 | 10 |

---

---

---

Versión sin funciones de orden superior

```python
numbers = [1, 2, 3, 4]

def sumar_lista(lista):
    total = 0
    for numero in lista:
        total += numero
    return total

result = sumar_lista(numbers)
print(result)

```

salida:

```
10
```

| Iteration | Counter | Item | Return |
| --- | --- | --- | --- |
| 1 | 0 | 1 | 1 |
| 2 | 1 | 2 | 3 |
| 3 | 3 | 3 | 6 |
| 4 | 6 | 4 | 10 |

---

Veamos otro ejemplo de `reduce()` con un diccionario para calcular el precio total:

```python
import functools
items = [
    {
        'producto': 'camisa',
        'precio': 100,
    },
    {
        'producto': 'pantalones',
        'precio': 300
    },
    {
        'producto': 'pantalones 2',
        'precio': 200
    }
]

def accum(counter, item):
    print('counter => ',counter)
    print('item => ',item)
    return counter + item['precio']

total = functools.reduce(accum, items, 0)
print(total)
```

salida:

```
counter => 0
item => {'producto': 'camisa', 'precio': 100}
counter => 100
item => {'producto': 'pantalones', 'precio': 300}
counter => 400
item => {'producto': 'pantalones 2', 'precio': 200}
600
```

<aside>
💡

Ejercicios de refuerzo

### ✅ **Ejercicio 10: Calcular el producto de todos los números**

Dada una lista de números enteros, usa `reduce()` y `lambda` para calcular el producto de todos los elementos.

**Entrada:**

```python
numeros = [2, 3, 4, 5]
```

Salida esperada:

```python
120
```

- Solución
    
    ```python
    from functools import reduce
    
    numeros = [2, 3, 4, 5]
    
    # Usamos reduce() y lambda para multiplicar todos los elementos
    producto = reduce(lambda acc, x: acc * x, numeros)
    
    print(producto)
    ```
    

### ✅ **Ejercicio 11: Concatenar palabras en una sola cadena**

Dada una lista de palabras, usa `reduce()` para unirlas en una sola cadena separada por espacios.

**Entrada:**

```python
palabras = ['Python', 'es', 'genial']
```

Salida esperada:

```python
"Python es genial"
```

- Solución
    
    ```python
    from functools import reduce
    
    palabras = ['Python', 'es', 'genial']
    
    # Usamos reduce() para unir las palabras con un espacio
    frase = reduce(lambda acc, w: acc + ' ' + w, palabras)
    
    print(frase)
    ```
    

### ✅ **Ejercicio 12: Calcular el total de precios con descuento**

Dada una lista de diccionarios que representan productos con nombre y precio, usa `reduce()` para calcular el total aplicando un descuento del 10% a cada precio antes de sumarlo.

**Entrada:**

```python
productos = [
    {'nombre': 'Laptop', 'precio': 1200},
    {'nombre': 'Mouse', 'precio': 25},
    {'nombre': 'Monitor', 'precio': 300}
]
```

Salida esperada:

```python
1387.5
```

- Solución
    
    ```python
    from functools import reduce
    
    productos = [
        {'nombre': 'Laptop', 'precio': 1200},
        {'nombre': 'Mouse', 'precio': 25},
        {'nombre': 'Monitor', 'precio': 300}
    ]
    
    # Usamos reduce() para sumar los precios aplicando un descuento del 10%
    total_con_descuento = reduce(lambda acc, p: acc + (p['precio'] * 0.9), productos, 0)
    
    print(total_con_descuento)
    
    ```
    

</aside>

# Ejercicios combinados

### ✅ **Ejercicio 13: Suma de cuadrados de números pares**

**Objetivo:** De una lista de números, filtrar los pares, elevarlos al cuadrado y sumar el resultado.

**Código:**

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5, 6]

resultado = reduce(
    lambda acc, x: acc + x,
    map(lambda x: x ** 2, filter(lambda x: x % 2 == 0, numeros)),
    0
)
```

Salida:

```python
56
```

## **Ejercicio 14: Concatenar palabras que empiezan con vocal**

**Objetivo:** De una lista de palabras, filtrar las que empiezan con vocal y unirlas en una sola cadena.

```python
from functools import reduce

palabras = ['uva', 'manzana', 'aguacate', 'pera', 'kiwi']

resultado = reduce(
    lambda acc, w: acc + ' ' + w,
    filter(lambda w: w[0].lower() in 'aeiou', palabras)
)

print(resultado)
```

Salida:

```python
uva aguacate
```

## **Ejercicio 15: Producto de números mayores a 3**

**Objetivo:** Filtrar números mayores a 3, duplicarlos y calcular el producto.

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5]

resultado = reduce(
    lambda acc, x: acc * x,
    map(lambda x: x * 2, filter(lambda x: x > 3, numeros)),
    1
)

print(resultado)
```

Salida:

```python
80
```

## **Ejercicio 16: Suma de precios con descuento**

**Objetivo:** De una lista de productos, filtrar los que cuestan más de 100, aplicar un descuento del 10% y sumar el total.

```python
from functools import reduce

productos = [
    {'nombre': 'Laptop', 'precio': 1200},
    {'nombre': 'Mouse', 'precio': 25},
    {'nombre': 'Monitor', 'precio': 300}
]

resultado = reduce(
    lambda acc, p: acc + p,
    map(lambda p: p['precio'] * 0.9, filter(lambda p: p['precio'] > 100, productos)),
    0
)

```

Salida:

```python
1387.5
```

## **Ejercicio 17: Longitud total de palabras largas**

**Objetivo:** Filtrar palabras con más de 4 letras, convertirlas a mayúsculas y sumar la longitud total.

```python
from functools import reduce

palabras = ['sol', 'estrella', 'luz', 'universo']

resultado = reduce(
    lambda acc, x: acc + len(x),
    map(lambda w: w.upper(), filter(lambda w: len(w) > 4, palabras)),
    0
)

print(resultado) 
```

Salida:

```python
17
```