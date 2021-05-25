````
#include <U8glib.h>
#include <Adafruit_MLX90614.h>
#include<Wire.h>
#include <english.h>
#include <sound.h>
#include <TTS.h>
#define PWM 3
#define LED 13                          

TTS text2speech(PWM);  // default is digital pin 10

U8GLIB_PCD8544 u8g(11,10,8,9,7);
Adafruit_MLX90614 mlx = Adafruit_MLX90614();
void setup() {
  // put your setup code here, to run once:
   // put your setup code here, to run once:
  Serial.begin(9600);
  mlx.begin();
}
void draw(){
  const int threshold=95;
  float a=mlx.readAmbientTempF();
  if(a>threshold){
  u8g.setFont(u8g_font_gdr25r);
  char buf[9];
sprintf (buf, "%f", a);
u8g.drawStr(8,10,"Temperature=");
u8g.drawStr(13,30, buf);
u8g.drawStr(20,36,"F");
 digitalWrite(LED, !digitalRead(LED));

  text2speech.setPitch(6);
  text2speech.sayText(buf);
  delay(500);
  }
}
void loop() {  

  u8g.firstPage();
  do {  
    draw();
  } while( u8g.nextPage() );
  delay(1000);   
}
