# PCB Controller

PCB controller based on an arduino made with a milling machine designed for a radio controlled bot.

The final look of the controller:

![photo_2023-06-23_14-38-28](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/85d43026-839a-4100-af62-a7e1fb5e2c83)



Сomponents:

- NRF+

- Arduino Pro Micro

- x2 joystick

- potentiometer

- powerbank

PCB Controller code:

```C++
#include <SPI.h>
#include <nRF24L01.h>
#include <RF24.h>

RF24 radio(8, 10);
int data[3];

#define PIN_VRX A2
#define PIN_VRY A3

void setup() {
  Serial.begin(9600);

  radio.begin();
  radio.setChannel(1);
  radio.setDataRate(RF24_1MBPS);
  radio.setPALevel(RF24_PA_HIGH);
  radio.openWritingPipe(0x2234567890LL);
  radio.stopListening();

  pinMode(A1, INPUT);
}

void loop() {
  int potval = analogRead(A1);

  radio.write(&data, sizeof(data));
  delay(50);

  Serial.println(potval);
     
  int valX = analogRead(PIN_VRX);
  int valY = analogRead(PIN_VRY);

  Serial.print("X = ");
  Serial.println(analogRead(PIN_VRX));

  Serial.print("Y = ");
  Serial.println(analogRead(PIN_VRY));

  data[0] = potval;
  data[1] = valX;
  data[2] = valY;
}
```
