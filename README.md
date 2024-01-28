#define BLYNK_TEMPLATE_ID "TMPL30puDpxgf"
#define BLYNK_TEMPLATE_NAME "Test"
#define BLYNK_AUTH_TOKEN "7zGXQqe1h-L1wCdm4OJtoaJFe0-mmLtv"

char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "Wokwi-GUEST";   //Enter wifi name
char pass[] = ""; //Enter wifi-pass
#define relay1 0
#define relay2 2


unsigned int level = 0;
#define PIN_TRIG 26
#define PIN_ECHO 25


#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>


BLYNK_WRITE(V0) {
  bool value1 = param.asInt();
  if (value1 == 0) {
    digitalWrite(relay1, LOW);
  } else {
    digitalWrite(relay1, HIGH);
  }
}

BLYNK_WRITE(V1) {
  bool value2 = param.asInt();
  if (value2 == 0) {
    digitalWrite(relay2, LOW);
  } else {
    digitalWrite(relay2, HIGH);
  }
}

void setup() {
  pinMode(relay1, OUTPUT);
  pinMode(relay2, OUTPUT);

  digitalWrite(relay1, HIGH);
  digitalWrite(relay2, HIGH);

  pinMode(PIN_TRIG, OUTPUT);
  pinMode(PIN_ECHO, INPUT);

  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
 Serial.begin(115200);

}
void loop() {
    Blynk.run();
  digitalWrite(PIN_TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(PIN_TRIG, LOW);

  // Read the result:
  int duration = pulseIn(PIN_ECHO, HIGH);
  Serial.print("Distance in CM: ");
int c=  duration / 58;
  Serial.println(c);
  Serial.print("Distance in inches: ");
  Serial.println(duration/147);
  level = c/100 ;
  Blynk.virtualWrite(V2,level);
}
