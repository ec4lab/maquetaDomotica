# Maqueta Domótica

Maqueta educativa con servidor domótico.

## Estructura de carpetas

```text
/maquetaDomotica
├── docs/
│   ├── Todo lo referido a documentación y procedimientos
│   └── README.md
├── dispositivos/
│   ├── sv_template_multisensor # ejemplo y guía para los siguientes
│   └── Demás dispositivos creados y el Paso a paso
└── dev/
    └── Carpetas de alumnos durante el desarrollo
```

## Línea de Tiempo

1. Maqueta - Construcción y definición de las plantas y áreas
2. Servidor - Configurar Router
3. Servidor - Construcción del soporte e instalación.
4. Servidor - Ajustes básicos y definir IP estática
5. Servidor - Configurar Personas
6. Servidor - Instalar File Editor
7. Servidor - Instalar Tailscale
8. Servidor - Instalar EspHome Device Builder
9. documentación - Política de nombres
10. dispositivos - sv-template-multisensor: guía base y pruebas
11. documentación - Tabla Dispositivos /IP
12. dispositivos - Ficha

### Próximamente

Protocolos
Automatizaciones
Pruebas
Incidentes

usuarios  
backups  
actualización  
seguridad  
estructura de áreas  
integración  
naming  
dashboard base  
dashboards adicionales

Zigbee
Bluetooth/BLE
ESP-NOW
LoRa
Protocolos industriales (Modbus / OPCua)
KNX
BACnet
MQTT
Matter

### La Maqueta / Servidor

* [Construir la maqueta](link)
* Instalar Servidor
  * En Raspberry
  * En Máquina Virtual
    * En Virtual Box
    * en VMWare
  * En una mini PC
  * Versión contenedor
* Acceso Remoto con Tailscale


### Los dispositivos

* ESPHome
  * En Home Assistant OS
  * En ESPHome web
  * En VSCode
  * En Cursor con IA.
  * Crear un dispositivo genérico
    * Componentes básicos del dispositivo.
    * Entidades básicas del dispositivo.

### Tormenta de ideas

Implementación de protocolos.

### Proximos pasos
1- Control de luces.  
Debe admitir 6 entradas y 6 salidas. 

2- Control W-LED para sala Multimedia.  
Diferentes escenas.