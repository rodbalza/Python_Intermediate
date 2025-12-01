# 🐍 Clase 05 - Herencia y Polimorfismo

# Parte 01 - Herencia

## Convención sobre nombres de identificadores.

<aside>
<img src="https://www.notion.so/icons/laptop_green.svg" alt="https://www.notion.so/icons/laptop_green.svg" width="40px" />

(a) Todas las variables y funciones que no pertenecen a una clase comienzan con un alfabeto en minúsculas. Ejemplo: real, imag, nombre, edad, salario, muestra(), sumar().

(b) Para las variables que deben utilizarse y luego descartarse utilice _ *. Ejemplo: for _ in [10, 20, 30, 40] : print(*).

(c) Los nombres de clases deben empezar con mayúsculas. Ejemplo: Empleado, Fruta, Complejo, Herramienta, Máquina.

(d) Identificadores privados, es decir, identificadores a los que queremos que sólo se pueda acceder desde dentro de la clase en la que están declarados deben empezar con dos guiones bajos a la izquierda. Ejemplo: __nombre, __edad, __get_errors()

(e) Identificadores protegidos, es decir, identificadores a los que queremos que sólo se pueda acceder desde dentro de la clase en la que están declarados o desde las clases que derivan de la clase padre (utilizando un concepto llamado herencia) deben empezar con un guión bajo inicial. Ejemplos: _dirección, _mantener_altura()

(f) Identificadores públicos, es decir, identificadores a los que queremos que sólo se pueda acceder desde dentro de la clase o desde fuera de ella - Empezar con minúscula. Ejemplo: neighbour, displayheight().

(g) Los métodos especiales definidos por el lenguaje empiezan y terminan con dos guiones bajos. Ejemplos: `__**init**()__`, `__**del__**`.

</aside>

## 📌 Llamada a funciones y métodos

Considere el programa dado a continuación. Contiene una función global llamada `imprimelo()` que no pertenece a ninguna clase, un método de instancia llamado `display()` y un método de clase llamado `mostrar()`.

```python
def imprimelo(): # Funcion global
		    print('Imprimiendo')

class Mensaje:

    def display(self, msg): # Método de instancia
        imprimelo()
        print(msg)

    def mostrar(): # Método de clase
        imprimelo()
        print('Hello')
        # display() # Esta llamada resultará en un error
 
    

imprimelo() # Llamada a la función global
m = Mensaje()
m.display('Buenos días') # Llamando al método de instancia
Mensaje.mostrar() # Llamando al método de clase
```

**Salida esperada:**

```
Imprimiendo
Imprimiendo
Buenos días
Imprimiendo
Hello
```

## 📌 Diferencia entre métodos de instancia y métodos de clase

En la programación orientada a objetos, existen **3 tipos de métodos**:

- Métodos de instancia, que pertenecen a la instancia.
- Métodos de clase, que pertenecen a la clase.
- Métodos estáticos, que en realidad no pertenecen a nada.

```python
class Perro:
    def metodo_instancia(self): # Método de Instancia
        pass

    @classmethod         # Decorador
    def metodo_clase(cls):   # Método de clase
        pass

    @staticmethod        # Decorador
    def metodo_estatico(a, b): # Método Estático
        pass

dog = Perro()
dog.metodo_instancia() # Llamando al método de instancia
Perro.metodo_clase()     # Llamando al método de clase
Perro.metodo_estatico(3, 4) # Llamando al método estático
```

Salida:

```python

```

---

<aside>
<img src="https://www.notion.so/icons/laptop_green.svg" alt="https://www.notion.so/icons/laptop_green.svg" width="40px" />

### Ejemplo para probar y modificar

```python
class Coche:
    # Atributo de Clase: pertenece a la clase y es compartido por todas las instancias
    cantidad_coches_creados = 0
    ruedas = 4

    def __init__(self, marca, modelo):
        # Atributos de Instancia: únicos para cada objeto (instancia)
        self.marca = marca
        self.modelo = modelo
        Coche.cantidad_coches_creados += 1 # Cada vez que se crea un coche, aumentamos el contador

    # 1. Método de Instancia
    #    - Recibe 'self' como primer argumento, que se refiere a la instancia actual del objeto.
    #    - Puede acceder y modificar atributos específicos de esa instancia (self.marca, self.modelo).
    #    - También puede acceder a atributos de clase (Coche.cantidad_coches_creados), pero no modificarlos de forma que afecte a todas las instancias (a menos que uses Coche.atributo_clase).
    def mostrar_info(self):
        print(f"\n--- Info del Coche ---")
        print(f"Marca: {self.marca}")
        print(f"Modelo: {self.modelo}")
        print(f"Ruedas: {Coche.ruedas} (Todos los coches tienen {Coche.ruedas} ruedas)")
        print(f"Este coche es la instancia #{Coche.cantidad_coches_creados} creada.")

    # 2. Método de Clase
    #    - Se define con el decorador @classmethod.
    #    - Recibe 'cls' como primer argumento, que se refiere a la clase misma (Coche).
    #    - Puede acceder y modificar atributos de clase (cls.cantidad_coches_creados, cls.ruedas).
    #    - NO puede acceder directamente a atributos de instancia (self.marca, self.modelo) sin una instancia específica.
    @classmethod
    def contar_coches(cls):
        print(f"\n--- Método de Clase: Contar Coches ---")
        print(f"Total de coches creados hasta ahora: {cls.cantidad_coches_creados}")
        # cls.ruedas = 5 # ¡Podríamos cambiar esto y afectaría a todos los coches! (no lo haremos aquí para no confundir)
        # print(f"Intentando acceder a la marca de un coche (esto NO funciona): {cls.marca}") # Error: no tiene 'self'

    # 3. Método Estático
    #    - Se define con el decorador @staticmethod.
    #    - NO recibe 'self' ni 'cls' como primer argumento.
    #    - No tiene acceso directo ni a atributos de instancia ni a atributos de clase.
    #    - Es básicamente una función normal que vive dentro de la clase para una mejor organización lógica, pero no depende de la clase o de la instancia para operar.
    @staticmethod
    def verificar_seguridad(velocidad):
        print(f"\n--- Método Estático: Verificación de Seguridad ---")
        if velocidad > 120:
            print(f"Alerta: ¡Velocidad ({velocidad} km/h) es excesiva! Conduzca con precaución.")
            return False
        else:
            print(f"Velocidad ({velocidad} km/h) segura. Continúe.")
            return True

# --- Demostración ---

print("--- Creando instancias de coches ---")
coche1 = Coche("Toyota", "Corolla")
coche2 = Coche("Ford", "Focus")
coche3 = Coche("Tesla", "Model 3")

# --- Llamando a Métodos de Instancia ---
# Se llaman a través de una instancia del objeto.
print("\n--- Llamando a métodos de instancia ---")
coche1.mostrar_info()
coche2.mostrar_info()

# --- Llamando a Métodos de Clase ---
# Se llaman a través de la CLASE misma (aunque también se pueden llamar a través de una instancia, es menos común).
print("\n--- Llamando a métodos de clase ---")
Coche.contar_coches() # Lo más común es llamarlo así
coche3.contar_coches() # También funciona, pero internamente sigue usando la clase

# --- Llamando a Métodos Estáticos ---
# Se llaman a través de la CLASE misma (o una instancia, pero no usan los datos de la instancia).
print("\n--- Llamando a métodos estáticos ---")
Coche.verificar_seguridad(80)
Coche.verificar_seguridad(150)

# Un método estático no necesita una instancia para funcionar.
# Por ejemplo, podemos llamarlo incluso si no se ha creado ningún coche (aunque en este ejemplo ya hemos creado algunos).
print("\n--- Método estático sin instancias ---")
Coche.verificar_seguridad(100)
```

Salida:

```
--- Creando instancias de coches ---

--- Llamando a métodos de instancia ---

--- Info del Coche ---
Marca: Toyota
Modelo: Corolla
Ruedas: 4 (Todos los coches tienen 4 ruedas)
Este coche es la instancia #3 creada.

--- Info del Coche ---
Marca: Ford
Modelo: Focus
Ruedas: 4 (Todos los coches tienen 4 ruedas)
Este coche es la instancia #3 creada.

--- Llamando a métodos de clase ---

--- Método de Clase: Contar Coches ---
Total de coches creados hasta ahora: 3

--- Método de Clase: Contar Coches ---
Total de coches creados hasta ahora: 3

--- Llamando a métodos estáticos ---

--- Método Estático: Verificación de Seguridad ---
Velocidad (80 km/h) segura. Continúe.

--- Método Estático: Verificación de Seguridad ---
Alerta: ¡Velocidad (150 km/h) es excesiva! Conduzca con precaución.

--- Método estático sin instancias ---

--- Método Estático: Verificación de Seguridad ---
Velocidad (100 km/h) segura. Continúe.

```

</aside>

## 📌 Métodos de instancia

Son métodos que pertenecen a un objeto (instancia) y no a la clase. Los métodos de instancia **pueden acceder a los atributos del objeto utilizando el parámetro `self`**.

```python
class Dog():
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def bark(self): # Este es un método de instancia
        print("woof! my name is", self.name)

dog = Dog("fifi", 5)
dog.bark()
# woof! my name is fifi

```

Salida:

```
woof! my name is fifi
```

---

## 📌 Métodos de clase

Son métodos que pertenecen a una clase (no a una instancia). Utilizamos el decorador `@classmethod`. Los métodos de clase **pueden acceder a atributos de clase**, **pero no a atributos de instancia.**

```python
class Dog():
    num_dog_objects = 0
    def __init__(self, name, age):
        self.name = name
        self.age = age
        Dog.num_dog_objects += 1

    @classmethod # Este es un método de clase
    def increase_num_dog_objects(cls):
        cls.num_dog_objects += 1

dog1 = Dog("rocky", 4)
dog2 = Dog("fifi", 5)
dog3 = Dog("baaron", 6)
# print(Dog.num_dog_objects) # 3 Imprime error
Dog.increase_num_dog_objects() # calling our class method
print(Dog.num_dog_objects) # 4 Este funciond
```

Salida:

```
4
```

**Observación:** Utilizamos la propia clase `Dog` para llamar al método `increase_num_dog_objects`, ya que este método pertenece a la clase y no a un objeto (instancia).

---

## 📌 Método Estático

Los métodos estáticos no pertenecen ni a una instancia ni a una clase, y no tienen acceso ni a los atributos de instancia ni a los atributos de clase. Se definen mediante el decorador `@staticmethod`.

```python
class Dog:
    @staticmethod
    def add(a, b):
        return a + b

x = Dog.add(4, 5)
# x is now 9
print(x)
```

**Salida esperada:**

```
9
```

De cierta manera, los métodos estáticos realmente no tienen mucho que ver con la clase en sí, y podemos tratarlos como una función normal que existe en el espacio de nombres de la clase.

## 📌 Herencia

Es un mecanismo que permite crear nuevas clases como una extensión de otras clases ya definidas. La clase que se extiende (hija) hereda los métodos y atributos de la clase extendida (Padre), pudiendo sobrescribirlos y cambiar así su comportamiento.

Para indicar que una clase hereda de otra se usa el mismo nombre de la clase de la que se hereda entre paréntesis después del nombre de la nueva clase.

```python
class Mamífero:
    def __init__(self, sangre):
        self.sangre = sangre
    def tipo_sangre(self):
        print(self.sangre)

class Elefante(Mamífero):
    def tipo_sangre(self):
        print('El tipo de sangre es', self.sangre)
j = Elefante('Frío')
j.tipo_sangre()
```

**Salida esperada:**

```
El tipo de sangre es Frío
```

---

## 📌 Sobrescribir métodos y añadir atributos

Si se quiere especificar un nuevo atributo cuando se crea un objeto Elefante bastaría con escribir un nuevo método `__init__` para la clase que se ejecutaría en lugar del `__init__` heredado.

```python
class Mamífero:
    def __init__(self, sangre):
        self.sangre = sangre
    def tipo_sangre(self):
        print(self.sangre)
class Elefante(Mamífero):
    def __init__(self, sangre, sexo):
        self.sangre = sangre
        self.sexo = sexo
    def tipo_sangre(self):
        print('El tipo de sangre es', self.sangre)
j = Elefante('Frío', 'hembra')
j.sexo  # 'hembra'
```

**Salida esperada:**

```
'hembra'
```

---

## 📌 Ejemplo sin herencia

La herencia es la capacidad que tiene una clase de heredar los atributos y métodos de otra, algo que nos permite reutilizar código y hacer programas óptimos.

> **Ejemplo sin herencia:** Supongamos que nos piden diseñar una estructura para una tienda que vendía tres tipos de productos: adornos, alimentos y libros. Si partimos de una clase que contenga todos los atributos, quedaría más o menos así:
> 

```python
class Producto:
    def __init__(self, referencia, tipo, nombre,
                 pvp, descripcion, productor=None,
                 distribuidor=None, isbn=None, autor=None):
        self.referencia = referencia
        self.tipo = tipo
        self.nombre = nombre
        self.pvp = pvp
        self.descripcion = descripcion
        self.productor = productor
        self.distribuidor = distribuidor
        self.isbn = isbn
        self.autor = autor
adorno = Producto('000A', 'ADORNO', 'Vaso Adornado', 15,
                  'Vaso de porcelana con dibujos')
print(adorno)
print(adorno.tipo)
print(vars(adorno))
```

**Salida esperada:**

```
<__main__.Producto object at 0x7f7d2f34b610>
ADORNO
{'referencia': '000A', 'tipo': 'ADORNO', 'nombre': 'Vaso Adornado', 'pvp': 15, 'descripcion': 'Vaso de porcelana con dibujos', 'productor': None, 'distribuidor': None, 'isbn': None, 'autor': None}
```

## 📌 Superclases

La idea de la herencia es identificar una clase base (la superclase o clase Padre) con los atributos comunes y luego crear las demás clases heredando de ella (las subclases o clases hijas) extendiendo sus campos específicos. En nuestro caso esa clase Padre o Superclase sería el Producto en sí mismo:

```python
class Producto:
    def __init__(self, referencia, nombre, pvp, descripcion):
        self.referencia = referencia
        self.nombre = nombre
        self.pvp = pvp
        self.descripcion = descripcion

    def __str__(self):
        return f"REFERENCIA\t {self.referencia}\n" \
               f"NOMBRE\t\t {self.nombre}\n" \
               f"PVP\t\t {self.pvp}\n" \
               f"DESCRIPCIÓN\t {self.descripcion}\n"
```

---

## 📌 Subclases

Para heredar los atributos y métodos de una clase en otra sólo tenemos que pasarla entre paréntesis durante la definición:

```python
class Adorno(Producto):
    pass

adorno = Adorno(2034, "Vaso adornado", 15, "Vaso de porcelana")
print(adorno)
```

**Salida esperada:**

```
REFERENCIA    2034
NOMBRE        Vaso adornado
PVP           15
DESCRIPCIÓN   Vaso de porcelana
```

Como se puede apreciar es posible utilizar el comportamiento de una superclase sin definir nada en la subclase.

---

## 📌 Subclases con atributos adicionales

Respecto a las demás subclases como se añaden algunos atributos, podríamos definirlas de esta forma:

```python
class Alimento(Producto):
    productor = ""
    distribuidor = ""

    def __str__(self):
        return f"REFERENCIA\t {self.referencia}\n" \
               f"NOMBRE\t\t {self.nombre}\n" \
               f"PVP\t\t {self.pvp}\n" \
               f"DESCRIPCIÓN\t {self.descripcion}\n" \
               f"PRODUCTOR\t {self.productor}\n" \
               f"DISTRIBUIDOR\t {self.distribuidor}\n"

class Libro(Producto):
    isbn = ""
    autor = ""

    def __str__(self):
        return f"REFERENCIA\t {self.referencia}\n" \
               f"NOMBRE\t\t {self.nombre}\n" \
               f"PVP\t\t {self.pvp}\n" \
               f"DESCRIPCIÓN\t {self.descripcion}\n" \
               f"ISBN\t\t {self.isbn}\n" \
               f"AUTOR\t\t {self.autor}\n"
```

---

## 📌 Usar las subclases

Ahora para utilizarlas simplemente tendríamos que establecer los atributos después de crear los objetos:

```python
alimento = Alimento(2035, "Botella de Aceite de Oliva", 5, "250 ML")
alimento.productor = "La Aceitera"
alimento.distribuidor = "Distribuciones SA"
print(alimento)

libro = Libro(2036, "Cocina Mediterránea", 9, "Recetas sanas y buenas")
libro.isbn = "0-123456-78-9"
libro.autor = "Doña Juana"
print(libro)
```

**Salida esperada:**

```
REFERENCIA    2035
NOMBRE        Botella de Aceite de Oliva
PVP           5
DESCRIPCIÓN   250 ML
PRODUCTOR     La Aceitera
DISTRIBUIDOR  Distribuciones SA

REFERENCIA    2036
NOMBRE        Cocina Mediterránea
PVP           9
DESCRIPCIÓN   Recetas sanas y buenas
ISBN          0-123456-78-9
AUTOR         Doña Juana
```

<aside>
<img src="https://www.notion.so/icons/laptop_green.svg" alt="https://www.notion.so/icons/laptop_green.svg" width="40px" />

Ejercicio 1 . Tomando como referencia el ejemplo anterior crear una Superclase llamada Vehículo con sus respectivos atributos comunes. Luego crear al menos tres subclases (con sus atributos adicionales) del tipo: Coche, Moto, Camión, Autobús. Crear por lo menos un objeto de cada subclase y muestra en pantalla los resultados.

</aside>

<aside>
<img src="https://www.notion.so/icons/laptop_green.svg" alt="https://www.notion.so/icons/laptop_green.svg" width="40px" />

Ejercicio 2.  Modifica el **código del ejercicio de refuerzo 3** de la clase 4 [](https://www.notion.so/Clase-69-M-todos-y-Encapsulaci-n-2161a81bbef4805282accae7df8d616c?pvs=21) utilizando esta vez herencias superclases y subclases.

</aside>

## Trabajando en conjunto

> Gracias a la flexibilidad de Python podemos manejar objetos de distintas clases masivamente de una forma muy simple. Vamos a empezar creando una lista con nuestros tres productos de subclases distintas:
> 

```python
productos = [adorno, alimento]
productos.append(libro)

print(productos)
```

Salida:

```python
[<__main__.Adorno object at 0x0000027EB304D150>, <__main__.Alimento object at 0x0000027EB3071F10>, <__main__.Libro object at 0x0000027EB3022150>]
```

Ahora si queremos recorrer todos los productos de la lista podemos usar un bucle `for`.

```python
for producto in productos:
    print(producto, "\n")
```

Salida:

```
REFERENCIA      2034
NOMBRE          Vaso adornado
PVP             15
DESCRIPCIÓN     Vaso de porcelana

REFERENCIA      2035
NOMBRE          Botella de Aceite de Oliva
PVP             5
DESCRIPCIÓN     250 ML
PRODUCTOR       La Aceitera
DISTRIBUIDOR    Distribuciones SA

REFERENCIA      2036
NOMBRE          Cocina Mediterránea
PVP             9
DESCRIPCIÓN     Recetas sanas y buenas
ISBN            0-123456-78-9
AUTOR           Doña Juana
```

También podemos acceder a los atributos, siempre que sean compartidos entre todos los objetos:

```python
for producto in productos:
    print(producto.referencia, producto.nombre)
```

Salida:

```
2034 Vaso adornado
2035 Botella de Aceite de Oliva
2036 Cocina Mediterránea
```

Si un objeto no tiene el atributo al que queremos acceder nos dará error:

```
for producto in productos:
    print(producto.autor)
```

Salida:

```
AttributeError
Cell In[7], line 2
----> 2     print(producto.autor)

AttributeError: 'Adorno' object has no attribute 'autor'
```

Por suerte podemos hacer una comprobación con la función `isinstance()` para determinar si una instancia es de una determinada clase y así mostrar unos atributos u otros:

```python
for producto in productos:
    if isinstance(producto, Adorno):
        print(producto.referencia, producto.nombre)
    elif isinstance(producto, Alimento):
        print(producto.referencia, producto.nombre, producto.productor)
    elif isinstance(producto, Libro):
        print(producto.referencia, producto.nombre, producto.isbn)
```

Salida:

```
2034 Vaso adornado
2035 Botella de Aceite de Oliva La Aceitera
2036 Cocina Mediterránea 0-123456-78-9
```

# Parte 02 - Polimorfismo

## Polimorfismo.

> El polimorfismo es una propiedad de la herencia por la que objetos de distintas subclases pueden responder a una misma acción. La polimorfia es implícita en Python, ya que todas las clases son subclases de una superclase común llamada `Object`. Por ejemplo, la siguiente función aplica una rebaja al precio de un producto:
> 

```python
def rebajar_producto(producto, rebaja):
    producto.pvp = producto.pvp - (producto.pvp/100 * rebaja)
```

Gracias al polimorfismo no tenemos que comprobar si un objeto tiene o no el atributo `pvp`, simplemente intentamos acceder y si existe premio:

```python
print(alimento, "\n")
rebajar_producto(alimento, 10)
print(alimento)
```

Salida:

```
REFERENCIA      2035
NOMBRE          Botella de Aceite de Oliva
PVP             5
DESCRIPCIÓN     250 ML
PRODUCTOR       La Aceitera
DISTRIBUIDOR    Distribuciones SA

REFERENCIA      2035
NOMBRE          Botella de Aceite de Oliva
PVP             4.5
DESCRIPCIÓN     250 ML
PRODUCTOR       La Aceitera
DISTRIBUIDOR    Distribuciones SA
```

## Herencia Múltiple

> La herencia múltiple es la capacidad de una subclase de heredar de múltiples superclases. Esto conlleva un problema, y es que, si varias superclases tienen los mismos atributos o métodos, la subclase sólo podrá heredar de una de ellas. En estos casos Python dará prioridad a las clases más a la izquierda en el momento de la declaración de la subclase:
> 

```python
class A:
    def __init__(self):
        print("Soy de clase A")

class B:
    def __init__(self):
        print("Soy de clase B")

class C(A,B):
    pass

c = C()
```

Salida:

```python
Soy de clase A
```

Si cambiamos la "B" por la "A":

```python
class A:
    def __init__(self):
        print("Soy de clase A")

class B:
    def __init__(self):
        print("Soy de clase B")

class C(B,A):
    pass

c = C()
```

Salida:

```python
Soy de clase B
```

 Para comprobar cómo se heredan todos los métodos de las superclases, podemos añadir algunos métodos específicos de clase, por ejemplo:

```python
class A:
    def __init__(self):
        print("Soy de clase A")
    def a(self):
        print("Este método lo heredo de A")

class B:
    def __init__(self):
        print("Soy de clase B")
    def b(self):
        print("Este método lo heredo de B")

class C(B,A):
    def c(self):
        print("Este método es de C")

c = C()
c.a()
```

Salida:

```python
Soy de clase B
Este método lo heredo de A
```

Por ejemplo

```python
c.b()
```

Salida:

```python
Este método lo heredo de B
```

otro ejemplo:

```python
c.c()
```

Salida:

```python
Este método es de C
```

Como se puede observar, utilizando la herencia múltiple podemos gestionar atributos y métodos heredados de varias clases a la vez.

<aside>
<img src="https://www.notion.so/icons/laptop_green.svg" alt="https://www.notion.so/icons/laptop_green.svg" width="40px" />

**Ejercicio 3**. *Ejemplos resueltos para revisar, corregir y modificar añadiendo nuevos atributos, métodos y herencias.*

### 3.1.  Polimorfismo: El "Botón Mágico”

Imagina que tienes un botón mágico que, al presionarlo, hace que diferentes objetos hagan su "sonido" característico. Un perro ladra, un gato maúlla y un pato cuaquea. No tienes que saber de antemano qué animal es, ¡el botón simplemente le dice "haz tu sonido" y cada uno lo hace a su manera!

En programación, el **polimorfismo** significa que objetos de diferentes tipos pueden responder a la misma "orden" (o método) de maneras diferentes, pero adecuadas a su propio tipo.

**Concepto Clave:** Tener un método con el mismo nombre en diferentes clases, y cada clase lo implementa a su manera.

```python
# Nuestro plano general para cualquier animal
class Animal:
    def hacer_sonido(self):
        # Este es un sonido genérico, cada animal lo cambiará
        pass

# Nuestro plano para perros
class Perro(Animal): # Perro es un tipo de Animal
    def hacer_sonido(self):
        return "¡Guau! ¡Guau!"

# Nuestro plano para gatos
class Gato(Animal): # Gato es un tipo de Animal
    def hacer_sonido(self):
        return "¡Miau! ¡Miau!"

# Nuestro plano para patos
class Pato(Animal): # Pato es un tipo de Animal
    def hacer_sonido(self):
        return "¡Cuac! ¡Cuac!"

# --- Demostración del Polimorfismo ---

# Creamos algunos animales (instancias)
mi_perro = Perro()
mi_gato = Gato()
mi_pato = Pato()

# Los ponemos todos en una lista, ¡sin importar el tipo exacto!
animales_en_el_zoo = [mi_perro, mi_gato, mi_pato]

print("--- El botón mágico para hacer sonidos ---")

# Ahora, recorremos la lista y le decimos a cada animal: "¡Haz tu sonido!"
# No necesitamos saber si es un perro, gato o pato.
# Cada uno sabe cómo responder a 'hacer_sonido'.
for animal in animales_en_el_zoo:
    print(f"El animal hace: {animal.hacer_sonido()}")

print("\n--- Salida Esperada ---")
print("El animal hace: ¡Guau! ¡Guau!")
print("El animal hace: ¡Miau! ¡Miau!")
print("El animal hace: ¡Cuac! ¡Cuac!")
```

> **¿Por qué es útil el Polimorfismo?**
> 
> - **Código más limpio:** No tienes que escribir `if es_perro: haz_guau() elif es_gato: haz_miau()`. Simplemente dices `animal.hacer_sonido()`.
> - **Fácil de expandir:** Si mañana añades una clase `Vaca` con su `hacer_sonido()`, tu "botón mágico" (el bucle `for`) seguirá funcionando sin cambios.

## 3.2. Herencia Múltiple: El "Súper Héroe con Doble Poder”

Imagina que quieres crear un nuevo súper héroe que tiene poderes de dos súper héroes diferentes. Por ejemplo, quieres un héroe que pueda volar (como Superman) y que pueda lanzar telarañas (como Spiderman).

La **herencia múltiple** en Python permite que una clase (tu nuevo súper héroe) herede características (atributos y métodos) de *más de una* clase "padre" (Superman y Spiderman).

```python
# Clase Padre 1: Héroe Volador
class HeroeVolador:
    def __init__(self):
        print("Poder: Capacidad de Volar!")

    def volar(self):
        return "Estoy volando por el cielo."

# Clase Padre 2: Héroe Lanzador de Telarañas
class HeroeTelarana:
    def __init__(self):
        print("Poder: Capacidad de Lanzar Telarañas!")

    def lanzar_telarana(self):
        return "¡Zas! Telaraña lanzada."

# Clase Hija: Nuestro Súper Héroe con doble poder
# Hereda de HeroeVolador Y HeroeTelarana
class SúperHeroeCombinado(HeroeVolador, HeroeTelarana):
    def __init__(self, nombre):
        # Llamar a los constructores de las clases padre
        # En herencia múltiple, se llama el constructor de la primera clase listada
        super().__init__() # Llama al __init__ de HeroeVolador en este caso
        HeroeTelarana.__init__(self) # Debemos llamar explícitamente al otro constructor si tiene uno.

        self.nombre = nombre
        print(f"¡{self.nombre} ha nacido con poderes combinados!")

    def presentarse(self):
        return f"Hola, soy {self.nombre} y tengo poderes de volar y lanzar telarañas."

# --- Demostración de la Herencia Múltiple ---

# Creamos una instancia de nuestro Súper Héroe Combinado
mi_super_heroe = SúperHeroeCombinado("Capitán Combinado")

print(f"\n{mi_super_heroe.presentarse()}")
print(f"¡Mira, puede! {mi_super_heroe.volar()}")
print(f"¡Y también puede! {mi_super_heroe.lanzar_telarana()}")

print("\n--- Salida Esperada ---")
print("Poder: Capacidad de Volar!")
print("Poder: Capacidad de Lanzar Telarañas!")
print("¡Capitán Combinado ha nacido con poderes combinados!")
print("Hola, soy Capitán Combinado y tengo poderes de volar y lanzar telarañas.")
print("¡Mira, puede! Estoy volando por el cielo.")
print("¡Y también puede! ¡Zas! Telaraña lanzada.")
```

**¿Por qué es útil la Herencia Múltiple?**

- **Reutilización de código:** Puedes tomar características de varias clases existentes y combinarlas en una nueva.
- **Modelos complejos:** Para modelar situaciones donde un objeto realmente necesita ser "algo de varios tipos" al mismo tiempo.
</aside>