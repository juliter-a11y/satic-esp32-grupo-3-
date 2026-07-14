# satic-esp32-grupo-3-
sistema de alerta temprana de inundaciones con ESP32-IESTP-ACORA 
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
