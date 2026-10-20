# Control de 6 Luces con ESP32 y Home Assistant

Este proyecto consiste en el desarrollo de un módulo de automatización del hogar basado en el microcontrolador **ESP32 (NodeMCU de 38 pines con chip CP2102)**. El objetivo principal es controlar **6 circuitos de iluminación independientes**. 

El sistema se integra de manera nativa con **Home Assistant** a través de firmware, como ESPHome , lo que permite:

* **Control Inteligente:** Encender, apagar y programar las luces desde la app de Home Assistant.
* **Control Físico Local:** Mantener la operatividad clásica mediante teclas físicas en la pared. Si el servidor de Home Assistant o la red Wi-Fi se caen, las luces se pueden seguir controlando de forma manual sin interrupciones.

---

## ⚡ Consideraciones Eléctricas (5V CC)

Dado que las luces operan a **5V CC** y el ESP32 opera internamente a **3.3V** (con una corriente máxima por pin muy baja, ~12-20mA), se siguen las siguientes pautas de hardware:

1. **Alimentación:** Se  utiliza una fuente regulada de 5V y 20A de para alimentar tanto las 6 luces como el ESP32. Los 5V de la fuente se conectan directamente al pin **VIN / 5V** del ESP32 para energizar la placa.
2. **Interfaz de Potencia (Conmutación):** Para activar las luces desde los pines de 3.3V del ESP32, se debe utilizar como intermediario:
   * **Transistor BJT 2N2222A** Que se activan al poner en alto las salidas del ESP32.
3. **Masa Común:** El cable negativo (-) de la fuente de 5V, el negativo de las luces, el pin **GND** del ESP32 y las masas de los transistores deben estar unidos para compartir la misma referencia.

## 📌 Guía de Referencia de Pines (ESP32 38 Pines)

Para garantizar la estabilidad del sistema y evitar reinicios inesperados (boot loops), la distribución de los pines del hardware se ha diseñado respetando las restricciones eléctricas de la placa.

### 🟢 GPIO de Uso General Seguros (Entrada y Salida)
Son los pines recomendados para cualquier propósito general (actuadores, relés, señales digitales):

* **GPIO 4**
* **GPIO 13**
* **GPIO 14**
* **GPIO 19**
* **GPIO 21**
* **GPIO 22**
* **GPIO 23**
* **GPIO 25**
* **GPIO 26**
* **GPIO 27**
* **GPIO 32**
* **GPIO 33**

### 🔵 Pines Exclusivos de Entrada (solamente de entrada)
Funcionan **únicamente como entradas** (digitales o analógicas). No cuentan con resistencias *pull-up* internas y no pueden configurarse como salidas:

* **GPIO 34**
* **GPIO 35**
* **GPIO 36 (VP)**
* **GPIO 39 (VN)**



## 🛠️ Configuración de Hardware del Proyecto

Se detalla la asignación de pines utilizada para procesar las señales de las teclas (entradas).

### ⌨️ Pines Utilizados para Teclas (Entradas)
Se configuran 8 opciones de entrada para gestionar el control manual. En tres de ellas se puede aprovechan los sensores táctiles internos del chip.

* **GPIO 4** -> Puede configurarse como Entrada Touch (Capacitiva) `[LIBRE]`
* **GPIO 19** -> Entrada Digital Convencional `[ASIGNADA]`
* **GPIO 32** -> Puede configurarse como Entrada Touch (Capacitiva) `[LIBRE]`
* **GPIO 33** -> Puede configurarse como Entrada Touch (Capacitiva) `[ASIGNADA]`
* **GPIO 34** -> Entrada Digital Convencional (Requiere Pull-Up externa) `[ASIGNADA]`
* **GPIO 35** -> Entrada Digital Convencional (Requiere Pull-Up externa) `[ASIGNADA]`
* **GPIO 36 (VP)** -> Entrada Digital Convencional (Requiere Pull-Up externa) `[ASIGNADA]`
* **GPIO 39 (VN)** -> Entrada Digital Convencional (Requiere Pull-Up externa) `[ASIGNADA]`

### 💡 Asignación de Salidas (Control de Relés)
Lista de pines disponibles y validos para enviar la señal de activación:

* **GPIO 16** -> Salida Actuador `[ASIGNADA]`
* **GPIO 17** -> Salida Actuador `[ASIGNADA]`
* **GPIO 18** -> Salida Actuador `[ASIGNADA]`
* **GPIO 21** -> Salida Actuador `[ASIGNADA]`
* **GPIO 22** -> Salida Actuador `[ASIGNADA]`
* **GPIO 23** -> Salida Actuador `[ASIGNADA]`
* **GPIO 25** -> Salida Actuador `[LIBRE]`
* **GPIO 26** -> Salida Actuador `[LIBRE]`
* **GPIO 27** -> Salida Actuador `[LIBRE]`



