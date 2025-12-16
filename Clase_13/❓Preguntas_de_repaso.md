# ❓Preguntas de repaso

---

## **Pregunta 1**

¿Cuál de las siguientes expresiones crea correctamente una lista usando comprensión de listas en Python?

a) `[x, y for x in range(5)]`

b) `(x for x in range(5))`

c) `[x**2 for x in range(5)]`

d) `{x: x**2 for x in range(5)}`

---

## **Pregunta 2**

¿Cuál de las siguientes expresiones genera una lista con los números pares del 0 al 10?

a) `[x for x in range(10) if x % 2 == 1]`

b) `[x*2 for x in range(5)]`

c) `(x for x in range(0, 11, 2))`

d) `{x for x in range(0, 11) if x % 2 == 0}`

---

## **Pregunta 3**

¿Cuál de las siguientes funciones de Python puede recibir otra función como argumento?

a) `input()`

b) `sorted()`

c) `type()`

d) `id()`

---

## **Pregunta 4**

¿Cuál de estas funciones permite aplicar una operación a cada elemento de un iterable?

a) `filter()`

b) `print()`

c) `range()`

d) `enumerate()`

---

## **Pregunta 5**

En una clase de Python, ¿qué método se ejecuta automáticamente al crear un objeto?

a) `__str__()`

b) `__init__()`

c) `__new__()`

d) `__repr__()`

---

## **Pregunta 6**

¿Cuál es el propósito principal del método `__init__` en una clase?

a) Mostrar información del objeto

b) Eliminar un objeto de memoria

c) Inicializar los atributos del objeto

d) Comparar dos objetos

---

## **Pregunta 7**

¿Qué se imprime al ejecutar la siguiente instrucción?

```python
print("Python".upper())
```

a) python

b) Python

c) PYTHON

d) Error

---

## **Pregunta 8**

¿Cuál de las siguientes opciones muestra correctamente la sintaxis para definir una clase y crear un objeto en Python?

a)

```python
class Persona():
    pass
p = Persona
```

b)

```python
class Persona:
    pass
p = Persona()
```

c)

```python
def Persona:
    pass
p = new Persona()
```

d)

```python
Persona class:
    pass
p = Persona
```

---

## **Pregunta 9**

Observa el siguiente código y selecciona la salida correcta:

```python
class Animal:
    def speak(self):
        return "Soy un animal"

class Perro(Animal):
    pass

p = Perro()
print(p.speak())
```

a) Error de herencia

b) Soy un animal

c) Soy un perro

d) None

---

## **Pregunta 10**

Dado el siguiente código, ¿cuál será la salida?

```python
class A:
    def mostrar(self):
        return "Clase A"

class B(A):
    def mostrar(self):
        return "Clase B"

obj = B()
print(obj.mostrar())
```

a) Clase A

b) Clase B

c) Error por sobrescritura

d) None

---

## **Pregunta 11**

Analiza el siguiente código y selecciona la salida correcta:

```python
class A:
    def mensaje(self):
        return "Mensaje A"

class B(A):
    pass

class C(B):
    pass

c = C()
print(c.mensaje())
```

a) Mensaje A

b) Mensaje B

c) Mensaje C

d) Error de herencia

---

## **Pregunta 12**

¿Cuál es la sintaxis correcta para acceder a un método de la clase padre desde una clase hija?

a)

```python
parent.metodo()
```

b)

```python
super().metodo()
```

c)

```python
this.metodo()
```

d)

```python
base.metodo()
```

---

## **Pregunta 13**

¿Qué comando permite guardar todas las dependencias instaladas en un proyecto Python?

a) `pip list > requirements.txt`

b) `pip export requirements.txt`

c) `pip freeze > requirements.txt`

d) `pip show all`

---

## **Pregunta 14**

¿Para qué se utiliza el archivo `requirements.txt`?

a) Para ejecutar scripts Python

b) Para definir variables de entorno

c) Para listar dependencias del proyecto

d) Para documentar el código

---

## **Pregunta 15**

¿Cuál de los siguientes comandos crea un entorno virtual usando `venv`?

a) `pip install venv`

b) `python -m venv entorno`

c) `virtualenv activate entorno`

d) `source entorno`

---

## **Pregunta 16**

¿Cómo se activa un entorno virtual en Windows usando PowerShell?

a) `source env/bin/activate`

b) `env activate`

c) `.\env\Scripts\activate`

d) `activate env`

---

## **Pregunta 17**

¿Cuál es la sintaxis correcta de una función lambda en Python?

a) `lambda x = x + 1`

b) `def lambda(x): return x + 1`

c) `f = lambda x: x + 1`

d) `lambda(x): return x + 1`

---

## **Pregunta 18**

¿En qué situación es más adecuado usar una función lambda?

a) Cuando se necesita una función compleja

b) Cuando la función se reutiliza muchas veces

c) Para operaciones simples y de corta duración

d) Para definir clases dinámicas