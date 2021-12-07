# OLED Arduino Animation
These are the files and steps needed to create an OLED animation using Arduino.

## How to make it

**1. Connect the OLED display to Arduino**

| Arduino  | OLED        |
| ---------|-------------|
| 3V       | VCC      |
| GND      | GND      |
| SDA      | SDA      |
| SCL      | SCL      |

**2. Download gif and split it into frames**

![alt text](https://github.com/tigrisli/oled-animation/blob/main/heart.gif)

I used https://ezgif.com/split to split my gif into single image frames.

**3. Convert images to X Bitmap with Processing**

![alt text](https://github.com/tigrisli/oled-animation/blob/main/images/bitmap-convertor.png)

- Open .pde file in Processing
- Hit the play button and run the code
- Click on the gray square
- Select image file you want to convert
- The C header file will be exported and saved in the same location as your image file

**4. Open oled-animated-heart.ino with Arduino**

This code uses the **U8g2 library** for monochrome displays. The library can be installed from the Arduino IDE by going to
**Sketch > Include Library > Manage Libraries.** Then search for **u8g2 by Oliver** and click Install.

**5. Create an array of images and store it in the flash**

Open the converted C Header file, copy & paste the image data in between the curly brackets and paste each image file in between the curly brackets of the Arduino code.

```javascript
const static unsigned char heart_bits[7][512] PROGMEM = {
  { // heart 1 - frame 0
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
    ...
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
  },
  { // heart 2 - frame 1
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
    ...
    00x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
  },
  { // heart 3 - frame 2
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
    ...
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
  }
};
```

**6. Create a function to play images in our desired sequence order**

```javascript
void drawAnimation(void) {
 
  static uint8_t frame = 0 ;

  switch(frame){
    case 0: u8g2.clearBuffer(); u8g2.setFont(u8g2_font_tenthinguys_tr); u8g2.drawStr(32,26,"I love you"); u8g2.sendBuffer(); delay(1500);break;
    case 1: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[0]);break;
    case 2: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[1]);break;
    case 3: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[2]);break;
    case 4: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[3]);break;
    case 5: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[4]);break;
    case 6: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[6]);break;
    case 7: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[6]);break;
    case 8: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[6]);break;
    case 9: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[5]);break;
    case 10: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[4]);break;
    case 11: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[3]);break;
    case 12: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[2]);break;
    case 13: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[1]);break;
    case 14: u8g2.drawXBMP( 0, 15, heart_width, heart_height, heart_bits[0]);break;
  }
  frame++;
  
    if(frame>15){
      frame = 0;
    }
}
```

**7. Upload onto Arduino board**
