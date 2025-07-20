#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#define WATER_LEVEL_PIN 15
#define RELAY_PIN 5
#define RAIN_SENSOR_PIN 4
#define BUZZER_PIN 23 
Adafruit_SSD1306 display(128, 64, &Wire, -1);
int relayState = 0;
void setup() {
  Serial.begin(115200);
  if (!display.begin(SSD1306_PAGEADDR, 0x3C)) {
    Serial.println("OLED tidak ditemukan. Periksa koneksi!");
    while (true);
  }

  display.setTextColor(SSD1306_WHITE);
  display.clearDisplay();
  display.setCursor(0, 0);
  display.setTextSize(2);

  pinMode(WATER_LEVEL_PIN, INPUT); 
  pinMode(RAIN_SENSOR_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW);

  display.print("SEDANG\nMEMBACA\nSENSOR");
  display.display();
  delay(1000);
}

void loop() {
  display.clearDisplay();
  display.setCursor(0, 0);
  int waterLevelValue = analogRead(WATER_LEVEL_PIN);
  int waterLevelPercent = map(waterLevelValue, 0, 2200, 0, 100);
  int rainSensorState = digitalRead(RAIN_SENSOR_PIN);

  Serial.print("Tingkat Air: ");
  Serial.print(waterLevelValue);
  Serial.print(" / ");
  Serial.print(waterLevelPercent);
  Serial.print("% | Sensor Hujan: ");
  Serial.println(rainSensorState);

  if (waterLevelPercent > 100) waterLevelPercent = 100;

  display.printf("Meluap:%d%%\n", waterLevelPercent);
  
  if (waterLevelPercent >= 80) {
    display.println("Bahaya");
  } else if (waterLevelPercent < 80 && waterLevelPercent > 40) {
    display.println("Siaga");
  } else {
    display.println("Aman");
  }
  
  (rainSensorState == LOW) ? display.print("Hujan") : display.print("Cerah");

 if (waterLevelPercent >= 80) {
    relayState = 1;
    digitalWrite(BUZZER_PIN, LOW);
    delay(500);
    digitalWrite(BUZZER_PIN, HIGH);
    delay(50);

  } else if (waterLevelPercent < 80 && waterLevelPercent > 40) {
    relayState = 0;
    digitalWrite(BUZZER_PIN, LOW);
    delay(200);
    digitalWrite(BUZZER_PIN, HIGH);
    delay(100);
    
  } else {
    digitalWrite(BUZZER_PIN, HIGH);
    if (rainSensorState == 0) {
      relayState = 1;
      digitalWrite(BUZZER_PIN, LOW);
      delay(50);
      digitalWrite(BUZZER_PIN, HIGH);
      delay(1000);
    } else {
      digitalWrite(RELAY_PIN, LOW);
      relayState = 0;
    }
  }

  if (relayState == 1 || rainSensorState == 0){
    display.print("\nRelay ON");
    digitalWrite(RELAY_PIN, HIGH);
  } else {
    display.print("\nRelay OFF");
    digitalWrite(RELAY_PIN, LOW);
  }
  display.display();
  delay(1000);
}
