# Guía de Configuración: ESP8266 + WLED + Home Assistant

En esta guía se detalla el proceso paso a paso para flashear el firmware **WLED** en un ESP8266, configurar los presets para la tira NeoPixel del Living y vincular el dispositivo con **Home Assistant**.

---

## Requisitos Previos

* **Hardware:**
  * Microcontrolador ESP8266 (NodeMCU o Wemos D1 Mini).
  * Tira LED NeoPixel (WS2812B) instalada en el Living.
  * Fuente de alimentación de 5V adecuada para los LEDs.
  * Cable Micro-USB con líneas de datos habilitadas.
* **Software / Red:**
  * Navegador web basado en Chromium (Google Chrome o Microsoft Edge).
  * Servidor de Home Assistant corriendo en la misma red local Wi-Fi.

---

## Paso 1: Flasheo de WLED en el ESP8266

1. Conectá el ESP8266 a la computadora mediante el cable USB.
2. Abrí la herramienta de instalación oficial: **[install.wled.me](https://install.wled.me/)**.
3. Seleccioná la versión estándar de WLED para **ESP8266**.
4. Hacé clic en **Install** y seleccioná el puerto COM asignado a la placa (si no aparece, verificá tener instalados los controladores CH340 o CP210x).
5. Confirmá el flasheo y esperá a que el proceso complete al 100%.

---

## Paso 2: Configuración de Red Wi-Fi y Parámetros Hardware

1. Al encenderse por primera vez, el ESP8266 creará un punto de acceso Wi-Fi propio llamado **`WLED-AP`**.
2. Conectate a esa red desde un celular o PC (Contraseña por defecto: **`wled1234`**).
3. En la ventana emergente (*Captive Portal*), seleccioná tu red Wi-Fi local e ingresá la contraseña.
4. Una vez conectado a tu red, obtené la dirección IP asignada (podés verla en la pantalla o en la tabla de clientes de tu router) e ingresá a ella desde el navegador web.
5. Ve a **Config > LED Preferences** y configurá lo siguiente:
   * **LED outputs:** Seleccioná `WS281x`.
   * **GPIO:** Ingresá **`2`** (que corresponde al pin **D4** del NodeMCU/Wemos).
   * **Length:** Colocá el número exacto de LEDs instalados en la tira.
   * **Maximum Current:** Definí un límite de seguridad en mA (ej. 850 mA para evitar sobrecargas en la alimentación USB).
6. Guardá los cambios haciendo clic en **Save**.

---

## Paso 3: Configuración de Presets en WLED

Dentro de la interfaz web de WLED, ingresá a la pestaña **Presets** en el panel lateral superior y crea 4 presets con sus correspondientes identificadores (IDs):

1. **Preset ID 1 - `Living_Normal`**
   * Configuración: Efecto `Solid` | Color Blanco Cálido | Brillo 100%.
2. **Preset ID 2 - `Living_Cine`**
   * Configuración: Efecto `Breathe` (velocidad lenta) | Color Azul Profundo / Ámbar | Brillo 15%.
3. **Preset ID 3 - `Living_Fiesta`**
   * Configuración: Efecto `Rainbow` o `Palette` | Color Multicolor | Brillo 70%.
4. **Preset ID 4 - `Living_Alerta`**
   * Configuración: Efecto `Blink` | Color Rojo Puro | Brillo 100%.

---

## Paso 4: Vinculación con Home Assistant

1. Con el ESP8266 encendido en la misma red local, ingresá a tu instancia de **Home Assistant**.
2. Dirigite a **Ajustes > Dispositivos y servicios**.
3. Debería aparecer una notificación emergente indicando **"Dispositivo descubierto: WLED"** (vía mDNS/zeroconf).
4. Hacé clic en **Configurar** y asignale el área **Living**.
5. Verificá que se hayan creado las siguientes entidades principales:
   * `light.living_neopixel` (Entidad principal de luz).
   * `select.living_neopixel_preset` (Selector de Presets).

---

## Paso 5: Verificación y Prueba de Automatizaciones

Para verificar el funcionamiento desde Home Assistant, podés probar cambiar el Preset mediante la herramienta de desarrollo o llamando al servicio desde una tarjeta en el Dashboard:

```yaml
# Ejemplo de llamada de prueba desde Developer Tools > Services
service: select.select_option
target:
  entity_id: select.living_neopixel_preset
data:
  option: "Living_Cine"