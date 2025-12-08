# 🐍 Clase 09 - Venv: Entornos virtuales

### 1. Instalación de Anaconda Navigator:

[Navigator Anaconda Navigator | Anaconda](https://www.anaconda.com/products/navigator)

![image.png](image.png)

Puedes descargar el ejecutable dando [click aqui](https://tajamar365.sharepoint.com/:u:/s/357718.2PYTHONF241783AA/EcHMhI4A5ARJk9i58kqxrQgB82QHQaG5H0fssWu0D2HGvw?e=EidCqD)

Una vez instalado Anaconda, cuando lo ejecutas verás el home principal así:

![image.png](image%201.png)

Para crear un entorno virtual damos click en Environments.

![image.png](image%202.png)

Damos click en Create:

![image.png](image%203.png)

![image.png](image%204.png)

Click en Create

 

![image.png](image%205.png)

Podemos instalar las aplicaciones que necesitamos dando click en Install dependiendo de la herramienta que necesite instalar, por ejemplo PyCharm, JupyterLab…

Si queremos instalar librerías y módulos de python nos vamos a Environments:

![image.png](image%206.png)

y en la parte superior derecha en Search Packages escribimos la librería o modulo que deseamos instalar, por ejemplo: Numpy

![image.png](image%207.png)

Luego click en Apply y esperamos a que se instale la libreria:

![image.png](image%208.png)

Click de nuevo en Apply. 

Una vez que se ha instalado la libreria, esta deberia verse con un check ✅ verde:

![image.png](image%209.png)

## 2. Instalación de Jupyter Lab desde un entorno virtual

[Cómo instalar y abrir JupyterLab – Kanaries](https://docs.kanaries.net/es/topics/Python/how-to-start-juypter-lab)

Un entorno virtual ayuda a aislar los paquetes entre distintos proyectos.

### **1. Instalar `virtualenv`**

```
pip install virtualenv
```

### **2. Crear un entorno virtual**

```
virtualenv myenv
```

### **3. Activar el entorno**

**Windows**

```
.\myenv\Scripts\activate
```

**macOS y Linux**

```
source myenv/bin/activate
```

### **4. Instalar JupyterLab dentro del entorno virtual**

```
pip install jupyterlab
```

### **5. Abrir JupyterLab**

```
jupyter lab
```

Para salir del entorno:

```python
**deactivate**
```

## 3. Instalación de entornos virtuales desde VS Code + Jupyter notebook como extension

### Pasos para instalar VSCode en Windows lo puedes ver en [este enlace](https://www.youtube.com/watch?v=HxD8nVU0oy8).

Cuando abres VsCode por primera vez debería verse así:

 

![image.png](image%2010.png)

Vamos a la terminal y desde allí vamos a crear una carpeta de prueba, podemos crear esa carpeta en el escritorio pero usado comandos de terminal:

1. Nos vamos hacia la carpeta escritorio escribiendo:
    
    ```python
    cd OneDrive\Escritorio
    ```
    
    ![image.png](image%2011.png)
    
    > Nota: La ruta puede variar dependiendo de la forma en que tengas configurado tu sistema de archivos en Windows.
    > 
2. Creamos una carpeta de nombre prueba escribiendo.
    
    ```python
    mkdir prueba
    ```
    
    ![image.png](image%2012.png)
    
3. Nos movemos a la carpeta prueba escribiendo
    
    ```python
    cd prueba
    ```
    
    ![image.png](image%2013.png)
    
4. Estamos dentro de la carpeta prueba desde la terminal, pero necesitamos abrir dicha carpeta desde VSCODE, damos click en Open Folder:
    
    ![image.png](image%2014.png)
    
5. Seleccionamos la carpeta prueba, la cual se encuentra en el escritorio:
    
    ![image.png](image%2015.png)
    
6. Damos click en Yes
    
    ![image.png](image%2016.png)
    
7. Deberia verse en la barra explorer que has abierto al carpeta prueba:
    
    ![image.png](image%2017.png)
    
    ![image.png](image%2018.png)
    
8. En la terminal, estando dentro de la carpeta prueba podemos crear nuestro entorno virtual siguiendo los mismos pasos de la sección anterior:
    
    ```python
    virtualenv mi-entorno
    
    ```
    
    luego activamos el entorno:
    
    ```python
    .\mi-entorno\Scripts\activate
    ```
    
    ![image.png](image%2019.png)
    
    Debería verse en nuestra interfaz gráfica de VSCode que se ha creado el entorno virtual (mi-entorno):
    
    ![image.png](image%2020.png)
    

Con el entorno activado podemos instalar las librerías necesarias para nuestro proyecto:

```python
pip install jupyter lab
```

![image.png](image%2021.png)

Podemos hacer uso de una extensión en VSCode para escribir con notebooks de jupyter, buscamos el boton de extensiones:

![image.png](image%2022.png)

 En la barra de búsqueda de la extensiones:

![image.png](image%2023.png)

En la barra de busqueda de las extensiones escribimos Jupyter, seleccionamos aquella que contenga la palabra Microsoft:

![image.png](image%2024.png)

Al dar click sobre él veremos lo siguiente:

![image.png](image%2025.png)

Damos click en Install: 

![image.png](image%2026.png)

Una vez instalado podemos cerrar la pestaña:

![image.png](image%2027.png)

![image.png](image%2028.png)

Volvemos a dar click en el icono de Explorer (Explorador):

![image.png](image%2029.png)

Deberías verse así:

![image.png](image%2030.png)

Vamos a probar crear un notebook de Jupyter dando click en: 

![image.png](image%2031.png)

Luego escribimos: `cuaderno1.ipymb`y presionamos Enter:

![image.png](image%2032.png)

![image.png](image%2033.png)

Seleccionamos un kernel dando click en:

![image.png](image%2034.png)

Escogemos Python Environments:

![image.png](image%2035.png)

Seleccionamos el que dice: mi-entorno:

![image.png](image%2036.png)

Ya tenemos nuestro notebook preparado para trabajar sobre el. Guardamos el notebook presionando CTRL + s. o en: **Archivo>Guardar.**

Vamos a probar si todo anda bien, creamos una celda de codigo pulsando sobre:

![image.png](image%2037.png)

Escribimos sobre la celda: `Hola Mundo:`

![image.png](image%2038.png)

Guardamos el archivo: CTRL +s (o CTRL+ g si tu versión es en español) o pulsando en **file(archivo)>Save(guardar).**

Para ejecutar nuestra celda de código pulsamos sobre ▶️, o escribiendo en nuestro teclado SHIFT + ENTER

![image.png](image%2039.png)

Devberia verse la salida asi:

![image.png](image%2040.png)

Ahora vamos a crear otro archivo, pero esta vez con extensión .py, nos vamos a:

![image.png](image%2041.png)

Y escribimos: `archivo1.py` , y luego Enter:

![image.png](image%2042.png)

Debería verse así:

![image.png](image%2043.png)

vamos a probar si Python funciona correctamente en ese entorno escribiendo sobre el archivo **archivo1.py:**

![image.png](image%2044.png)

Ejecutamos el código dando pulsando sobre ▶️:

![image.png](image%2045.png)

En el terminal debería verse así:

![image.png](image%2046.png)

si logras ver en tu terminal “Hola Mundo” todo está bien.