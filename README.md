# Arduino-Code
#define BLYNK_TEMPLATE_ID "TMPL3AqKlVBO_"
#define BLYNK_TEMPLATE_NAME "Home Automation"
#define BLYNK_AUTH_TOKEN "RVZuc3uSrNVpgmkmWAk8waW9aWFVz7Qe"

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Replace with your WiFi credentials
char ssid[] = "Apna Engineer";
char pass[] = "apna@engineer";

// Relay pins (D1–D3 + D5 on NodeMCU)
#define RELAY1 D1
#define RELAY2 D2
#define RELAY3 D3
#define RELAY4 D5  // Changed from D4 to D5

// Onboard LED pin (D4 = GPIO2)
#define WIFI_LED D4

void setup()
{
  Serial.begin(115200);

  // Set relay pins as output
  pinMode(RELAY1, OUTPUT);
  pinMode(RELAY2, OUTPUT);
  pinMode(RELAY3, OUTPUT);
  pinMode(RELAY4, OUTPUT);

  // Set onboard LED pin
  pinMode(WIFI_LED, OUTPUT);
  digitalWrite(WIFI_LED, HIGH);  // OFF initially

  // Turn all relays off initially (assuming active LOW)
  digitalWrite(RELAY1, HIGH);
  digitalWrite(RELAY2, HIGH);
  digitalWrite(RELAY3, HIGH);
  digitalWrite(RELAY4, HIGH);

  Serial.println("Relays initialized to OFF state.");
  Serial.println("Connecting to WiFi...");

  // Start Blynk and WiFi
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  // Wait until connected
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi Connected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}

// Individual relay control
BLYNK_WRITE(V0) {
  int state = param.asInt();
  digitalWrite(RELAY1, state ? LOW : HIGH);
  Serial.print("Relay 1: ");
  Serial.println(state ? "ON" : "OFF");
}

BLYNK_WRITE(V1) {
  int state = param.asInt();
  digitalWrite(RELAY2, state ? LOW : HIGH);
  Serial.print("Relay 2: ");
  Serial.println(state ? "ON" : "OFF");
}

BLYNK_WRITE(V2) {
  int state = param.asInt();
  digitalWrite(RELAY3, state ? LOW : HIGH);
  Serial.print("Relay 3: ");
  Serial.println(state ? "ON" : "OFF");
}

BLYNK_WRITE(V3) {
  int state = param.asInt();
  digitalWrite(RELAY4, state ? LOW : HIGH);
  Serial.print("Relay 4: ");
  Serial.println(state ? "ON" : "OFF");
}

// Master switch to control all relays
BLYNK_WRITE(V4) {
  int state = param.asInt();
  digitalWrite(RELAY1, state ? LOW : HIGH);
  digitalWrite(RELAY2, state ? LOW : HIGH);
  digitalWrite(RELAY3, state ? LOW : HIGH);
  digitalWrite(RELAY4, state ? LOW : HIGH);

  Serial.print("All Relays: ");
  Serial.println(state ? "ON" : "OFF");
}

void loop()
{
  Blynk.run();

  // WiFi LED status indicator
  if (WiFi.status() == WL_CONNECTED) {
    digitalWrite(WIFI_LED, LOW);  // ON when connected
  } else {
    digitalWrite(WIFI_LED, HIGH); // OFF when disconnected
  }
}
