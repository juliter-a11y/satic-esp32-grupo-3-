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

### Redes y Comunicaciones: Configuración Wi-Fi y envío de datos

Aporte de [josue]: Configuración de conectividad Wi-Fi y protocolo HTTP para ThingSpeak.

Se agregó la explicación de la conexión de red, el código completo para conectar el ESP32 al Wi-Fi y enviar las lecturas del sensor de nivel de agua a la plataforma ThingSpeak mediante el protocolo HTTP GET.
```cpp
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "NOMBRE_DE_TU_RED_WIFI";
const char* password = "CONTRASEÑA_DE_TU_RED";
const char* server = "http://api.thingspeak.com/update";
const char* apiKey = "TU_CLAVE_DE_ESCRITURA_THINGSPEAK";

void conectarWiFi() {
  WiFi.begin(ssid, password);
  Serial.print("Conectando a Wi-Fi...");
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  
  Serial.println("\n Conectado exitosamente");
  Serial.print("IP asignada: ");
  Serial.println(WiFi.localIP());
}

void enviarDatos(float nivelAgua) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    
    // URL completa con el valor del sensor
    String url = String(server) + "?api_key=" + apiKey + "&field1=" + String(nivelAgua);
    
    http.begin(url);
    int codigoRespuesta = http.GET();
    
    // Verificación del envío
    if (codigoRespuesta > 0) {
      Serial.print("Dato enviado. Código: ");
      Serial.println(codigoRespuesta);
    } else {
      Serial.print("Error al enviar. Código: ");
      Serial.println(codigoRespuesta);
    }
    
    http.end();
  } else {
    Serial.println("Sin conexión Wi-Fi, no se pudo enviar el dato");
  }
}
```

## 5. Smart Contract - Blockchain
**Aporte de: Sofía**

Se implementará un Smart Contract en la red Blockchain para registrar de forma segura e inmutable los datos de nivel de agua obtenidos por el sensor HC-SR04. 

**Funcionalidad:**
- Cada vez que el ESP32 detecte un nivel de agua, enviará el dato a la Blockchain.
- El Smart Contract guardará: fecha, hora, nivel del agua y ubicación.
- Una vez registrado, el dato no se puede modificar, lo que garantiza transparencia.
- Las autoridades y la comunidad podrán consultar el historial de alertas en tiempo real.

**Tecnología usada:** Solidity en Ethereum / Polygon
**Beneficio:** Evita falsificación de datos y sirve como evidencia oficial ante inundaciones.


# Soporte y Contingencia

**Aporte de [Yakhemmy]:** Plan de resolución de problemas (Troubleshooting) y cálculo de energía del sistema.

## Plan de resolución de problemas (Troubleshooting)

### Sensor HC-SR04 con lodo o suciedad

Si el sensor ultrasónico HC-SR04 se ensucia con lodo, hojas o residuos, las mediciones pueden ser incorrectas o dejar de funcionar. Se recomienda limpiar el sensor periódicamente, verificar que no existan obstáculos frente a él, instalar una cubierta protectora y realizar mantenimiento preventivo antes de la temporada de lluvias.

## Tabla de problemas y soluciones

| Problema | Posible causa | Solución |
|----------|---------------|----------|
| ESP32 no enciende | Batería descargada o conexiones sueltas | Revisar la batería, el panel solar y las conexiones. |
| HC-SR04 no mide correctamente | Sensor sucio o mal conectado | Limpiar el sensor y revisar el cableado. |
| No hay conexión Wi-Fi | Señal débil o credenciales incorrectas | Verificar la red Wi-Fi y volver a conectar el ESP32. |
| El buzzer no funciona | Buzzer desconectado o dañado | Revisar las conexiones o reemplazar el buzzer. |
| No se envían datos a ThingSpeak | Error en la API Key o falta de Internet | Verificar la conexión y la configuración de ThingSpeak. |

## Cálculo de energía

Si la batería tiene una capacidad de **100 Wh** y trabaja con una eficiencia del **85%**, la energía útil se calcula de la siguiente manera:

**Energía útil = 100 Wh × 0.85 = 85 Wh**

**Resultado:** El sistema dispondrá de **85 Wh** de energía útil para alimentar el ESP32, el sensor HC-SR04 y el buzzer.

## Recomendaciones

- Limpiar periódicamente el sensor ultrasónico.
- Revisar el estado de la batería y del panel solar.
- Verificar la conexión Wi-Fi antes de cada monitoreo.
- Probar el buzzer para asegurar el funcionamiento de la alarma.

## Conclusión

El mantenimiento preventivo y el monitoreo constante permiten que el sistema de alerta temprana funcione de forma confiable. Además, una batería de **100 Wh** con una eficiencia del **85%** proporciona **85 Wh** de energía útil para el correcto funcionamiento del sistema.
