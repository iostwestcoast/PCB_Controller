# PCB Controller

PCB controller based on an arduino made with a milling machine designed for a radio controlled bot

The final look of the controller:

![photo_2023-06-23_14-38-28](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/85d43026-839a-4100-af62-a7e1fb5e2c83)

https://github.com/iostwestcoast/PCB_Controller/assets/114690482/6b196048-b7b7-4629-8197-2101ce62043e

Сomponents:

- NRF+

- Arduino Pro Micro

- x2 joystick

- potentiometer

- powerbank

Work plan:

- In EasyEDA add the required electronic components and connect them
- Trace and correct the tracks for the PCB, 1 mm indentation and 0.8 mm track thickness
- Download a .dxf file and remove unnecessary vectors with the Rhino program
- Correct vectors in Rhino or ArtCAM and mirror the whole schematic
- Make milling on CNC machine, prepare file and paths in ArtCAM
- We mill the holes for the pins through, mill 0.5mm, mill the tracks 0.2mm deep, mill 0.3mm tapered
- Since the PCB is double sided and the second layer is not needed, we remove it completely with the 6mm mill
- Solder the pins to the board according to the holes and install the components
- The design can be absolutely whatever you want
- In this case I painted the front part before soldering and cut out a transparent cover plate from acrylic on a laser machine, it is fastened with screws

Scematics:

![2](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/1bf0d808-dc16-4d9d-b917-b0bf0e16157b)

PCB:

![1](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/76ff96c3-6c17-450c-9069-abed590cccf5)

ArtCAM:

![image](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/abe67164-a6a6-4d6e-8ddd-eefd4e30cc09)

Milling:

![photo_2023-06-23_14-38-55](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/4859df42-341a-4ddd-9afa-ec1ed4eedf05)

Painting:

![photo_2023-06-23_14-39-07](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/aadca1a2-3c22-46ce-8ad9-7760b690b09c)

Soldering:

![photo_2023-06-23_14-39-04](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/75bf24a9-a95a-4780-bb4c-390b822608ba)

Acrylic panel:

![photo_2023-06-23_14-38-37 (2)](https://github.com/iostwestcoast/PCB_Controller/assets/114690482/248dbfd5-9ad9-489f-a633-9249739ea075)

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
