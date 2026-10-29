#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

const int POT_PIN = A0;  // slide pot wiper
const int ENC_A = D1;    // encoder A
const int ENC_B = D2;    // encoder B
const int BTN_PIN = D3;  // button or encoder switch

Adafruit_SSD1306 display(128, 64, &Wire, -1);  // -1 = no reset pin

int count = 0;
bool inverted = false;
int lastA = HIGH;
int lastBtn = HIGH;
int lastBar = -1;
bool changed = true;

void setup() {
  pinMode(ENC_A, INPUT_PULLUP);
  pinMode(ENC_B, INPUT_PULLUP);
  pinMode(BTN_PIN, INPUT_PULLUP);
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    while (true);  // no screen at that address: stop here
  }
}

void loop() {
  // Encoder: when A goes low, B tells us the direction
  int a = digitalRead(ENC_A);
  if (a == LOW && lastA == HIGH) {
    count += (digitalRead(ENC_B) == HIGH) ? 1 : -1;
    changed = true;
  }
  lastA = a;

  // Button: flip the colours on each press
  int btn = digitalRead(BTN_PIN);
  if (btn == LOW && lastBtn == HIGH) {
    inverted = !inverted;
    display.invertDisplay(inverted);
  }
  lastBtn = btn;

  // Slide pot: 0-1023 becomes a bar 0-128 pixels wide
  int bar = map(analogRead(POT_PIN), 0, 1023, 0, 128);
  if (abs(bar - lastBar) > 1) {  // ignore tiny jitters
    lastBar = bar;
    changed = true;
  }

  if (!changed) return;  // only redraw when something moved
  changed = false;
  display.clearDisplay();
  display.setTextSize(2);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0, 0);
  display.print("Count: ");
  display.println(count);
  display.fillRect(0, 48, lastBar, 16, SSD1306_WHITE);
  display.display();
}
