# 🗒️Practica 10 - Crear Entornos de Python desde requirements.txt

## 9️⃣ Ejercicio Práctico 1

### Escenario

> Vas a configurar un entorno para un proyecto de análisis de datos.
> 

1️⃣ Crear la carpeta del proyecto y el entorno virtual

1. Abre **PowerShell**.
2. Ve a la carpeta donde quieras crear el proyecto (por ejemplo, Escritorio):
    
    ```powershell
    cd $HOME\\Desktop
    ```
    
3. Crea la carpeta del proyecto y entra en ella:
    
    ```powershell
    mkdir proyecto_analisis
    cd proyecto_analisis
    ```
    
4. Crea un entorno virtual llamado `venv`:
    
    ```powershell
    py -m venv venv
    ```
    
5. Activa el entorno virtual:
    
    ```powershell
    .\venv\Scripts\Activate.ps1
    ```
    
    Deberías ver algo así al inicio de la línea:
    
    ```
    (venv) PS C:\Users\tu_usuario\Desktop\proyecto_analisis>
    ```
    

---

### 2️⃣ Crear el archivo `requirements.txt`

1. Dentro de la carpeta `proyecto_analisis`, crea un archivo llamado `requirements.txt` con el siguiente contenido:
    
    ```
    pandas
    numpy
    matplotlib
    seaborn
    jupyter
    ```
    
2. Guarda el archivo en la carpeta del proyecto.

### 3️⃣ Instalar las dependencias y verificar instalación

1. Asegúrate de que el entorno `venv` sigue activado (debe verse `(venv)` al inicio de la línea).
2. Instala las dependencias:
    
    ```powershell
    pip install -r requirements.txt
    ```
    
3. Verifica la instalación viendo la lista de paquetes:
    
    ```powershell
    pip list
    ```
    
    Comprueba que aparecen `pandas`, `numpy`, `matplotlib`, `seaborn` y `jupyter`.
    

---

### 4️⃣ Crear el archivo `test_entorno.py`

1. En la carpeta `proyecto_analisis`, crea un archivo llamado `test_entorno.py`.
2. Escribe el siguiente código:
    
    ```python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import seaborn as sns
    
    print("✅ Todas las librerías se importaron correctamente")
    print(f"Pandas version: {pd.__version__}")
    print(f"NumPy version: {np.__version__}")
    
    # Pequeño ejemplo opcional: crear una serie y mostrar estadísticas
    datos = pd.Series([10, 20, 30, 40, 50])
    print("\nDatos de prueba:", list(datos))
    print("Media:", datos.mean())
    print("Desviación estándar:", datos.std())
    ```
    
3. Guarda el archivo.

---

### 5️⃣ Ejecutar el script `test_entorno.py` para comprobar el entorno

1. Asegúrate de que estás en la carpeta del proyecto y el entorno sigue activado:
    
    ```powershell
    (venv) PS C:\Users\tu_usuario\Desktop\proyecto_analisis>
    ```
    
2. Ejecuta el script:
    
    ```powershell
    python .\test_entorno.py
    ```
    
3. Deberías ver una salida similar a:
    
    ```
    ✅ Todas las librerías se importaron correctamente
    Pandas version: 2.x.x
    NumPy version: 2.x.x
    
    Datos de prueba: [10, 20, 30, 40, 50]
    Media: 30.0
    Desviación estándar: 15.811388...
    ```
    

---

### 6️⃣ Cerrar el entorno cuando termines

Cuando hayas terminado de trabajar:

```powershell
deactivate
```

La línea de PowerShell volverá a su estado normal, sin el prefijo `(venv)`.

## Ejercicio Practico 2: Entorno básico de análisis

1. En PowerShell, crea una carpeta llamada `proyecto_basico` en tu Escritorio.
2. Entra en la carpeta con `cd`.
3. Crea un archivo `requirements.txt` con:
    - `pandas`
    - `numpy`
    - `jupyter`
4. Crea un entorno virtual `.venv` con `py -m venv .venv`.
5. Activa el entorno con `.\.venv\Scripts\Activate.ps1`.
6. Instala las dependencias con:
    
    ```powershell
    pip install -r requirements.txt
    ```
    
7. Abre un notebook de Jupyter:
    
    ```powershell
    jupyter notebook
    ```
    
8. En el notebook:
    - Crea un `DataFrame` con 5 filas y 3 columnas.
    - Muestra `.shape` y `.dtypes`.

---

## Ejercicio Practico 3: Congelar dependencias

1. Crea una carpeta `proyecto_graficos`.
2. Crea un entorno virtual y actívalo.
3. Instala manualmente los paquetes:
    
    ```powershell
    pip install pandas matplotlib seaborn
    ```
    
4. Usa `pip list` para ver lo instalado.
5. Genera un `requirements.txt` con:
    
    ```powershell
    pip freeze > requirements.txt
    
    ```
    
6. Crea otra carpeta `proyecto_clonado`.
7. Copia `requirements.txt` a `proyecto_clonado`.
8. En `proyecto_clonado`, crea un nuevo `.venv`, actívalo e instala:
    
    ```powershell
    pip install -r requirements.txt
    ```
    
9. Verifica con `pip list` que las versiones de `pandas` y `matplotlib` coinciden en ambos proyectos.

---

## Ejercicio 3: Resolver conflicto de versiones

1. En una carpeta nueva, crea un `requirements.txt` con versiones conflictivas, por ejemplo:
    
    ```
    pandas==2.2.2
    numpy==1.18.0
    ```
    
2. Crea un entorno `.venv`, actívalo e intenta instalar:
    
    ```powershell
    pip install -r requirements.txt
    ```
    
3. Observa el mensaje de error en PowerShell.
4. Ajusta la versión de `numpy` en `requirements.txt` a una más reciente (por ejemplo, `numpy==1.26.4`).
5. Vuelve a instalar.
6. Documenta qué cambio hiciste para resolver el conflicto.