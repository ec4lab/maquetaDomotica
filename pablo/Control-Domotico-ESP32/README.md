# Control de 6 Luces con ESP32 y Home Assistant

## 📌 Descripción del proyecto

Este proyecto consiste en el desarrollo de un sistema de domótica para controlar **6 circuitos de iluminación independientes**, utilizando un microcontrolador **ESP32 DevKit V1 de 38 pines con chip CP2102**.

## Documentación

- [Funcionamiento del sistema](#funcionamiento-del-sistema)
- [Diseño electrónico y PCB](Hardware\README)
- [Programación e integración con Home Assistant](Software/README)

## Funcionamiento del sistema

El ESP32 funciona como el cerebro del sistema y se comunica con **Home Assistant** mediante Wi-Fi, utilizando ESPHome para la configuración del firmware y la integración con la plataforma de automatización del hogar.

El sistema permite:

- **Control desde Home Assistant:** encender y apagar cada una de las seis luces de forma independiente.
- **Control físico local:** utilizar teclas conectadas directamente a las entradas del ESP32 para controlar las luces manualmente.
- **Funcionamiento independiente de la conexión:** las teclas físicas permiten seguir controlando las luces aunque Home Assistant o la conexión Wi-Fi no estén disponibles, siempre que el ESP32 continúe alimentado y funcionando.
- **Diseño de hardware propio:** desarrollar el circuito electrónico, el esquemático y la placa de circuito impreso (PCB) mediante KiCad.

---

## ⚡ 1. Consideraciones eléctricas

El circuito trabaja con dos niveles de tensión: **5 V CC para alimentar las luces y 3,3 V para las señales de entrada y salida del ESP32**.

### 1.1. Alimentación del sistema

Se utiliza una fuente regulada de **5 V y 20 A** para alimentar los circuitos de iluminación. Esta fuente también puede alimentar la placa ESP32 mediante su entrada VIN/5V, siempre que la tensión y la conexión sean compatibles con la placa utilizada.

La distribución de alimentación se organiza de la siguiente manera:

- **5 V:** alimentación de los circuitos de iluminación.
- **3,3 V:** tensión utilizada para las señales de las teclas físicas.
- **GND:** referencia común entre el ESP32, la fuente de alimentación y los circuitos de los transistores.

El negativo de la fuente de 5 V y el GND del ESP32 deben estar conectados entre sí para que las señales de control tengan una referencia eléctrica común.

### 1.2. Entradas para las teclas físicas

Las seis teclas se conectan a las entradas GPIO 19, 33, 34, 35, 36 y 39 del ESP32.

Cada entrada utiliza una resistencia externa **pull-down de 10 kΩ**, conectada entre el GPIO y GND. La tecla conecta la entrada a 3,3 V cuando se presiona.

De esta manera, se obtienen dos estados eléctricos:

- **Tecla sin presionar:** la resistencia pull-down mantiene la entrada en 0 V (nivel lógico LOW).
- **Tecla presionada:** la entrada recibe 3,3 V (nivel lógico HIGH).

Las resistencias pull-down mantienen un nivel lógico definido cuando la tecla está abierta, evitando que las entradas queden flotantes y puedan detectar cambios de estado incorrectos.

Los GPIO 34, 35, 36 y 39 son exclusivamente de entrada y no disponen de resistencias pull-up o pull-down internas configurables. Por eso, en estos pines es necesario utilizar resistencias externas.

### 1.3. Etapa de potencia con transistores BJT 2N2222A

Las luces se controlan mediante seis transistores **BJT 2N2222A**, uno para cada circuito de iluminación.

Los GPIO 16, 17, 18, 21, 22 y 23 se utilizan como salidas digitales del ESP32. Cada salida controla la base de un transistor a través de una resistencia limitadora de corriente.

El transistor permite utilizar una señal de control de 3,3 V para conmutar una carga alimentada con 5 V, evitando que la corriente de las luces circule directamente por los pines del microcontrolador.

En la configuración de conmutación habitual con un transistor NPN:

- El emisor se conecta a GND.
- La base recibe la señal de control del ESP32 a través de una resistencia.
- El colector se conecta al circuito de la carga.
- La otra conexión de la luz se conecta a la alimentación de 5 V.

Cuando la salida del ESP32 se pone en nivel alto, el transistor conduce y permite el encendido de la luz. Cuando la salida se pone en nivel bajo, el transistor deja de conducir y la luz se apaga.

La resistencia de base debe dimensionarse según la corriente necesaria para activar el transistor y la corriente que demanda cada carga. También se debe verificar que el transistor soporte la corriente de la luz y que la conexión de sus terminales corresponda al modelo utilizado.

---

## 🔌 2. Asignación de pines del ESP32

La siguiente tabla muestra la distribución de las seis entradas destinadas a las teclas y las seis salidas destinadas al control de las luces.

### 2.1. Entradas: teclas físicas

| Tecla | GPIO del ESP32 | Función |
|---|---|---|
| Tecla 1 | GPIO 19 | Control luz Living / Entrada |
| Tecla 2 | GPIO 33 | Control luz Cocina |
| Tecla 3 | GPIO 34 | Control luz Garage |
| Tecla 4 | GPIO 35 | Control luz Sala de Estar |
| Tecla 5 | GPIO 36 (VP) | Control luz Dormitorio |
| Tecla 6 | GPIO 39 (VN) | Control luz Dormitorio Principal|

### 2.2. Salidas: control de luces

| Luz | GPIO del ESP32 | Elemento controlado |Sector|
|---|---|---|---|
| Luz 1 | GPIO 16 | Transistor 2N2222A | Living / Entrada |
| Luz 2 | GPIO 17 | Transistor 2N2222A | Cocina |
| Luz 3 | GPIO 18 | Transistor 2N2222A | Garage |
| Luz 4 | GPIO 21 | Transistor 2N2222A | Sala de Estar |
| Luz 5 | GPIO 22 | Transistor 2N2222A | Dormitorio |
| Luz 6 | GPIO 23 | Transistor 2N2222A | Dormitorio Principal |

Cada tecla se asocia a una luz determinada. Cuando se detecta una pulsación, se invierte el estado de esa luz: si estaba encendida, se apaga; si estaba apagada, se enciende.

---

## 🧠 3. Funcionamiento general del sistema

El funcionamiento del proyecto se basa en la combinación del control físico mediante teclas y el control remoto a través de Home Assistant.

1. El ESP32 inicializa sus entradas y salidas digitales y establece la comunicación con Home Assistant mediante Wi-Fi.
2. Cada tecla se lee a través de su entrada GPIO correspondiente.
3. Cuando se detecta una pulsación, se invierte el estado de la luz asociada.
4. El estado de la luz se refleja en la salida digital del ESP32: nivel alto para activar el transistor y nivel bajo para desactivarlo.
5. El transistor conmuta la alimentación de 5 V de la luz correspondiente.
6. Cuando se utiliza Home Assistant para encender o apagar una luz, se modifica el estado de la misma salida y se actualiza el estado lógico asociado.

De esta manera, cada luz puede controlarse de forma independiente, tanto desde las teclas físicas como desde la interfaz de Home Assistant.

---
