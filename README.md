# UID-READING (Arduino MEGA)
Source code to read RFID Card and TAG UID
#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN 53
#define RST_PIN 5

MFRC522 mfrc522(SS_PIN, RST_PIN);

void setup() {
  Serial.begin(9600);
  SPI.begin();
  mfrc522.PCD_Init();

  Serial.println("Place RFID card near reader...");
}

void loop() {
  // Look for new cards
  if (!mfrc522.PICC_IsNewCardPresent()) {
    return;
  }

  // Select one card
  if (!mfrc522.PICC_ReadCardSerial()) {
    return;
  }

  Serial.print("UID Tag: ");

  for (byte i = 0; i < mfrc522.uid.size; i++) {
    Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? "0" : "");
    Serial.print(mfrc522.uid.uidByte[i], HEX);
    Serial.print(" ");
  }

  Serial.println();

  mfrc522.PICC_HaltA();
}
