#include <SoftwareSerial.h>
#include <MP3.h>
long distancia;
long tiempo;

/** define mp3 class */
MP3 mp3;

void setup()
{
  
  Serial.begin(9600);
  pinMode(9, OUTPUT); /*activación del pin 9 como salida: para el pulso ultrasónico*/
  pinMode(8, INPUT); /*activación del pin 8 como entrada: tiempo del rebote del ultrasonido*/
  
  /** begin function */
  mp3.begin(MP3_SOFTWARE_SERIAL);    // select software serial
//  mp3.begin();                       // select hardware serial(or mp3.begin(MP3_HARDWARE_SERIAL);)
  
  /** set volum to the MAX */
  mp3.volume(0x1F);
  
  /** set MP3 Shield CYCLE mode */
  mp3.set_mode(MP3::CYCLE);
  
  /** play music in sd, '0001' for first music */
  mp3.play_sd(0x0001);
  
  /** play music in USB-disk */ 
  //mp3.play_usb_disk(0x0001);
  
  /** play music in SPI FLASH */ 
  //mp3.play_spi_flash(0x0001);
}

void loop()
{
	  
  digitalWrite(9,LOW); /* Por cuestión de estabilización del sensor*/
  delayMicroseconds(5);
  digitalWrite(9, HIGH); /* envío del pulso ultrasónico*/
  delayMicroseconds(10);
  tiempo=pulseIn(8, HIGH); /* Función para medir la longitud del pulso entrante. Mide el tiempo que transcurrido entre el envío
  del pulso ultrasónico y cuando el sensor recibe el rebote, es decir: desde que el pin 12 empieza a recibir el rebote, HIGH, hasta que
  deja de hacerlo, LOW, la longitud del pulso entrante*/
  distancia= int(0.017*tiempo); /*fórmula para calcular la distancia obteniendo un valor entero*/
  /*Monitorización en centímetros por el monitor serial*/
if(distancia<30){
  Serial.println("Mira mi huevo");
  delay(1000);
  }
}
