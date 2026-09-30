# Actividad 5 — Control de brazo robótico mediante URDF, ESP32 y Python

## Descripción del proyecto

En esta actividad se desarrolló un sistema de control para un brazo robótico utilizando un modelo en formato **URDF (Unified Robot Description Format)**.

El proyecto integra una simulación robótica con un sistema embebido basado en ESP32, permitiendo adquirir datos mediante sensores, transmitir información mediante comunicación UART y utilizar un script desarrollado en Python para controlar las articulaciones del robot.

El objetivo principal es establecer una comunicación en tiempo real entre la ESP32 y el entorno de simulación, permitiendo controlar movimientos del brazo robótico y validar acciones como:

- Movimiento de articulaciones.
- Apertura y cierre de pinza.
- Control mediante datos enviados desde hardware real.
- Comunicación serial entre ESP32 y computador.


---

# Arquitectura del sistema

```text
                 Sensores
                    |
                    |
                    v
                ESP32
            (MicroPython)
                    |
                    |
              Comunicación UART
                    |
                    |
                    v
              Computador
                  Python
                    |
                    |
                    v
              Simulación Robot
                URDF + PyBullet
                    |
                    |
                    v
              Movimiento brazo
              Apertura pinza
```

---

# Hardware utilizado

## ESP32

La ESP32 funciona como sistema de adquisición y transmisión de datos.

Funciones:

- Lectura de sensores.
- Procesamiento inicial de datos.
- Comunicación UART con el computador.
- Envío de comandos de control.


---

# Brazo robótico URDF

El robot utilizado está definido mediante un archivo:

```text
brazo.urdf
```

El formato URDF permite describir:

- Estructura mecánica del robot.
- Enlaces (`links`).
- Articulaciones (`joints`).
- Materiales.
- Geometrías.
- Restricciones de movimiento.


La simulación interpreta este archivo para generar el modelo virtual del brazo robótico.


---

# Software utilizado

## Python

El computador utiliza Python para recibir información de la ESP32 y controlar la simulación.


Librerías utilizadas:

```text
pybullet
pyserial
numpy
```

Instalación:

```bash
pip install -r requirements.txt
```


---

# Organización del repositorio

```text
Actividad_5/

│
├── URDF/
│   |
│   └── brazo.urdf
│
├── ESP32/
│   |
│   └── main.py
│
├── Python/
│   |
│   └── control_robot.py
│
├── evidencias/
│   |
│   ├── videos
│   └── capturas
│
└── README.md
```

---

# Funcionamiento del sistema

## 1. Lectura de sensores mediante ESP32

La ESP32 realiza la adquisición de señales provenientes de sensores.

Los datos obtenidos representan órdenes o valores de control para modificar el estado del robot.


Proceso:

1. Inicialización de sensores.
2. Lectura periódica de datos.
3. Conversión a formato digital.
4. Envío mediante UART.


---

# 2. Comunicación UART

La comunicación entre ESP32 y computador se realiza mediante comunicación serial.

Configuración utilizada:

```text
Baudrate:
115200
```

Ejemplo de dato enviado:

```text
Joint1:45
Joint2:20
Gripper:1
```


El computador recibe estos datos y los utiliza para actualizar el estado del robot.


---

# 3. Control mediante Python

El script desarrollado en Python realiza:

- Apertura del puerto serial.
- Lectura de datos enviados por ESP32.
- Interpretación de comandos.
- Actualización de articulaciones.
- Control del movimiento del robot.


Flujo:

```text
UART
 |
 v
Python
 |
 v
PyBullet
 |
 v
Articulaciones robot
```

---

# 4. Movimiento de articulaciones

Cada articulación del robot posee un movimiento independiente.

El programa permite modificar:

- Posición angular.
- Velocidad.
- Estado de la articulación.


Ejemplo:

```python
p.setJointMotorControl2()
```

permite enviar una posición objetivo al motor correspondiente.


---

# 5. Control de pinza

El sistema también permite validar el movimiento del efector final.

Funciones:

- Apertura de pinza.
- Cierre de pinza.
- Control mediante comandos recibidos.


Ejemplo:

```text
Gripper = 1
```

Indica apertura.

```text
Gripper = 0
```

Indica cierre.


---

# Simulación del robot

El modelo del brazo es cargado en el entorno de simulación mediante:

```python
p.loadURDF("brazo.urdf")
```

PyBullet permite:

- Visualización 3D.
- Control de articulaciones.
- Simulación física.
- Validación del movimiento.


---

# Pruebas realizadas

Durante la actividad se realizaron las siguientes pruebas:

## Prueba 1 — Carga del URDF

Validación de:

- Archivo correcto.
- Modelo visible.
- Articulaciones disponibles.


Resultado esperado:

```text
Robot cargado correctamente
```


---

## Prueba 2 — Comunicación UART

Se verificó:

- Conexión ESP32-PC.
- Recepción de datos.
- Actualización en tiempo real.


---

## Prueba 3 — Movimiento de articulaciones

Se validó:

- Movimiento individual de joints.
- Respuesta del robot.
- Control de posición.


---

## Prueba 4 — Apertura y cierre de pinza

Se comprobó:

- Activación del efector final.
- Movimiento correcto.
- Respuesta mediante comandos.


---

# Ejecución del proyecto

## Programar ESP32

Conectar la placa mediante USB.


Verificar puerto:

```bash
python -m serial.tools.list_ports
```


Subir programa:

```bash
mpremote connect COMx fs cp main.py :
```


Reiniciar:

```bash
mpremote connect COMx reset
```


---

# Ejecutar simulación

Ingresar a la carpeta Python:

```bash
cd Python
```

Ejecutar:

```bash
python control_robot.py
```

---

# Evidencias

El proyecto debe incluir:

- Capturas del brazo cargado en PyBullet.
- Evidencia de movimiento de articulaciones.
- Evidencia de apertura/cierre de pinza.
- Video de comunicación en tiempo real ESP32 - Python.
- Capturas del código desarrollado.


---

# Resultados obtenidos

El sistema permitió integrar un brazo robótico definido mediante URDF con un controlador externo basado en ESP32.

La comunicación UART permitió transmitir información desde el sistema embebido hacia Python, donde los datos fueron interpretados para modificar el comportamiento del robot dentro del entorno de simulación.

Se validó:

- Carga del modelo URDF.
- Comunicación en tiempo real.
- Movimiento de articulaciones.
- Control de la pinza.


---

# Conclusión

La actividad permitió desarrollar una arquitectura híbrida entre hardware real y simulación robótica.

La ESP32 funcionó como unidad de adquisición y transmisión de datos, mientras que Python y PyBullet permitieron controlar y visualizar el comportamiento del brazo robótico.

La integración de URDF, comunicación UART y simulación permitió establecer una base para sistemas robóticos donde dispositivos físicos controlan plataformas virtuales.
