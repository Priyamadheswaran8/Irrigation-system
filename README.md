#define BLYNK_TEMPLATE_ID "TMPL3RqwOy1wT"
#define BLYNK_TEMPLATE_NAME "soilmoisturedetect"
#define BLYNK_AUTH_TOKEN "owitkOesyBMMRN_icpK94_-kRqxN7FFi"

#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

int soilmoisture = 34;
int relay = 25;
int vp = V0;

char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "OPPO A31";
char pass[] = "priya123";

BlynkTimer timer;
int moisturevalue = 0;

void sendsensor() {
  int sensorval = analogRead(soilmoisture);
  moisturevalue = 100 - ((sensorval / 4095.00) * 100);
  Serial.print("moisture: ");
  Serial.println(moisturevalue);
  Blynk.virtualWrite(vp, moisturevalue);
}

void automaticControl() {
  if (moisturevalue >= 0 && moisturevalue < 80) {
    digitalWrite(relay, HIGH);  // Activate the relay
  }
  else {
    digitalWrite(relay, LOW);  // Deactivate the relay
  }
}

void setup() {
  Serial.begin(9600);
  pinMode(soilmoisture, INPUT);
  pinMode(relay, OUTPUT);
  Blynk.begin(auth, ssid, pass);
  timer.setInterval(1000L, sendsensor);
  timer.setInterval(1000L, automaticControl);
}

void loop() {
  Blynk.run();
  timer.run();
}