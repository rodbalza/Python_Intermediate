# 🗒️Práctica 03 - Clases y Objetos

### **Ejercicio 1 – Clase básica con métodos**

Crea una clase llamada **Cancion** que represente canciones:

1. Define un método `set_data(self, titulo, artista, duracion)` para asignar valores.
2. Define un método `mostrar(self)` que imprima:`Título: [titulo] - Artista: [artista] - Duración: [duracion] min`.
3. Crea **3 objetos** con datos distintos y muestra la información.

---

### **Ejercicio 2 – Del mundo real a clases**

1. Elige **3 clases del mundo real** (ej.: Avión, Película, Restaurante).
2. Para cada clase indica:
    - **3 objetos** (instancias).
    - **3 atributos** (ej.: modelo, capacidad, color).
    - **2 métodos** (ej.: despegar(), aterrizar()).

---

### **Ejercicio 3 – Inicialización con `__init__`**

Diseña una clase **Curso**:

1. Usa un constructor `__init__(self, nombre, duracion, nivel)`.
2. Crea una lista con **4 objetos** de tipo Curso.
3. Recorre la lista mostrando:`Curso: [nombre] | Duración: [duracion] | Nivel: [nivel]`.

---

### **Ejercicio 4 – Contador con variable de clase**

Crea una clase **Vehiculo**:

1. Define una **variable de clase** `total_vehiculos = 0`.
2. En `__init__(self, marca, tipo)` incrementa el contador.
3. Crea un método de clase `@classmethod contar(cls)` que devuelva:`"Total de vehículos: X"`.
4. Crea **5 objetos** y muestra el total usando la clase.

---

### **Ejercicio 5 – Uso de `__init__` y métodos de instancia**

Crea una clase **Pelicula**:

1. Constructor con atributos: `titulo`, `director`, `año`.
2. Método `descripcion()` que devuelva:`"Título: [titulo], Director: [director], Año: [año]"`.
3. Crea **3 películas** y muestra la descripción de cada una.

---

### **Ejercicio 6 – Explorando con `vars()` y `dir()`**

Crea una clase **Mascota**:

1. Variable de clase: `es_domestico = True`.
2. Constructor con atributos: `nombre`, `especie`.
3. Método `descripcion()` que devuelva:`"Soy [nombre], un [especie]"`.
4. Crea un objeto y usa:
    - `print(vars(Mascota))`
    - `print(dir(Mascota))`
    - `print(vars(objeto))`
    - `print(dir(objeto))`

---

### **Ejercicio 7 – Clase con lista interna**

Crea una clase **Playlist**:

1. Constructor que inicialice una lista vacía `self.canciones`.
2. Método `agregar_cancion(titulo)` que añada a la lista.
3. Método `mostrar_playlist()` que imprima todas las canciones.
4. Crea un objeto y agrega al menos **5 canciones**.

---

### **Ejercicio 8 – Métodos de instancia y clase**

Crea una clase **Empleado**:

1. Variable de clase `total_empleados = 0`.
2. Constructor con `nombre` y `puesto`.
3. Método de instancia `presentarse()` que devuelva:`"Hola, soy [nombre], trabajo como [puesto]"`.
4. Método de clase `contar_empleados()` que muestre el total.
5. Crea **3 empleados** y prueba ambos métodos.

---

### **Ejercicio 9 – Uso de `__del__`**

Crea una clase **Sesion**:

1. Constructor que reciba `usuario`.
2. Método `__del__()` que imprima:`"Sesión de [usuario] cerrada"`.
3. Crea y elimina objetos para observar el comportamiento.

---

### **Ejercicio 10 – Modelando una clase compleja**

Crea una clase **LibroDigital**:

1. Atributos: `titulo`, `autor`, `precio`, `formato`.
2. Métodos:
    - `mostrar_info()`
    - `aplicar_descuento(porcentaje)`
3. Crea **3 objetos** y aplica descuentos diferentes.