```
#include <SPI.h>
#include <Adafruit_GFX.h>
#include <Adafruit_ST7789.h>

// -------- Pantalla --------
#define TFT_MOSI 13
#define TFT_MISO 12
#define TFT_SCLK 14
#define TFT_CS 15
#define TFT_DC 2
#define TFT_RST -1
#define TFT_BL 21

Clase SPI lcdSPI(VSPI);
Adafruit_ST7789 tft = Adafruit_ST7789(&lcdSPI, TFT_CS, TFT_DC, TFT_RST);

void setup() {
pinMode(TFT_BL, SALIDA);
digitalWrite(TFT_BL, ALTO);

lcdSPI.begin(TFT_SCLK, TFT_MISO, TFT_MOSI, TFT_CS);

tft.init(240, 320);
tft.setRotation(3);
tft.fillScreen(ST77XX_WHITE);

// Texto
tft.setTextColor(ST77XX_BLACK);
tft.setTextSize(2);
tft.setCursor(20, 30);
tft.println("MSG");
tft.setCursor(20, 60);
tft.println("Dilan Castro");

// 3 puntos de colores
tft.fillCircle(60, 180, 20, ST77XX_RED);
tft.fillCircle(120, 180, 20, ST77XX_GREEN);
tft.fillCircle(180, 180, 20, ST77XX_BLUE);
}

void loop() {
}
```