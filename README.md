# IoTserver


# Servidor Web ESP32 con Cuatro Botones

Este proyecto utiliza un ESP32 para crear un servidor web que controla cuatro botones. Cada botón puede encender o apagar un pin de salida en el ESP32, permitiendo controlar dispositivos externos.

## Requisitos

- **Hardware**:
  - ESP32
  - Cables de conexión
  - Dispositivos a controlar (opcional)

- **Software**:
  - [Arduino IDE](https://www.arduino.cc/en/software) instalado
  - Biblioteca `WiFi.h` y `WebServer.h` (incluidas en el entorno de desarrollo de Arduino)

## Conexiones

| Pin del ESP32 | Dispositivo |
|---------------|-------------|
| GPIO 23      | Botón 1    |
| GPIO 22      | Botón 2    |
| GPIO 21      | Botón 3    |
| GPIO 19      | Botón 4    |

## Configuración

1. Clona este repositorio o descarga el archivo `sketch.ino`.
2. Abre el archivo en Arduino IDE.
3. Modifica las siguientes líneas con tus credenciales de WiFi:

   ```cpp
   const char* ssid = "TU_SSID";
   const char* password = "TU_CONTRASEÑA";

Conecta tu ESP32 a tu computadora.
Selecciona la placa correcta en el menú Herramientas.
Sube el código al ESP32.
Código
cpp
#include <WiFi.h>
#include <WebServer.h>

// Configuración de la red WiFi
const char* ssid = "TU_SSID";
const char* password = "TU_CONTRASEÑA";

// Crear un servidor en el puerto 80
WebServer server(80);

// Definir los pines para los botones
#define BUTTON1 23
#define BUTTON2 22
#define BUTTON3 21
#define BUTTON4 19

void setup() {
    Serial.begin(115200);
    WiFi.begin(ssid, password);
    
    // Esperar a que se conecte a WiFi
    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.println("Conectando a WiFi...");
    }
    
    Serial.println("Conectado a WiFi");
    Serial.print("IP: ");
    Serial.println(WiFi.localIP());

    // Configurar los pines como salida
    pinMode(BUTTON1, OUTPUT);
    pinMode(BUTTON2, OUTPUT);
    pinMode(BUTTON3, OUTPUT);
    pinMode(BUTTON4, OUTPUT);

    // Manejar las solicitudes HTTP
    server.on("/", handleRoot);
    server.on("/button1", handleButton1);
    server.on("/button2", handleButton2);
    server.on("/button3", handleButton3);
    server.on("/button4", handleButton4);

    server.begin();
}

void loop() {
    server.handleClient();
}

void handleRoot() {
    String html = "<html><body><h1>Controlador de Botones</h1>";
    html += "<button onclick=\"location.href='/button1'\">Botón 1</button><br>";
    html += "<button onclick=\"location.href='/button2'\">Botón 2</button><br>";
    html += "<button onclick=\"location.href='/button3'\">Botón 3</button><br>";
    html += "<button onclick=\"location.href='/button4'\">Botón 4</button><br>";
    html += "</body></html>";
    
    server.send(200, "text/html", html);
}

void handleButton1() {
    digitalWrite(BUTTON1, !digitalRead(BUTTON1)); // Cambiar estado del botón 1
    server.send(200, "text/html", "Botón 1 activado");
}

void handleButton2() {
    digitalWrite(BUTTON2, !digitalRead(BUTTON2)); // Cambiar estado del botón 2
    server.send(200, "text/html", "Botón 2 activado");
}

void handleButton3() {
    digitalWrite(BUTTON3, !digitalRead(BUTTON3)); // Cambiar estado del botón 3
    server.send(200, "text/html", "Botón 3 activado");
}

void handleButton4() {
    digitalWrite(BUTTON4, !digitalRead(BUTTON4)); // Cambiar estado del botón 4
    server.send(200, "text/html", "Botón 4 activado");
}

Cómo Probar
Abre el Monitor Serial en Arduino IDE para ver la dirección IP asignada al ESP32.
Accede a esa dirección IP desde un navegador web.
Verás una interfaz con cuatro botones que puedes usar para controlar los pines del ESP32.
Contribuciones
Si deseas contribuir a este proyecto, siéntete libre de hacer un fork y enviar un pull request.
