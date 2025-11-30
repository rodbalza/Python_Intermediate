# 🐍Practica 01 - Comprehensions

# Parte 01 - Comprehensions

## **1.1**

> Usa list comprehension para generar una lista de números en el rango 2 a 50 que sean divisibles por 2 y por 4.
> 

## **1.2**

> Aplana la siguiente lista usando list comprehension:
> 

```python
mat = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
```

## **1.3**

> Crea un conjunto con números aleatorios entre 15 y 45.
> 
> - Cuenta cuántos son **menores que 30**.
> - Elimina todos los números **menores que 30**.

## **1.4**

> Usa list comprehension para eliminar tuplas vacías de una lista de tuplas.
> 

```python
# Ejemplo de entrada
lista = [(1, 2), (), (3, 4), (), (5,), (6, 7, 8), ()]
```

## **1.5**

> Dada una cadena, divídela por espacios, capitaliza cada palabra y únelas de nuevo en una cadena. Usa list comprehension.
> 

```python
python = "Es un lenguaje de progrmación muy popular en ML , NN, IA y GenAI"
```

## **1.6**

> Dado un diccionario complejo con claves de tipo string (nombres de productos con espacios, números y símbolos), crea un nuevo diccionario donde:
> 
- Las claves sean **solo letras y números**, **en minúsculas**, **sin espacios ni vocales**
- Los valores sean **tuplas** con: (precio, stock, categoría)
- **Filtra** solo productos con stock > 0 y precio > 100

```python
# Diccionario 
inventario = {
    "Laptop Dell XPS-15": {"precio": 1200, "stock": 5, "cat": "Electrónica"},
    "Mouse RGB 2024!": {"precio": 45, "stock": 0, "cat": "Periféricos"},
    "Teclado Mecánico Pro": {"precio": 180, "stock": 12, "cat": "Periféricos"},
    "Monitor 4K Ultra": {"precio": 450, "stock": 3, "cat": "Electrónica"},
    "Auriculares BT-500": {"precio": 95, "stock": 8, "cat": "Audio"},
    "Webcam HD Plus+": {"precio": 110, "stock": 0, "cat": "Periféricos"},
    "SSD 1TB NVMe": {"precio": 130, "stock": 20, "cat": "Almacenamiento"}
}
```

## **1.7**

> Suma dos matrices 3 × 4 usando:
(a) listas anidadas
(b) list comprehension
> 

```python
mat1 = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
mat2 = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
mat3 = [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
```

## **1.8**

> Dado el diccionario de empleados:
> 

```python
emp = {
    'A101': {'name': 'Ashish', 'age': 30, 'salary': 21000},
    'B102': {'name': 'Dinesh', 'age': 25, 'salary': 12200},
    'A103': {'name': 'Ramesh', 'age': 28, 'salary': 11000},
    'D104': {'name': 'Akheel', 'age': 30, 'salary': 18000},
    'A105': {'name': 'Akaash', 'age': 32, 'salary': 20000}
}
```

> Usa dictionary comprehension para crear:
> 
1. Diccionario con códigos que **empiecen con 'A'**
2. Diccionario con **código y nombre**
3. Diccionario con **código y edad**
4. Diccionario con **código y edad > 30**
5. Diccionario con **código y nombre** donde el nombre empiece con 'A'
6. Diccionario con **código y salario** en rango [13000, 20000]

# Parte 2 - Programación Funcional - lambda

### 7️⃣ Ejercicio 1.9:

 Supón que un diccionario contiene pares clave-valor, donde la clave es una letra del alfabeto y el valor es un número. Escribe un programa que obtenga los valores máximo y mínimo del diccionario usando las funciones max y min en combinación con lambda.

### 8️⃣ Ejercicio 1.10:

**Usando `lambda`, `map()`, `filter()` y `reduce()` o una combinación de los mismos para realizar las siguientes tareas:**

(a) Supón que un diccionario contiene el tipo de mascota (gato, perro, etc.), el nombre de la mascota y la edad de la mascota. Escribe un programa que obtenga la suma de todas las edades de los perros.

(b) Considera la siguiente lista:
`lst = [1.25, 3.22, 4.68, 10.95, 32.55, 12.54]`

Los números en la lista representan los radios de círculos. Escribe un programa para obtener una lista de las áreas de estos círculos redondeadas a dos decimales.

(c) Considera las siguientes listas:
`nums = [10, 20, 30, 40, 50, 60, 70, 80]`

`strs = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']`
Escribe un programa para obtener una lista de tuplas, donde cada tupla contenga un número de una lista y una cadena de otra, en el mismo orden en que aparecen en las listas originales.

(d) Supón que un diccionario contiene nombres de estudiantes y las calificaciones obtenidas por ellos en un examen. Escribe un programa para obtener una lista de los estudiantes que obtuvieron más de 40 puntos en el examen.

(e) Considera la siguiente lista:
`lst = ['Malayalam', 'Drawing', 'madamlamadam', '1234321']`
Escribe un programa para imprimir aquellas cadenas que son palíndromos.

(f) Una lista contiene nombres de empleados. Escribe un programa para filtrar aquellos nombres cuya longitud sea mayor a 8 caracteres.

(g) Un diccionario contiene la siguiente información sobre 5 empleados:

- Nombre
- Apellido
- Edad
- Grado (Experto, Semiexperto, Altamente experto)
Escribe un programa para obtener una lista de empleados (nombre + apellido) que sean Altamente expertos.

(h) Considera la siguiente lista:
`lst = ['Benevolent', 'Dictator', 'For', 'Life']`
Escribe un programa para obtener una cadena 'Benevolent Dictator For Life'.

(i) Considera la siguiente lista de estudiantes en una clase.
`lst = ['Rahul', 'Priya', 'Chaaya', 'Narendra', 'Prashant']`
Escribe un programa para obtener una lista en la que todos los nombres se conviertan a mayúsculas.