# Actividad — Lectura de potenciómetros mediante ESP32 y comunicación serial

## Descripción del proyecto

En esta actividad se desarrolló un sistema de adquisición de señales analógicas utilizando una placa **ESP32** y dos potenciómetros como elementos de entrada.

El objetivo principal es realizar la lectura de dos señales analógicas mediante los conversores ADC de la ESP32 y transmitir los valores obtenidos hacia un computador mediante comunicación serial.

El sistema permite observar en tiempo real la variación de las señales provenientes de los potenciómetros, estableciendo una base para aplicaciones posteriores de control, monitoreo o interacción con sistemas externos.

El desarrollo integra:

- Lectura de entradas analógicas mediante ADC.
- Programación de ESP32 utilizando Arduino IDE.
- Comunicación serial UART.
- Transmisión de datos en tiempo real.

---

# Arquitectura del sistema

```text
        Potenciómetro 1
              |
              |
          GPIO 4 ADC
              |
              |
              v

            ESP32

              ^
              |
          GPIO 5 ADC
              |
              |
        Potenciómetro 2


              |
              |
              v

        Comunicación Serial
            115200 baudios

              |
              |
              v

          Computador
```

---

# Hardware utilizado

## ESP32

La ESP32 funciona como sistema de adquisición de datos analógicos.

Sus funciones principales son:

- Leer las señales provenientes de los potenciómetros.
- Convertir las señales analógicas mediante el ADC interno.
- Enviar los valores obtenidos mediante comunicación serial.


---

# Potenciómetros

Se utilizaron dos potenciómetros conectados como entradas analógicas.

Configuración utilizada:

| Dispositivo | ESP32 |
|---|---|
| Potenciómetro 1 | GPIO 4 |
| Potenciómetro 2 | GPIO 5 |


Los valores obtenidos corresponden a la conversión ADC realizada por la ESP32.

---

# Software utilizado

## Arduino IDE

Lenguaje:

```text
C++
```

Librería utilizada:

```cpp
Arduino.h
```

El programa implementado permite configurar la comunicación serial y realizar la lectura periódica de los sensores analógicos.

---

# Organización del proyecto

```text
Actividad/

│
├── main.cpp
│
├── README.md
│
└── evidencias/
    |
    ├── capturas
    └── videos
```

---

# Funcionamiento del sistema

## 1. Inicialización de la ESP32

Durante el inicio del programa se configura la comunicación serial:

```cpp
Serial0.begin(115200);
```

La ESP32 establece una velocidad de comunicación de:

```text
115200 baudios
```

Además, envía mensajes iniciales indicando el estado del sistema:

```text
ESP32 INICIADO

Lectura de potenciometros:
```

---

# 2. Lectura de señales analógicas

La ESP32 realiza la lectura de los dos canales ADC:

```cpp
int pot1 = analogRead(POT1_PIN);

int pot2 = analogRead(POT2_PIN);
```

Cada lectura representa la posición del cursor del potenciómetro convertida a un valor digital mediante el ADC interno.

---

# 3. Envío de datos por comunicación serial

Los valores obtenidos son enviados al computador separados por una coma:

Ejemplo:

```text
1520,2800
```

La estructura enviada es:

```text
Potenciómetro 1 , Potenciómetro 2
```

El envío se realiza mediante:

```cpp
Serial0.print()
```

y:

```cpp
Serial0.println()
```

---

# 4. Actualización de datos

El sistema realiza una nueva lectura cada:

```cpp
delay(50);
```

equivalente a una actualización aproximada de:

```text
20 muestras por segundo
```

---

# Código principal

Archivo:

```text
main.cpp
```

Funciones principales:

- Configuración de pines analógicos.
- Inicialización serial.
- Lectura ADC.
- Transmisión de datos.

---

# Ejecución del proyecto

## 1. Conexión del hardware

Conectar:

- Potenciómetro 1 al GPIO 4.
- Potenciómetro 2 al GPIO 5.
- Alimentación de los potenciómetros.
- Tierra común con la ESP32.


---

## 2. Cargar programa en ESP32

Abrir el archivo:

```text
main.cpp
```

desde Arduino IDE.

Seleccionar:

- Placa ESP32 correspondiente.
- Puerto COM asignado.

Compilar y cargar el programa.

---

## 3. Visualizar datos

Abrir el monitor serial:

```text
115200 baudios
```

La salida esperada es:

```text
ESP32 INICIADO
Lectura de potenciometros:

1200,2500
1210,2498
1220,2505
```

---

# Resultados esperados

El sistema permite:

- Obtener lecturas analógicas mediante la ESP32.
- Transmitir información en tiempo real.
- Visualizar la variación de dos señales independientes.
- Establecer comunicación entre un sistema embebido y un computador.

---

# Conclusión

La implementación permitió desarrollar un sistema básico de adquisición de datos utilizando una ESP32 y dos entradas analógicas.

Mediante la lectura ADC y la comunicación serial fue posible capturar las variaciones de los potenciómetros y transmitirlas hacia un computador, estableciendo una base para sistemas de control y monitoreo más avanzados.
