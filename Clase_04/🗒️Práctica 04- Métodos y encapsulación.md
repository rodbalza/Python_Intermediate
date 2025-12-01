# 🗒️Práctica 04- Métodos y encapsulación

> **Instrucciones de entrega:
Todos tus códigos deben ir en un notebook o en [ficheros.py](http://ficheros.py) comprimidos en .zip**
> 

## 📝 Ejercicio 1: Catálogo de Libros

Diseña una clase `Libro` con atributos `titulo`, `autor` y `anio`.

Crea otra clase `Biblioteca` que pueda:
- Agregar libros.
- Mostrar todos los libros usando `__str__`.
- Buscar un libro por título.

Crea al menos 3 instancias de `Libro` y prueba todas las funcionalidades de `Biblioteca`.

---

## 📝 Ejercicio 2: Sistema de Usuarios con Atributos Privados

Implementa una clase `Usuario` que tenga:
- Un atributo privado `__password` y un atributo público `username`.
- Métodos públicos para:
- Cambiar la contraseña de forma segura.
- Verificar si una contraseña dada coincide con la almacenada.

Demuestra su uso creando al menos 2 usuarios y probando las verificaciones.

---

## 📝 Ejercicio 3: Lista de Reproducción Musical

Crea una clase `Cancion` con los atributos `titulo` y `artista`.

Crea una clase `ListaReproduccion` que tenga:
- Un método para agregar canciones.
- Un método para mostrar todas las canciones.
- Un método para eliminar una canción por título.

Prueba la lista con 4 canciones, elimina una y muestra el resultado.

---

## 📝 Ejercicio 4: Control de Temperatura de un Dispositivo

Define una clase `SensorTemperatura` con:
- Un atributo privado `__temperatura` (valor inicial 20).
- Métodos públicos para:
- Aumentar la temperatura.
- Disminuir la temperatura.
- Mostrar la temperatura actual.

Simula varios aumentos y disminuciones y muestra la temperatura final.

---

## 📝 Ejercicio 5: Encapsulación en un Cajero Automático

Diseña una clase `Cajero` que tenga:
- Un atributo privado `__saldo` inicializado en 0.
- Métodos públicos para:
- Depositar dinero.
- Retirar dinero (solo si hay suficiente saldo).
- Consultar el saldo disponible.

Crea una instancia de `Cajero`, realiza varias operaciones de depósito y retiro, y verifica que la encapsulación funcione correctamente.

---

## 📝 Ejercicio 6: Juego de Dados

Crea una clase `Dado` que simule un dado de 6 caras.
- Implementa un método `lanzar()` que devuelva un número aleatorio entre 1 y 6.

Luego, crea una clase `JuegoDados` que:
- Tenga dos objetos `Dado`.
- Permita lanzar ambos dados.
- Muestre el resultado de cada dado y la suma total.

Simula 3 turnos de lanzamiento.

---

## 📝 Ejercicio 7: Juego de Adivinar un Número

Implementa una clase `JuegoAdivinaNumero` que:
- Tenga un atributo privado `__numero_secreto` generado aleatoriamente entre 1 y 20.
- Tenga un método `adivinar(numero)` que:
- Compare el número dado con el secreto.
- Devuelva un mensaje indicando si es mayor, menor o correcto.

Crea una instancia del juego y simula al menos 5 intentos de adivinanza mostrando los resultados.

## 📝 Ejercicio 8: Libre elección

Implementa un ejemplo código de tu temática preferida donde apliques encapsulación.