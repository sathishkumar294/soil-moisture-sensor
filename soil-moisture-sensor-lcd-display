#include <LiquidCrystal.h>

// initialize the library with the numbers of the interface pins
int rs = 12, en = 7, d7 = 8, d6 = 9, d5 = 10, d4 = 11;

LiquidCrystal lcd(rs, en, d7, d6, d5, d4); /// REGISTER SELECT PIN,ENABLE PIN,D4 PIN,D5 PIN, D6 PIN, D7 PIN

int displayMode = 1; // 0 for soil moisture, 1 for name + school

int MOISTURE_DISPLAY_DURATION = 12000;
int MOISTURE_SAMPLING_INTERVAL = 250;
int noOfLoops = MOISTURE_DISPLAY_DURATION / MOISTURE_SAMPLING_INTERVAL;

String NAME = "HarsathAbinav";
char* SCHOOL = "Sri Chaitanya Techno School, CBE";

int msPin = A1; // moisture sensor is connected with the analog pin A1 of the Arduino
int msValue = 0; // moisture sensor value
int LED = 13;
boolean flag = false;
String message = String("");

void setup()

{
  pinMode(msPin, INPUT);
  pinMode(LED, OUTPUT);
  // set up the LCD's number of columns and rows:
  Serial.begin(9600);
  lcd.begin(16, 2);
  showName();
}

void showOnLCD(String message, int line) {
  line = line > 1 ? 1 : line;
  lcd.setCursor(0, line);
  lcd.print(message);
}

void showName() {
  Serial.println("showing name...");
  displayMode = 1;
  lcd.clear();
  showOnLCD("  HarsathAbinav  ", 0);
  lcd.setCursor(0, 1);
  showSchoolName();
}

void showMoisture() {
  Serial.println("showing mositure...");
  lcd.clear();
  showOnLCD("Soil Moisture", 0);
  for (int j = 1; j <= noOfLoops; j++) {
    readAndShowMoisture();
    delay(MOISTURE_SAMPLING_INTERVAL);
  }
}

void readAndShowMoisture() {
  msValue = analogRead(msPin);
  if(msValue > 500) {
    digitalWrite(LED, HIGH);
  } else {
    digitalWrite(LED, LOW);
  }
  int moisture = getSoilMoistureForVoltage(msValue);
  message = String("V = ") + msValue + "    Soil moisture: " + moisture + "%";
  Serial.println(message);
  String sMoisture = moisture < 10 ? String("0") + moisture : String(moisture);
  showOnLCD(sMoisture + "%", 1);
}

void showSchoolName()
{
  for (int letter = 0; letter <= strlen(SCHOOL) - 16; letter++) //From 0 to upto n-16 characters supply to below function
  {
    showLetters(0, letter);
  }
}

void showLetters(int printStart, int startLetter)
{
  lcd.setCursor(printStart, 1);
  for (int letter = startLetter; letter <= startLetter + 15; letter++) // Print only 16 chars in Line #2 starting 'startLetter'
  {
    lcd.print(SCHOOL[letter]);
  }
  lcd.print(" ");
  delay(400);
}

void loop()
{
  if (displayMode == 1) {
    showName();
    displayMode = 0;
  }

  if (displayMode == 0) {
    showMoisture();
    displayMode = 1;
  }

  lcd.clear();
}
// When voltage is 1023, it is dryest (moisture = 0%)
// When voltage is 230, it is wettest (moisture=100%)
int getSoilMoistureForVoltage(int v) {
  // See: https://www.wolframalpha.com/input?i=solve+1023m%2Bc%3D0+and+230m%2Bc%3D100
  int moisture =  -0.126 * v + 129;
  if (moisture > 100) moisture = 100;
  if (moisture < 0) moisture = 0;
  return moisture;
}
