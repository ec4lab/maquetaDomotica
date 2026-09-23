# Propuesta de Implementación: Tira NeoPixel en Living (WLED + ESP8266)

## 1. Objetivos
Para la maqueta domótica, se implementará una tira LED direccionable (**WS2812B / NeoPixel**) ubicada exclusivamente en el **Living**.

El objetivo es doble:
1. **Iluminación Ambiental Dinámica:** Crear distintas escenas de confort según la actividad en el ambiente.
2. **Interfaz de Notificación Visual:** Utilizar la flexibilidad de los LEDs direccionables para actuar como indicador discreto del estado general de la casa (seguridad, accesos, etc.).

---

## 2. Hardware y Firmware Seleccionado

* **Microcontrolador:** **ESP8266** (NodeMCU v2/v3 o Wemos D1 Mini).
  * *Justificación:* Compacto, económico y altamente optimizado para tiras LED de un solo tramo en WLED, con rendimiento sobrado para los efectos 1D requeridos en la maqueta.
* **Pin de Salida de Datos (DIN):** **Pin D4 (GPIO 2 en WLED)**.
  * *Lógica de Conexión:* Tira NeoPixel con alimentación de 5V dedicada y masa común (`GND`) compartida con la fuente y el ESP8266. Resistencia de protección de ~330 Ω en la línea de datos.
* **Firmware:** **WLED** (Flasheado vía `install.wled.me`).
  * *Ventajas:* Procesamiento local de efectos sin sobrecargar la red, interfaz web nativa e integración automática (*autodiscovery*) con Home Assistant mediante mDNS/zeroconf.

---

## 3. Escenas y Automatizaciones en el Living

### A. Escenas Ambientales
* **Modo Normal / Diario (Preset 1):**
  * *Efecto:* `Solid`
  * *Color / Brillo:* Blanco Cálido / 80%–100%
  * *Disparador:* Sensor de presencia o encendido manual desde el Dashboard.
* **Modo Cine / Relax (Preset 2):**
  * *Efecto:* `Breathe` (transición tenue y muy lenta) o `Solid`
  * *Color / Brillo:* Azul profundo / Ámbar al 15%–20%
  * *Disparador:* Reproducción en Smart TV o botón de escena "Cine".
* **Modo Fiesta / Demo (Preset 3):**
  * *Efecto:* `Rainbow` / `Palette`
  * *Color / Brillo:* Dinámico multicolor / 70%
  * *Disparador:* Seleccionador manual en el Dashboard.

### B. Notificaciones de Estado
* **Alerta de Seguridad / Intrusión (Preset 4):**
  * *Efecto:* `Blink` / `Strobe`
  * *Color / Brillo:* Rojo intermitente / 100%
  * *Disparador:* Disparo del sistema de alarma (`alarm_control_panel`) o sensor de humo.
* **Aviso de Puerta / Portón Abierto:**
  * *Efecto:* Destello en segmento o esquina de la tira.
  * *Color:* Amarillo / Naranja por 3 segundos.
  * *Disparador:* Sensor magnético abierto por más de 1 minuto.

---

## 4. Matriz de Presets (WLED)

| ID Preset | Nombre en WLED | Efecto WLED | Color / Brillo | Función en Home Assistant |
| :---: | :--- | :--- | :--- | :--- |
| **1** | `Living_Normal` | `Solid` | Blanco Cálido (100%) | Luz principal de uso diario |
| **2** | `Living_Cine` | `Breathe` | Azul / Ámbar (15%) | Luz ambiental tenue para TV |
| **3** | `Living_Fiesta` | `Rainbow` | Multicolor (70%) | Demostración de efectos 1D/2D |
| **4** | `Living_Alerta` | `Blink` | Rojo (100%) | Alerta visual de seguridad |

---

