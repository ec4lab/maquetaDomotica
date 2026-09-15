# Maqueta Domótica

El propósito de este repositorio es documentar el paso a paso de la construcción de una Maqueta domótica didáctica implementada sobre la plataforma Home Assistant.

<p align="center">
<a href="docs/maqueta/" target="_blank" rel="noopener noreferrer">
    <img src="imagenes/maqueta.png" alt="System Monitor" width="400" border="none"/>
</a>
<figcaption align="center" style="margin-top: 10px; font-size: 14px; color: #555;">
    Maqueta Domótica
</figcaption>
</p>

Debido a la constante actualización de las plataformas utilizadas, en las guías se priorizarán conceptos y enfoques en lugar de un paso a paso estricto de utilización de cada herramienta, para evitar lo más posible la obsolescencia de la información.

## Estructura de carpetas

El repositorio se estructura en dos carpetas principales `docs` e `info`.

### docs

La primer carpeta contiene la documentación técnica y guías para la construcción e implementación de los dispositivos que componen la maqueta, par que puedan ser replicados y/o modificados en otras aplicaciones.

### info

Esta carpeta contiene información y algunos conceptos generales que complementan la documentación. No son aplicables directamente a la maqueta pero sirven como orientación para una mejor compresión de las aplicaciones y también brindan contexto a los dispositivos.

### Estructura actual del repositorio

```text
/maquetaDomótica
├── docs/
│   ├── maqueta/
│   ├── router-servidor/
│   ├── sv-template-multisensor/
│   ├── 
│   └── 
├── info/
│   ├── neopixels/
│   └── 
└── dev/
    └── Carpetas de alumnos durante el desarrollo
```

## Línea de Tiempo

Pasos a seguir para la construcción de la maqueta según la fecha de implementación

### 2026/08 - [docs/maqueta](docs/maqueta/)

* Construcción de la maqueta física
* Definición de las plantas y áreas  

### 2026/08 [docs/router-servidor](docs/router-servidor/)

* Construcción del soporte físico
* Configuración de la LAN en el Router
* Instalación de Home Assistant OS en una raspberryPI 4.
* Configuraciones básicas (IP estática, unidades, usuarios, mapa, App Companion)
* Asignación de áreas, plantas y habitaciones
* Primeras Aplicaciones (File Editor, ESPHome Device Builder, Tailscale)
* Mapa de la red y consideraciones para futuros dispositivos e integraciones.

### 2026/08 [docs/specs](docs/specs)

* Política de nombres de los dispositivos

### 2026/09 [docs/sv-template-multisensor](docs/sv-template-multisensor/)

* Primer dispositivo integrado a la maqueta.
* Entidades:
  * DHT11: Humedad y temperatura
  * Pulsador
  * Leds
  * Sensor de señal analógica
  * Botón de reset
* Sirve como plantilla para próximos dispositivos.

### 2026/09 [info/neopixels](info/neopixels)

* Introducción a las tiras de leds direccionables.
* Opciones de integración con ESPHome y Home Assistant

### 2026/10 [docs/control de luces](link)

### 2026/10 [docs/neopixels sala de estar](link)

## Trabajos Futuros

Protocolos  
Automatizaciones  
Pruebas  
Incidentes  
usuarios  
backups  
actualización  
seguridad  
dashboards  
dashboards adicionales
ZigBee  
Bluetooth/BLE  
ESP-NOW  
LoRa  
Protocolos industriales (ModBus / OPCua)  
KNX  
BACNet  
MQTT  
Matter  

### info dispositivos

* ESPHome
  * En Home Assistant OS
  * En ESPHome web
  * En VSCode
  * En Cursor con IA.
  * Crear un dispositivo genérico
    * Componentes básicos del dispositivo.
    * Entidades básicas del dispositivo.

## Licencia

Este proyecto está licenciado bajo la Licencia MIT.
Está permitido el uso, copia, modificaciones y distribución del software libremente, siempre que se incluya el aviso de derechos de autor original.  

Para más información, consultar el archivo [LICENSE](LICENSE)

## Contacto

**EC4lab**  
GitHub: [ec4lab](https://github.com/ec4lab)  
email: [ec4lab@gmail.com](ec4lab@gmail.com)
