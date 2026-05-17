```
#include <WiFi.h>
#include <HTTPClient.h>

// --- CONFIGURACIÓN MEDE USUARIO ---
const char* ssid = "RED MECATRONICA";
const char* password = "MECA2026@.";
const char* tagoToken = "3c8a0b1b-aa46-4d18-a9df-c4f56638c699";

// Pines del sensor HC-SR04
const int TRIG_PIN = 5;
const int ECHO_PIN = 18;

void setup() {
  Serial.begin(115200);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  WiFi.begin(ssid, password);
  Serial.print("Conectando a WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\n¡WiFi Conectado!");
}

void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    // 1. Medir distancia
    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);

    long duracion = pulseIn(ECHO_PIN, HIGH);
    float distancia = duracion * 0.034 / 2;

    // Filtro para lecturas reales
    if (distancia < 400 && distancia > 2) {
      Serial.printf("Distancia detectada: %.2f cm\n", distancia);

      // 2. Enviar a TagoIO
      HTTPClient http;
      http.begin("http://api.tago.io/data");
      http.addHeader("Content-Type", "application/json");
      http.addHeader("Device-Token", tagoToken);

      // El nombre de la variable DEBE ser "distancia"
      String payload = "{\"variable\":\"distancia\",\"value\":" + String(distancia) + "}";
      
      int httpCode = http.POST(payload);
      Serial.print("Respuesta TagoIO: "); 
      Serial.println(httpCode); // El 202 que ya lograste es perfecto

      http.end();
    }
  } else {
    Serial.println("Error: WiFi desconectado");
  }
  
  delay(3000); // Envío cada 3 segundos para que el HUD sea fluido
}
```