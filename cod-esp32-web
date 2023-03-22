#include <WiFi.h>
#include <WiFiClient.h>
#include <WebServer.h>
#include <ESPAsyncWebServer.h>
#include <Adafruit_Sensor.h>
#include <DHT.h>

// Definir o tipo de sensor DHT11 ou DHT22
#define DHTTYPE DHT11

// Define o pino do sensor
#define DHTPIN 4

// Inicializa o sensor DHT
DHT dht(DHTPIN, DHTTYPE);

// Inicializa o servidor web
AsyncWebServer server(80);

// Define a rede Wi-Fi que o ESP32 irá se conectar
const char* ssid = "sua_rede_wifi";
const char* password = "sua_senha_wifi";

void setup() {
  // Inicializa o monitor serial
  Serial.begin(115200);

  // Conecta-se à rede Wi-Fi
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Conectando à rede Wi-Fi...");
  }
  Serial.println("Conexão Wi-Fi estabelecida");

  // Inicializa o sensor DHT
  dht.begin();

  // Define as rotas do servidor web
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    float temperatura = dht.readTemperature();
    float umidade = dht.readHumidity();
    String response = "Temperatura: " + String(temperatura) + "C, Umidade: " + String(umidade) + "%";
    request->send(200, "text/plain", response);
  });

  // Inicia o servidor web
  server.begin();
}

void loop() {
  // Aguarda novas solicitações HTTP
  server.handleClient();
}
