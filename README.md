# satic-esp32-grupo-3-
sistema de alerta temprana de inundaciones con ESP32-IESTP-ACORA

## Hardware: Componentes y Conexiones

Aporte de [Nayeli]: Listado de componentes electrónicos y tabla de pines.

### Componentes Electrónicos

- 1x ESP32 (DevKit v1) – microcontrolador principal con Wi-Fi integrado
- 1x Sensor Ultrasónico HC-SR04 – mide la distancia/nivel del agua
- 1x Buzzer piezoeléctrico de 5V – alarma sonora local
- Cables Dupont (macho-macho y macho-hembra) – conexiones
- 1x Protoboard – montaje de pruebas
- 1x Fuente de alimentación solar (panel + regulador/batería) – energía autónoma
- Resistencias (divisor de voltaje para el pin Echo)

### Tabla de Conexión de Pines

| Componente             | Pin del componente | Pin del ESP32 |
|-------------------------|--------------------|----------------|
| HC-SR04                 | VCC                | 5V             |
| HC-SR04                 | GND                | GND            |
| HC-SR04                 | Trigger             | D23            |
| HC-SR04                 | Echo                | D22            |
| Buzzer piezoeléctrico   | (+)                | D5             |
| Buzzer piezoeléctrico   | (-)                | GND            |


## Firmware: Plantilla base de código (ESP32 - Arduino IDE)

Aporte de [Joel]: Estructura inicial del firmware para la lectura del sensor ultrasónico y control del buzzer.

```cpp
// Configuración de pines
const int trigPin = 23;
const int echoPin = 22;
const int buzzerPin = 5;

void setup() {
  Serial.begin(115200);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
}

void loop() {
  // Aquí irá la lógica de medición y activación de alarma
}
```
 Líder / Gestor Social

 Información del Proyecto
El proyecto SATIC-ESP32 tiene como finalidad desarrollar un sistema de alerta temprana de inundaciones mediante el uso del ESP32 y un sensor ultrasónico HC-SR04 para monitorear el nivel del agua y proteger a las comunidades vulnerables.

 Objetivos Comunitarios
- Detectar oportunamente posibles inundaciones.
- Reducir pérdidas humanas y materiales.
- Alertar a las autoridades y a la población.
- Implementar tecnología IoT para el monitoreo del río.

 Diagrama General del Sistema

Sensor HC-SR04
↓
ESP32
↓
Wi-Fi / Internet
↓
ThingSpeak / Blockchain
↓
Sirena de Emergencia y Autoridades
