# 🗒️Práctica 02 Funciones de orden superior

### **✅ Ejercicio 1: Elevar al cubo**

Usa `map()` y `lambda` para elevar al cubo cada número de la lista.

```python
numeros = [2, 3, 4, 5]
```

**Salida esperada:**

```python
[8, 27, 64, 125]
```

### **✅ Ejercicio 2: Filtrar múltiplos de 7**

Usa `filter()` y `lambda` para quedarte solo con los números múltiplos de 7.

```python
numeros = [14, 23, 28, 35, 42, 51, 56]
```

**Salida esperada:**

```python
[14, 28, 35, 42, 56]
```

### **✅ Ejercicio 3: Concatenar con guiones**

Usa `reduce()` y `lambda` para unir todas las palabras con un guión `-`.

```python
from functools import reduce
palabras = ['Python', 'es', 'tremendo', '2025']
```

**Salida esperada:**

```python
"Python-es-tremendo-2025"
```

### **✅ Ejercicio 4: Ordenar por nombre descendente**

Usa `sorted()` y `lambda` para ordenar la lista de diccionarios por el nombre del producto en orden alfabético descendente.

```python
productos = [
    {'producto': 'Teclado', 'precio': 45},
    {'producto': 'Auriculares', 'precio': 120},
    {'producto': 'Monitor', 'precio': 300},
    {'producto': 'Ratón', 'precio': 25}
]

```

**Salida esperada:**

```python
[
    {'producto': 'Teclado', 'precio': 45},
    {'producto': 'Ratón', 'precio': 25},
    {'producto': 'Monitor', 'precio': 300},
    {'producto': 'Auriculares', 'precio': 120}
]

```

### **✅ Ejercicio 5: Convertir a negativo y duplicar**

Usa `map()` y `lambda` para convertir cada número en su negativo y luego multiplicarlo por 2.

```python
valores = [5, -8, 10, -3, -15]

```

**Salida esperada:**

```python
[-10, 16, -20, -6, 30]

```

### **✅ Ejercicio 6: Filtrar palabras de más de 6 letras**

Usa `filter()` y `lambda` para obtener solo las palabras con longitud mayor a 6.

```python
frutas = ['manzana', 'kiwi', 'sandía', 'melocotón', 'plátano', 'fresa', 'mandarina']

```

**Salida esperada:**

```python
['melocotón', 'mandarina']

```

### **✅ Ejercicio 7: Precio final con IVA del 21 % (solo los precios)**

Usa `map()` y `lambda` para calcular el precio final con IVA del 21 % de cada producto y devolver solo la lista de precios finales (float con 2 decimales no es obligatorio).

```python
carrito = [
    {'producto': 'Camiseta', 'precio': 20},
    {'producto': 'Sudadera', 'precio': 45},
    {'producto': 'Zapatillas', 'precio': 89.99},
    {'producto': 'Calcetines', 'precio': 8}
]

```

**Salida esperada:**

```python
[24.2, 54.45, 108.79, 9.68]

```

### **✅ Ejercicio 8: Total de productos en oferta**

Usa `reduce()` y `lambda` para calcular el importe total solo de los productos que están en oferta (campo `'oferta': True`).

```python
from functools import reduce
tienda = [
    {'nombre': 'Teclado mecánico', 'precio': 120, 'oferta': True},
    {'nombre': 'Webcam', 'precio': 75, 'oferta': False},
    {'nombre': 'Silla gamer', 'precio': 299, 'oferta': True},
    {'nombre': 'Alfombrilla', 'precio': 15, 'oferta': True}
]

```

**Salida esperada:**

```python
434

```

### **✅ Ejercicio 9: Suma de cubos de números impares**

Filtra los números impares, eleva cada uno al cubo y suma todos los resultados (usa las tres funciones encadenadas).

```python
from functools import reduce

numeros = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```

**Salida esperada:**

```python
1225    # 1³ + 3³ + 5³ + 7³ + 9³ = 1 + 27 + 125 + 343 + 729

```

### **✅ Ejercicio 10: Frase más larga en mayúsculas separada por puntos**

Filtra las palabras que tengan 5 o más letras, conviértelas a mayúsculas y únelas con un punto (`.`).

```python
from functools import reduce

texto = ['Python', 'es', 'muy', 'poderoso', 'en', 'una', 'sola', 'línea', 'wey']

```

**Salida esperada:**

```python
"PYTHON.PODEROSO.LÍNEA"

```