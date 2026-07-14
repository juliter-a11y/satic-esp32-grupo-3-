## Líder / Gestor Social
Aporte de [Erick]: informacion del proyecto, objetivos comunitarios, diagrama.
### Información del Proyecto
El proyecto SATIC-ESP32 tiene como finalidad desarrollar un sistema de alerta temprana de inundaciones mediante el uso del ESP32 y un sensor ultrasónico HC-SR04 para monitorear el nivel del agua y proteger a las comunidades vulnerables.

### Objetivos Comunitarios
- Detectar oportunamente posibles inundaciones.
- Reducir pérdidas humanas y materiales.
- Alertar a las autoridades y a la población.
- Implementar tecnología IoT para el monitoreo del río.

### Diagrama General del Sistema

Sensor HC-SR04
↓
ESP32
↓
Wi-Fi / Internet
↓
ThingSpeak / Blockchain
↓
Sirena de Emergencia y Autoridades



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

## Redes y Comunicaciones (Web 2.0)

Aporte de [Josue]: Configuración de red WiFi y envío de datos a ThingSpeak.

### Conexión WiFi

El ESP32 se conectará a la red WiFi de la comunidad usando las credenciales (SSID y contraseña). En caso de zonas sin cobertura WiFi, se contempla como alternativa un módulo de radiofrecuencia (LoRa) para el envío de datos a distancia.

```cpp
#include <WiFi.h>

const char* ssid = "WiFi_Comunidad";
const char* password = "contraseña123";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.println("Conectando a WiFi...");
  }
  Serial.println("Conectado a la red WiFi");
  Serial.println(WiFi.localIP());
}
```

### Envío de datos a ThingSpeak (HTTP GET)
Cada cierto intervalo de tiempo (por ejemplo, cada 20 segundos), el ESP32 enviará el dato del nivel de agua medido hacia la plataforma ThingSpeak mediante una petición HTTP GET, usando el API Key del canal creado.

```cpp
#include <HTTPClient.h>

String serverName = "http://api.thingspeak.com/update?api_key=TU_API_KEY";

void enviarDatos(float nivelAgua) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    String url = serverName + "&field1=" + String(nivelAgua);
    http.begin(url);
    int httpCode = http.GET();
    Serial.println(httpCode);
    http.end();
  }
}
```
