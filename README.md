# my-first-mini-project
#include <Wire.h>
#include <Adafruit_MLX90614.h>

// Define pins
#define BUZZER_PIN 8
#define GAS_SENSOR_PIN A1

// Thresholds
#define TEMP_THRESHOLD 30.0   // Temperature in Celsius
#define GAS_THRESHOLD 300   // Gas level (adjust based on your MQ-2)

// Create sensor object
Adafruit_MLX90614 thermistor;

void setup() {
  Serial.begin(9600);
  pinMode(BUZZER_PIN, OUTPUT);
  
  if (!thermistor.begin()) {
    Serial.println("Could not find MLX90614 sensor.");
    while (1);
  }
}

void loop() {
  double temperature = thermistor.readObjectTempC();
  int gasLevel = analogRead(GAS_SENSOR_PIN);
  
  bool alert = false;
  
  // Check temperature threshold
  if (temperature > TEMP_THRESHOLD) {
    alert = true;
    digitalWrite(BUZZER_PIN, HIGH);
  }
  
  // Check gas threshold
  if (gasLevel > GAS_THRESHOLD) {
    alert = true;
    digitalWrite(BUZZER_PIN, HIGH);
  }
  
  if (!alert) {
    digitalWrite(BUZZER_PIN, LOW);
  }
  
  delay(5000); // Wait 5 seconds before next reading
