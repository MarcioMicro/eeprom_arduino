# eeprom_arduino

## Gravador

```
#define WE 12
#define OE 11

byte ascii[] = {
  0b01000001,
  0b01000010,
  0b01000011,
  0b01000100,
  0b01000101,
  0b01000110,
  0b01000111,
  0b01001000,
  0b01001001,
  0b01001010,
  0b01001011,
  0b01001100,
  0b01001101,
  0b01001110,
  0b01001111,
  0b01010000,
  0b01010001,
  0b01010010,
  0b01010011,
  0b01010100,
  0b01010101,
  0b01010110,
  0b01010111,
  0b01011000,
  0b01011001,
  0b01011010
};

byte braille[] = {
  0b00100000,
  0b00110000,
  0b00100100,
  0b00100110,
  0b00100010,
  0b00110100,
  0b00110110,
  0b00110010,
  0b00010100,
  0b00010110,
  0b00101000,
  0b00111000,
  0b00101100,
  0b00101110,
  0b00101010,
  0b00111100,
  0b00111110,
  0b00111010,
  0b00011100,
  0b00011110,
  0b00101001,
  0b00111001,
  0b00010111,
  0b00101101,
  0b00101111,
  0b00101011
};


void setup() {  
  for (int i=3;i<=12;i++) pinMode(i, OUTPUT);
  for (int i=14;i<=19;i++) pinMode(i, OUTPUT);
  digitalWrite(WE, HIGH);
  digitalWrite(OE, HIGH);
  delay(100);
  
  for(int i=0;i<26;i++) {
    for(int j=0; j<8; j++) {
      if (bitRead(ascii[i], j)) {
      digitalWrite(j+3, HIGH);
      } else {
        digitalWrite(j+3, LOW);
      }
    }    
    
    for(int j=0; j<6; j++) {
      if (bitRead(braille[i], j)) {
      digitalWrite(19-j, HIGH);  // Ativa a porta digital
      } else {
        digitalWrite(19-j, LOW);   // Desativa a porta digital
      }     
    }
    delay(10);    
    digitalWrite(WE, LOW);
    delayMicroseconds(1);
    digitalWrite(WE, HIGH);
    delay(50);    
  }  
}

void loop() {
  
}
```

## Leitor

```
#define WE 12
#define OE 11

byte ascii = 0b01000001;

void setup() {
  Serial.begin(115200);
  for (int i=3;i<=12;i++) pinMode(i, OUTPUT);
  for (int i=14;i<=19;i++) pinMode(i, INPUT);
  digitalWrite(WE, HIGH);
  digitalWrite(OE, LOW);
  delay(100);
  
  for(int j=0; j<8; j++) {
    if (bitRead(ascii, j)) {
    digitalWrite(j+3, HIGH);  // Ativa a porta digital
    } else {
      digitalWrite(j+3, LOW);   // Desativa a porta digital
    }
  }  
  
  for (int i=14;i<=19;i++) Serial.print(digitalRead(i));
  
}  

void loop() {
  
}
```
