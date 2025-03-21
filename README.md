# Pharma-3-year-CATCH-25-Hackathon-
https://youtu.be/Itc2p_czm8I?si=qiH05Hcwdk-5WIRO
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// OLED display settings
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Pin definitions
const int pulsePin = A0;   // Heart pulse sensor connected to A0
const int buzzerPin = 8;   // Buzzer connected to digital pin 8

// Variables
int pulseValue = 0;
int bpm = 0;
int threshold = 550; // Adjust according to your sensor output

void setup() {
  Serial.begin(9600);

  // Initialize OLED display
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for (;;);
  }
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  pinMode(buzzerPin, OUTPUT);
}

void loop() {
  pulseValue = analogRead(pulsePin);

  // Simulated BPM (for now random, replace with real BPM logic)
  if (pulseValue > threshold) {
    bpm = random(60, 140); // Simulating BPM between 60 and 140
  }

  Serial.print("Pulse Sensor Value: ");
  Serial.print(pulseValue);
  Serial.print(" | BPM: ");
  Serial.println(bpm);

  display.clearDisplay();
  display.setCursor(0, 0);
  display.println("Health Monitor");
  display.setCursor(0, 20);
  display.print("Heart Rate: ");
  display.print(bpm);
  display.println(" BPM");

  if (bpm < 80) {
    display.setCursor(0, 40);
    display.println("Status: LOW BPM");
    tone(buzzerPin, 500); // Low beep
  }
  else if (bpm >= 80 && bpm <= 120) {
    display.setCursor(0, 40);
    display.println("Status: NORMAL");
    noTone(buzzerPin); // No buzzer
  }
  else if (bpm > 120) {
    display.setCursor(0, 40);
    display.println("Status: HIGH BPM");
    tone(buzzerPin, 1500); // High pitch beep
  }

  display.display();
  delay(800);
}
