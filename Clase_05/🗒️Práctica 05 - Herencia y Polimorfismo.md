# 🗒️Práctica 05 - Herencia y Polimorfismo

<aside>
💡

### Ejercicios 1, 2 y 3 de la clase 5

</aside>

<aside>
<img src="https://www.notion.so/icons/database_red.svg" alt="https://www.notion.so/icons/database_red.svg" width="40px" />

### Ejercicio 4: Sistema de Gestión de Biblioteca

> Tienes el siguiente código estructurado que maneja una biblioteca simple:
> 

```python
# Código estructurado existente
libros = []
usuarios = []
prestamos = []

def agregar_libro(titulo, autor, isbn, disponible=True):
    libro = {
        'titulo': titulo,
        'autor': autor,
        'isbn': isbn,
        'disponible': disponible
    }
    libros.append(libro)

def registrar_usuario(nombre, email, telefono):
    usuario = {
        'id': len(usuarios) + 1,
        'nombre': nombre,
        'email': email,
        'telefono': telefono,
        'libros_prestados': []
    }
    usuarios.append(usuario)

def prestar_libro(isbn_libro, id_usuario):
    libro = buscar_libro_por_isbn(isbn_libro)
    usuario = buscar_usuario_por_id(id_usuario)
    
    if libro and usuario and libro['disponible']:
        libro['disponible'] = False
        usuario['libros_prestados'].append(isbn_libro)
        prestamo = {
            'isbn_libro': isbn_libro,
            'id_usuario': id_usuario,
            'fecha_prestamo': datetime.now(),
            'fecha_devolucion': None
        }
        prestamos.append(prestamo)
        return True
    return False

def devolver_libro(isbn_libro, id_usuario):
    for prestamo in prestamos:
        if (prestamo['isbn_libro'] == isbn_libro and 
            prestamo['id_usuario'] == id_usuario and 
            prestamo['fecha_devolucion'] is None):
            
            prestamo['fecha_devolucion'] = datetime.now()
            libro = buscar_libro_por_isbn(isbn_libro)
            usuario = buscar_usuario_por_id(id_usuario)
            
            if libro and usuario:
                libro['disponible'] = True
                usuario['libros_prestados'].remove(isbn_libro)
            return True
    return False

def buscar_libro_por_isbn(isbn):
    for libro in libros:
        if libro['isbn'] == isbn:
            return libro
    return None

def buscar_usuario_por_id(id_usuario):
    for usuario in usuarios:
        if usuario['id'] == id_usuario:
            return usuario
    return None

def mostrar_libros_disponibles():
    disponibles = [libro for libro in libros if libro['disponible']]
    for libro in disponibles:
        print(f"{libro['titulo']} - {libro['autor']} (ISBN: {libro['isbn']})")

def mostrar_prestamos_activos():
    activos = [p for p in prestamos if p['fecha_devolucion'] is None]
    for prestamo in activos:
        libro = buscar_libro_por_isbn(prestamo['isbn_libro'])
        usuario = buscar_usuario_por_id(prestamo['id_usuario'])
        print(f"Libro: {libro['titulo']} - Usuario: {usuario['nombre']}")
```

> Convierte este código estructurado en un diseño orientado a objetos. Debes crear las clases necesarias (`Libro`, `Usuario`, `Prestamo`, `Biblioteca`) con sus respectivos atributos y métodos. Considera los principios de encapsulación y responsabilidad única.
> 
</aside>

<aside>
<img src="https://www.notion.so/icons/database_red.svg" alt="https://www.notion.so/icons/database_red.svg" width="40px" />

### Ejercicio 5: Calculadora de Nómina

> El siguiente código estructurado calcula nóminas para diferentes tipos de empleados:
> 

```python
# Código estructurado existente
empleados = []

def agregar_empleado(nombre, id_empleado, tipo, salario_base, **kwargs):
    empleado = {
        'nombre': nombre,
        'id': id_empleado,
        'tipo': tipo,
        'salario_base': salario_base,
        'horas_extra': kwargs.get('horas_extra', 0),
        'ventas': kwargs.get('ventas', 0),
        'comision_rate': kwargs.get('comision_rate', 0),
        'proyectos_completados': kwargs.get('proyectos_completados', 0)
    }
    empleados.append(empleado)

def calcular_salario(id_empleado):
    empleado = buscar_empleado(id_empleado)
    if not empleado:
        return 0
    
    salario_total = empleado['salario_base']
    
    if empleado['tipo'] == 'tiempo_completo':
        # Cálculo para empleado de tiempo completo
        if empleado['horas_extra'] > 0:
            salario_total += empleado['horas_extra'] * (empleado['salario_base'] / 160) * 1.5
    
    elif empleado['tipo'] == 'vendedor':
        # Cálculo para vendedor
        comision = empleado['ventas'] * empleado['comision_rate']
        salario_total += comision
    
    elif empleado['tipo'] == 'freelancer':
        # Cálculo para freelancer
        bonus_proyecto = empleado['proyectos_completados'] * 500
        salario_total += bonus_proyecto
    
    return salario_total

def aplicar_descuentos(salario_bruto, id_empleado):
    empleado = buscar_empleado(id_empleado)
    descuentos = 0
    
    # Seguridad social (8%)
    descuentos += salario_bruto * 0.08
    
    # Impuesto sobre la renta (progressive)
    if salario_bruto > 50000:
        descuentos += (salario_bruto - 50000) * 0.15
    if salario_bruto > 100000:
        descuentos += (salario_bruto - 100000) * 0.05
    
    return salario_bruto - descuentos

def generar_nomina_completa():
    nominas = []
    for empleado in empleados:
        salario_bruto = calcular_salario(empleado['id'])
        salario_neto = aplicar_descuentos(salario_bruto, empleado['id'])
        
        nomina = {
            'empleado': empleado['nombre'],
            'tipo': empleado['tipo'],
            'salario_bruto': salario_bruto,
            'salario_neto': salario_neto,
            'descuentos': salario_bruto - salario_neto
        }
        nominas.append(nomina)
        
        print(f"Empleado: {nomina['empleado']}")
        print(f"Tipo: {nomina['tipo']}")
        print(f"Salario Bruto: ${nomina['salario_bruto']:,.2f}")
        print(f"Descuentos: ${nomina['descuentos']:,.2f}")
        print(f"Salario Neto: ${nomina['salario_neto']:,.2f}")
        print("-" * 40)
    
    return nominas

def buscar_empleado(id_empleado):
    for empleado in empleados:
        if empleado['id'] == id_empleado:
            return empleado
    return None
```

> Refactoriza este código a un diseño orientado a objetos. Crea una clase base `Empleado` y clases derivadas para cada tipo de empleado. Implementa los métodos de cálculo de salario de manera apropiada y considera cómo manejar los descuentos de forma elegante.
> 
</aside>

<aside>
<img src="https://www.notion.so/icons/database_red.svg" alt="https://www.notion.so/icons/database_red.svg" width="40px" />

### Ejercicio 6. Sistema de Transporte

> Tienes las siguientes clases independientes:
> 

```python
class Auto:
    def __init__(self, marca, modelo, año, combustible):
        self.marca = marca
        self.modelo = modelo
        self.año = año
        self.combustible = combustible
        self.encendido = False
    
    def encender(self):
        self.encendido = True
        print(f"Auto {self.marca} {self.modelo} encendido")
    
    def apagar(self):
        self.encendido = False
        print(f"Auto {self.marca} {self.modelo} apagado")
    
    def acelerar(self):
        if self.encendido:
            print(f"Auto acelerando con motor de {self.combustible}")
    
    def calcular_costo_viaje(self, distancia):
        costo_por_km = 0.15
        return distancia * costo_por_km

class Bicicleta:
    def __init__(self, marca, tipo, velocidades):
        self.marca = marca
        self.tipo = tipo
        self.velocidades = velocidades
        self.velocidad_actual = 1
    
    def cambiar_velocidad(self, nueva_velocidad):
        if 1 <= nueva_velocidad <= self.velocidades:
            self.velocidad_actual = nueva_velocidad
            print(f"Velocidad cambiada a {nueva_velocidad}")
    
    def pedalear(self):
        print(f"Pedaleando en velocidad {self.velocidad_actual}")
    
    def calcular_costo_viaje(self, distancia):
        return 0  # Las bicicletas no tienen costo de combustible

class Autobus:
    def __init__(self, numero_ruta, capacidad, empresa):
        self.numero_ruta = numero_ruta
        self.capacidad = capacidad
        self.empresa = empresa
        self.pasajeros = 0
        self.en_servicio = False
    
    def iniciar_servicio(self):
        self.en_servicio = True
        print(f"Autobús ruta {self.numero_ruta} en servicio")
    
    def finalizar_servicio(self):
        self.en_servicio = False
        self.pasajeros = 0
        print(f"Autobús ruta {self.numero_ruta} fuera de servicio")
    
    def subir_pasajeros(self, cantidad):
        if self.en_servicio and self.pasajeros + cantidad <= self.capacidad:
            self.pasajeros += cantidad
            print(f"{cantidad} pasajeros subieron. Total: {self.pasajeros}")
    
    def calcular_costo_viaje(self, distancia):
        costo_por_km = 0.05  # Más eficiente por pasajero
        return distancia * costo_por_km * self.pasajeros

class Taxi:
    def __init__(self, placa, marca, modelo, tarifa_base):
        self.placa = placa
        self.marca = marca
        self.modelo = modelo
        self.tarifa_base = tarifa_base
        self.disponible = True
        self.taximetro = 0
    
    def iniciar_viaje(self):
        if self.disponible:
            self.disponible = False
            self.taximetro = self.tarifa_base
            print(f"Taxi {self.placa} iniciando viaje")
    
    def finalizar_viaje(self):
        if not self.disponible:
            self.disponible = True
            costo = self.taximetro
            self.taximetro = 0
            print(f"Viaje finalizado. Costo: ${costo}")
            return costo
    
    def calcular_costo_viaje(self, distancia):
        tarifa_por_km = 2.5
        return self.tarifa_base + (distancia * tarifa_por_km)
```

> Rediseña estas clases utilizando herencia, polimorfismo y posiblemente herencia múltiple. Considera qué características y comportamientos son comunes, cuáles son específicos de cada tipo de vehículo, y cómo podrías implementar interfaces para diferentes capacidades (por ejemplo, vehículos que usan combustible, vehículos comerciales, etc.).
> 
</aside>

<aside>
<img src="https://www.notion.so/icons/database_red.svg" alt="https://www.notion.so/icons/database_red.svg" width="40px" />

### Ejercicio 7: Sistema de Comercio Electrónico

> Tienes estas clases que manejan un sistema de e-commerce básico:
> 

```python
class ProductoFisico:
    def __init__(self, nombre, precio, peso, dimensiones, stock):
        self.nombre = nombre
        self.precio = precio
        self.peso = peso
        self.dimensiones = dimensiones
        self.stock = stock
    
    def calcular_envio(self, destino):
        # Cálculo basado en peso y dimensiones
        costo_base = 10
        costo_peso = self.peso * 0.5
        return costo_base + costo_peso
    
    def verificar_disponibilidad(self, cantidad):
        return self.stock >= cantidad
    
    def reducir_stock(self, cantidad):
        if self.verificar_disponibilidad(cantidad):
            self.stock -= cantidad
            return True
        return False

class ProductoDigital:
    def __init__(self, nombre, precio, tamaño_archivo, tipo_licencia):
        self.nombre = nombre
        self.precio = precio
        self.tamaño_archivo = tamaño_archivo
        self.tipo_licencia = tipo_licencia
    
    def generar_link_descarga(self):
        import uuid
        return f"https://downloads.com/{uuid.uuid4()}"
    
    def verificar_disponibilidad(self, cantidad):
        return True  # Los productos digitales siempre están disponibles
        
    def procesar_compra(self):
        return self.generar_link_descarga()

class ClienteRegular:
    def __init__(self, nombre, email, direccion):
        self.nombre = nombre
        self.email = email
        self.direccion = direccion
        self.historial_compras = []
    
    def realizar_compra(self, producto, cantidad):
        if producto.verificar_disponibilidad(cantidad):
            costo_total = producto.precio * cantidad
            compra = {
                'producto': producto.nombre,
                'cantidad': cantidad,
                'costo': costo_total
            }
            self.historial_compras.append(compra)
            return costo_total
        return None
    
    def obtener_descuento(self, monto):
        return 0  # Sin descuento

class ClientePremium:
    def __init__(self, nombre, email, direccion, fecha_membresia):
        self.nombre = nombre
        self.email = email
        self.direccion = direccion
        self.fecha_membresia = fecha_membresia
        self.historial_compras = []
        self.puntos_acumulados = 0
    
    def realizar_compra(self, producto, cantidad):
        if producto.verificar_disponibilidad(cantidad):
            costo_total = producto.precio * cantidad
            descuento = self.obtener_descuento(costo_total)
            costo_final = costo_total - descuento
            
            compra = {
                'producto': producto.nombre,
                'cantidad': cantidad,
                'costo_original': costo_total,
                'costo_final': costo_final,
                'descuento': descuento
            }
            self.historial_compras.append(compra)
            self.puntos_acumulados += int(costo_final / 10)
            return costo_final
        return None
    
    def obtener_descuento(self, monto):
        return monto * 0.15  # 15% de descuento
    
    def canjear_puntos(self, puntos):
        if self.puntos_acumulados >= puntos:
            self.puntos_acumulados -= puntos
            return puntos * 0.01  # 1 punto = $0.01
        return 0

class CarritoCompras:
    def __init__(self, cliente):
        self.cliente = cliente
        self.items = []
    
    def agregar_item(self, producto, cantidad):
        item = {
            'producto': producto,
            'cantidad': cantidad
        }
        self.items.append(item)
    
    def calcular_total(self):
        total = 0
        for item in self.items:
            subtotal = item['producto'].precio * item['cantidad']
            total += subtotal
        
        # Aplicar descuento del cliente
        descuento = self.cliente.obtener_descuento(total)
        return total - descuento
    
    def procesar_compra(self):
        total = self.calcular_total()
        success = True
        
        for item in self.items:
            if not item['producto'].verificar_disponibilidad(item['cantidad']):
                success = False
                break
        
        if success:
            for item in self.items:
                if hasattr(item['producto'], 'reducir_stock'):
                    item['producto'].reducir_stock(item['cantidad'])
            
            self.items = []
            return total
        
        return None
```

> Refactoriza este código utilizando herencia, polimorfismo y herencia múltiple donde sea apropiado. Considera crear jerarquías de clases para productos y clientes, implementar interfaces o clases abstractas (superclases) para comportamientos comunes, y utilizar polimorfismo para simplificar el manejo de diferentes tipos de productos y clientes.
> 
</aside>

<aside>
<img src="https://www.notion.so/icons/database_red.svg" alt="https://www.notion.so/icons/database_red.svg" width="40px" />

### Ejercicio 8: Clínica Veterinaria

> La Clínica Veterinaria "Patitas Felices" necesita un sistema integral para gestionar sus operaciones diarias. El sistema debe manejar lo siguiente:
> 
> 
> **Animales y Dueños:**
> 
> - Los animales pueden ser de diferentes especies (perros, gatos, aves, reptiles, etc.)
> - Cada especie tiene características específicas (por ejemplo, los perros tienen raza, los gatos pueden ser de interior/exterior, las aves tienen tipo de vuelo)
> - Todos los animales comparten información básica: nombre, edad, peso, estado de salud
> - Los dueños tienen información personal y pueden tener múltiples mascotas
> - Algunos animales pueden no tener dueño (rescatados, en adopción)
> 
> **Personal de la Clínica:**
> 
> - Veterinarios: tienen especialidades (cirugía, dermatología, cardiología, etc.), número de licencia
> - Asistentes veterinarios: tienen nivel de certificación, pueden asistir en procedimientos específicos
> - Personal administrativo: manejan citas, facturación, archivo
> - Algunos empleados pueden tener múltiples roles (veterinario que también administra)
> 
> **Servicios y Tratamientos:**
> 
> - Consultas generales y especializadas
> - Cirugías (con diferentes niveles de complejidad)
> - Vacunaciones (con calendarios específicos por especie)
> - Tratamientos de emergencia
> - Servicios de estética (baño, corte de uñas, etc.)
> - Servicios de hospitalización
> 
> Diseña e implementa un sistema orientado a objetos completo que maneje todos estos requerimientos.
> 
</aside>