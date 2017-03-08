# PULSOGAME-4.0
Juego de pulso con arduino BT version 4.0

//PULSOGAME  Volume Booster
//AÑO 2016
//VERSION: 2.0
//Autores: Hiroshi Takey Franco y Walter Mauricio Mauricio Matinez Nogales 

#include "pitches.h"
#include <SoftwareSerial.h>

SoftwareSerial Serial_2 (0, 1);   // Crea nueva conexion- Pin2(RX) a TX y Pin3(TX) a RX
String Mensaje;                  // Variable de cadena de caracteres para almacenar el mensaje
//_____________________________________________________________________
//notes in the melody:
int melody1[] = {
  NOTE_E5, NOTE_E3, NOTE_B4, NOTE_C5, NOTE_D5, NOTE_E5, NOTE_D5, NOTE_C5, NOTE_B4, NOTE_A4, NOTE_A3, NOTE_A4, NOTE_C5, NOTE_E5, NOTE_A3, NOTE_D5,
  NOTE_C5, NOTE_B4, NOTE_E4, NOTE_G4, NOTE_C5, NOTE_D5, NOTE_E3, NOTE_E5, NOTE_E3, NOTE_C5, NOTE_A3, NOTE_A4, NOTE_A3, NOTE_A4, NOTE_A3, NOTE_B2, 
  NOTE_C3, NOTE_D3, NOTE_D5, NOTE_F5, NOTE_A5, NOTE_C5, NOTE_C5, NOTE_G5, NOTE_F5, NOTE_E5, NOTE_C3, 0, NOTE_C5, NOTE_E5, NOTE_A4, NOTE_G4, NOTE_D5,
  NOTE_C5, NOTE_B4, NOTE_E4, NOTE_B4, NOTE_C5, NOTE_D5, NOTE_G4, NOTE_E5, NOTE_G4, NOTE_C5, NOTE_E4, NOTE_A4, NOTE_E3, NOTE_A4, 0, 
  NOTE_E5, NOTE_E3, NOTE_B4, NOTE_C5, NOTE_D5, NOTE_E5, NOTE_D5, NOTE_C5, NOTE_B4, NOTE_A4, NOTE_A3, NOTE_A4, NOTE_C5, NOTE_E5, NOTE_A3, NOTE_D5,
  NOTE_C5, NOTE_B4, NOTE_E4, NOTE_G4, NOTE_C5, NOTE_D5, NOTE_E3, NOTE_E5, NOTE_E3, NOTE_C5, NOTE_A3, NOTE_A4, NOTE_A3, NOTE_A4, NOTE_A3, NOTE_B2, 
  NOTE_C3, NOTE_D3, NOTE_D5, NOTE_F5, NOTE_A5, NOTE_C5, NOTE_C5, NOTE_G5, NOTE_F5, NOTE_E5, NOTE_C3, 0, NOTE_C5, NOTE_E5, NOTE_A4, NOTE_G4, NOTE_D5,
  NOTE_C5, NOTE_B4, NOTE_E4, NOTE_B4, NOTE_C5, NOTE_D5, NOTE_G4, NOTE_E5, NOTE_G4, NOTE_C5, NOTE_E4, NOTE_A4, NOTE_E3, NOTE_A4, 0,
  NOTE_E4, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_C4, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_D4, NOTE_E3, NOTE_GS2, NOTE_E3, NOTE_B3, NOTE_E3, NOTE_GS2, NOTE_E3,
  NOTE_C4, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_A3, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_GS3, NOTE_E3, NOTE_GS2, NOTE_E3, NOTE_B3, NOTE_E3, NOTE_GS2, NOTE_E3, 
  NOTE_E4, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_C4, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_D4, NOTE_E3, NOTE_GS2, NOTE_E3, NOTE_B3, NOTE_E3, NOTE_GS2, NOTE_E3,
  NOTE_C4, NOTE_E3, NOTE_E4, NOTE_E3, NOTE_A4, NOTE_E3, NOTE_A2, NOTE_E3, NOTE_GS4, NOTE_E3, NOTE_GS2, NOTE_E3, NOTE_GS2, NOTE_E3, NOTE_GS2, NOTE_E3,
  NOTE_E5, NOTE_E3,
};

//note durations: 4 = quarter note, 8 = eighth note, etc
int noteDurations1[] = {
  8,8,8,8,8,16,16,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,4,8,8,16,16,8,8,8,8,8,8,8,16,16,8,8,8,8,8,8,8,8,8,8,8,8,8,8,4,4,
  8,8,8,8,8,16,16,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,4,8,8,16,16,8,8,8,8,8,8,8,16,16,8,8,8,8,8,8,8,8,8,8,8,8,8,8,4,4,
  8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,
  8,8,8,
};
//______________________________________________________________________
int melody3[] = {
  NOTE_C4, NOTE_C5, NOTE_A3, NOTE_A4, NOTE_AS3, NOTE_AS4, 0, 0, NOTE_C4, NOTE_C5, NOTE_A3, NOTE_A4, NOTE_AS3, NOTE_AS4, 0, 0,
  NOTE_F3, NOTE_F4, NOTE_D3, NOTE_D4, NOTE_DS3, NOTE_DS4, 0, 0, NOTE_F3, NOTE_F4, NOTE_D3, NOTE_D4, NOTE_DS3, NOTE_DS4, 0,
  0, NOTE_DS4, NOTE_CS4, NOTE_D4, NOTE_CS4, NOTE_DS4, NOTE_DS4, NOTE_GS3, NOTE_G3, NOTE_CS4, NOTE_C4, NOTE_FS4, NOTE_F4, NOTE_E3, NOTE_AS4, NOTE_A4,
  NOTE_GS4, NOTE_DS4, NOTE_B3, NOTE_AS3, NOTE_A3, NOTE_GS3, 0, 0, 0
  };
  int tempo3[] = {
  12, 12, 12, 12, 12, 12, 6, 3, 12, 12, 12, 12, 12, 12, 6, 3, 12, 12, 12, 12, 12, 12, 6, 3, 12, 12, 12, 12, 12, 12, 6, 6, 18, 18, 18, 6, 6,
  6, 6, 6, 6, 18, 18, 18, 18, 18, 18, 10, 10, 10, 10, 10, 10,3, 3, 3
  };
//_____________________________________________________________________
const int gameover[] = {15,                                               // Array for Game over song
  NOTE_C4, 8, NOTE_H, 8, NOTE_H, 8, NOTE_G3, 8, NOTE_H, 4, NOTE_E3, 4, NOTE_A3, 6, NOTE_B3, 6, NOTE_A3, 6, NOTE_GS3, 6, NOTE_AS3, 6, NOTE_GS3, 6, NOTE_G3, 8, NOTE_F3, 8, NOTE_G3, 4};

int melody[] = {
  NOTE_E7, NOTE_E7, 0, NOTE_E7, 0, NOTE_C7, NOTE_E7, 0, NOTE_G7, 0, 0,  0, NOTE_G6, 0, 0, 0, NOTE_C7, 0, 0, NOTE_G6, 0, 0, NOTE_E6, 0, 0, NOTE_A6, 0, NOTE_B6, 0, NOTE_AS6, NOTE_A6, 0, 
  NOTE_G6, NOTE_E7, NOTE_G7, NOTE_A7, 0, NOTE_F7, NOTE_G7, 0, NOTE_E7, 0,NOTE_C7, NOTE_D7, NOTE_B6, 0, 0,NOTE_C7, 0, 0, NOTE_G6, 0, 0, NOTE_E6, 0, 0, NOTE_A6, 0, NOTE_B6, 0, NOTE_AS6, NOTE_A6, 0, 
  NOTE_G6, NOTE_E7, NOTE_G7, NOTE_A7, 0, NOTE_F7, NOTE_G7, 0, NOTE_E7, 0,NOTE_C7, NOTE_D7, NOTE_B6, 0, 0,
};
//Mario main them tempo
int tempo[] = {
  12, 12, 12, 12,12, 12, 12, 12, 12, 12, 12, 12,12, 12, 12, 12, 12, 12, 12, 12,12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 9, 9, 9, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12,
  12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 9, 9, 9, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12, 12,
};
//________________________________________________________________________________
int frecuencia=220;    // frecuencia correspondiente a la nota La
int contador;          // variable para el contador
float n = 1.059;         // constante para multiplicar frecuencias // 1.059
float l = 1.159;         // constante para multiplicar frecuencias // 1.159
float m = 1.240;         // constante para multiplicar frecuencias // 1.240
//_________________________________________________________________________________
int duracion=250; //Duración del sonido
int fMin=2000; //Frecuencia más baja que queremos emitir
int fMax=4000; //Frecuencia más alta que queremos emitir
int i=0;
//_________________________________________________________________________________
const int ledPin =  13; 
unsigned long tiempo = 0; 
unsigned long t_actualizado = 0;
unsigned long t_delay = 0;

const byte timer = A0;
const byte timer1 = A1;
const byte timer2 = A2;
const byte timer3 = A3;
const byte lineajuego = 2;
const byte lineameta = 3;//const byte led = 12;
const byte piezowinlos = A4;
const byte ledPin1 =  A5;
int inic = 12;

int lineajueg = 0;
int conta = 0;  // int conta1 = 0; 
int conta2 = 0; 
int conta3 = 0;

int melody2[] = {NOTE_C4, NOTE_G3,NOTE_G3, NOTE_A3, NOTE_G3,0, NOTE_B3, NOTE_C4};

int noteDurations[] = { 4, 8, 8, 4,4,4,4,4 };
//_____________________________________________________________________________________________________________
int ledState = 0;             // ledState used to set the LED
long previousMillis = 0;        // will store last time LED was updated
long seg0 = 970;           // medio segundo  ON
long seg1 = 30;         // cinco segundos OFF
//__________________________________________________________________________________________________________
//__________DEFINICION DE FRECUENCIA DEL TONO DE MARIO Y VELOCIDAD__________________________________________
void buzz(int targetPin, long frequency, long length) 
{
  long delayValue = 1000000/frequency/2; // calculate the delay value between transitions
  long numCycles = frequency * length/ 1000; // calculate the number of cycles for proper timing
  for (long i=0; i < numCycles; i++) {digitalWrite(targetPin,HIGH); delayMicroseconds(delayValue); digitalWrite(targetPin,LOW); delayMicroseconds(delayValue);}
}
 void(*resetFunc) (void) = 0;
//________________MUSIK MARIO_______________________________________________
void segundo() { t_delay = 1000;
  int size = sizeof(melody) / sizeof(int);
  for (int thisNote = 0; thisNote < size; thisNote++) 
   {
    int noteDuration = 1000/tempo[thisNote]; buzz(piezowinlos, melody[thisNote],noteDuration);
    int pauseBetweenNotes = noteDuration * 1;  // 0.95
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
    
    delay(pauseBetweenNotes);
    buzz(piezowinlos, 0,noteDuration);
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0) { perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0) { ganee();}
           tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
  }}
//_________________________________________________________________________
void segundoo() { t_delay = 750;
//________________MUSIK MARIO Acelerada______________________________________
  int size = sizeof(melody3) / sizeof(int);
  for (int thisNote = 0; thisNote < size; thisNote++) 
   { 
    int noteDuration = 1000/tempo3[thisNote]; buzz(piezowinlos, melody3[thisNote],noteDuration);
    int pauseBetweenNotes = noteDuration * 1;  // 0.95
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
    
    delay(pauseBetweenNotes); buzz(piezowinlos, 0,noteDuration);
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
       tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
  }}
//________________MUSIK MARIO Acelerada al hard_____________________________________________________________________________________________________________
void segundooo() { t_delay = 500;
  int size = sizeof(melody) / sizeof(int);
  for (int thisNote = 0; thisNote < size; thisNote++) 
   { 
    int noteDuration = 500/tempo[thisNote]; buzz(piezowinlos, melody[thisNote],noteDuration);
    int pauseBetweenNotes = noteDuration * 1;
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
    
    delay(pauseBetweenNotes); buzz(piezowinlos, 0,noteDuration);

  tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
    }}
//________________MUSIK MARIO Acelerada______________________________________
void segundoooo() {t_delay = 250;
  int size = sizeof(melody3) / sizeof(int);
  for (int thisNote = 0; thisNote < size; thisNote++) 
   { 
    int noteDuration = 500/tempo3[thisNote]; buzz(piezowinlos, melody3[thisNote],noteDuration);
    int pauseBetweenNotes = noteDuration * 1;
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
    
    delay(pauseBetweenNotes); buzz(piezowinlos, 0,noteDuration);

  tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdist();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganee();}
       tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
  }}    
//__________________________________________________________________________________________________________________________________________________________
void segP() { t_delay = 1000;
  for (int thisNote1 = 0; thisNote1 < 1000; thisNote1++){
    int noteDuration1 = 1000/noteDurations1[thisNote1];
    tone(piezowinlos, melody1[thisNote1],noteDuration1);
    int pauseBetweenNotes1 = noteDuration1 * 1.10;
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdistee(); }
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganeee(); }
        
    delay(pauseBetweenNotes1);
    noTone(piezowinlos);
    
  tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdistee(); }
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganeee(); }
   tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
    conta++;
  if (conta==190){conta = 0; segP1();}
    }}
//_____________________________________________________________________________________________________________
void segP1() { t_delay = 500;
  for (int thisNote1 = 0; thisNote1 < 1000; thisNote1++){
    int noteDuration1 = 1000/noteDurations1[thisNote1];
    tone(piezowinlos, melody1[thisNote1],noteDuration1);
    int pauseBetweenNotes1 = noteDuration1 * 0.80;
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdistee(); }
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganeee(); }
    
    delay(pauseBetweenNotes1);
    noTone(piezowinlos);

   tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdistee(); }
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganeee(); }
  tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
    conta++;
  if (conta==180){conta = 0; segP2();}
    }}
//________________________________________________________________________
void segP2() { t_delay = 250;
  for (int thisNote1 = 0; thisNote1 < 1000; thisNote1++){
    int noteDuration1 = 1000/noteDurations1[thisNote1];
    tone(piezowinlos, melody1[thisNote1],noteDuration1);
    int pauseBetweenNotes1 = noteDuration1 * 0.60;
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdistee(); }
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganeee();}
    delay(pauseBetweenNotes1);
    noTone(piezowinlos);

   tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdistee();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ ganeee();}
  tiempo = millis();
  if(tiempo > t_actualizado + t_delay) {t_actualizado = tiempo; digitalWrite(ledPin, !digitalRead(ledPin));}
    conta++;
  if (conta==195){conta = 0; perdiste();}
    }}
//________________________________________________________________________-------------------------------------------------------------------------------------------------------------------------------AQUI
void opera0() { seg0 = 30;  seg1 = 970; // tiempo de 1 min 
  while(1){
  unsigned long currentMillis = millis(); if (ledState == 0) {
    if(currentMillis - previousMillis > seg0) {previousMillis = currentMillis; ledState = 1;
            digitalWrite(ledPin1, 0); digitalWrite(ledPin, 1); seg1 = seg1 - 10;}
}else {
    if(currentMillis - previousMillis > seg1) {previousMillis = currentMillis; ledState = 0;
          digitalWrite(ledPin1, 1); digitalWrite(ledPin, 0); seg0 = seg0 + 0;}                                   
}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdiste(); }
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ gane();}

    if (seg1 <= 100){ perdiste();}
    }}
//________________________________________________________________________-------------------------------------------------------------------------------------------------------------------------------AQUI
void opera01() { seg0 = 30;  seg1 = 970; // tiempo de 1:20 min 
  while(1){ 
  unsigned long currentMillis = millis(); if (ledState == 0) {
    if(currentMillis - previousMillis > seg0) {previousMillis = currentMillis; ledState = 1;
            digitalWrite(ledPin1, 0); digitalWrite(ledPin, 1); seg1 = seg1 - 20;}
}else {
    if(currentMillis - previousMillis > seg1) {previousMillis = currentMillis; ledState = 0;
          digitalWrite(ledPin1, 1); digitalWrite(ledPin, 0); }                                   //
}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdiste();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ gane();}

    if (seg1 <= 100){ perdiste();}
    }}
//________________________________________________________________________-------------------------------------------------------------------------------------------------------------------------------AQUI
void opera03() { seg0 = 30;  seg1 = 970; // tiempo de 1:20 min 
  while(1){ 
  unsigned long currentMillis = millis(); if (ledState == 0) {
    if(currentMillis - previousMillis > seg0) {previousMillis = currentMillis; ledState = 1;
            digitalWrite(ledPin1, 0); digitalWrite(ledPin, 1); seg1 = seg1 - 5;}
}else {
    if(currentMillis - previousMillis > seg1) {previousMillis = currentMillis; ledState = 0;
          digitalWrite(ledPin1, 1); digitalWrite(ledPin, 0); }                                   //
}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ perdiste();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0){ gane();}

    if (seg1 <= 100){ perdiste(); }
    }}    
//________________________________________________________________________-------------------------------------------------------------------------------------------------------------------------------AQUI
void opera02() { digitalWrite(ledPin, 0); // tiempo de 1 min
  seg0 = 20000;  seg1 = 0; delay(100);
  while(1){ if (conta3 == 4){ conta3 = 0; perdiste(); }
  
  unsigned long currentMillis = millis(); if (ledState == 0) {
    if(currentMillis - previousMillis > seg0) {previousMillis = currentMillis; ledState = 1; }
}else {
    if(currentMillis - previousMillis > seg1) {previousMillis = currentMillis; ledState = 0; conta3 = conta3 + 1; } 
}
//_______________SI TOCO EL ALAMBRE _____________________________________________
    if (digitalRead(lineajuego) == 0){ conta3 = 0; perdiste();}
//_____________SI LLEGO A LA META_______________________________________     
    if (digitalRead(lineameta) == 0) { conta3 = 0; gane(); }
    }} 
    
//_____________SI TE ACABO EL TIEMPO_______________________________________ 
void perdist() { Serial.println("  Perdiste"); Serial.print(" You lose!! lastima");
  digitalWrite(ledPin1, 1); delay(400);digitalWrite(ledPin1, 0); delay(100);
   conta = 0; digitalWrite(ledPin, 0);
    for (int thisNote = 1; thisNote < (gameover[0] * 2 + 1); thisNote = thisNote + 2) {
    tone(piezowinlos, gameover[thisNote], (1000/gameover[thisNote + 1]));// Play the single note
    delay((1000/gameover[thisNote + 1]) * 1.35); noTone(piezowinlos);
    } delay(500); Mensaje = "0";
    delay(250); resetFunc();
}
//_____________SI TE ACABO EL TIEMPO 02_____________________________________ 
void perdistee() { Serial.println("  Perdiste"); Serial.print(" You lose!! lastima");
  digitalWrite(ledPin1, 1); delay(400);digitalWrite(ledPin1, 0); delay(100);
  conta = 0; digitalWrite(ledPin, 0);
   for(contador=0,frecuencia=220;contador<12;contador++) {
    frecuencia=frecuencia*n;        // actualiza la frecuencia
    tone(piezowinlos,frecuencia);   // emite el tono
    delay(100);                     // lo mantiene 1.5 segundos
    noTone(piezowinlos);            // para el tono
    delay(20);                      // espera medio segundo
        } delay(500); Mensaje = "0";
        delay(250); resetFunc();
}
//_____________SI TE ACABO EL TIEMPO 03_____________________________________ 
void perdiste() { Serial.println("  Perdiste"); Serial.print(" You lose!! lastima");
  digitalWrite(ledPin1, 1); delay(1100);digitalWrite(ledPin1, 0); delay(100);
  conta = 0; digitalWrite(ledPin, 0);

        delay(500); Mensaje = "0";
        delay(250); resetFunc();
}
//_____________SI LLEGUE AL FINAL OPERA_______________________________________ 
void gane() { Serial.println("  Ganaste"); Serial.print("  You Win!!"); 
    for (int ad=0; ad<=10; ad++){ digitalWrite(ledPin, 1); digitalWrite(ledPin1, 1); delay(250);
                                  digitalWrite(ledPin, 0); digitalWrite(ledPin1, 0); delay(250); }   
   fMin=1500; fMax=3000;
  for (int zz=0; zz<=30; zz++) {
  for (i=fMin;i<=fMax; i++) tone(piezowinlos, i, duracion);
  //sonido más grave
  for (i=fMax;i>=fMin; i--) tone(piezowinlos, i, duracion);  
}
  fMin=1000; fMax=2000;
  for (int zz=0; zz<=30; zz++) {
  for (i=fMin;i<=fMax; i++) tone(piezowinlos, i, duracion);
  //sonido más grave
  for (i=fMax;i>=fMin; i--) tone(piezowinlos, i, duracion);  
} delay(200); Mensaje = "0";
  delay(250); resetFunc();
}
//_____________SI LLEGUE AL FINAL MARIO BROSS_______________________________________ 
void ganee() { Serial.println("  Ganaste"); Serial.print("  You Win!!");
    for (int ac=0; ac<=30; ac++){ digitalWrite(ledPin, 1); digitalWrite(ledPin1, 1); delay(50);
                                  digitalWrite(ledPin, 0); digitalWrite(ledPin1, 0); delay(50); } 
    digitalWrite(ledPin, 1); digitalWrite(ledPin1, 1); delay(500);
    digitalWrite(ledPin, 0); digitalWrite(ledPin1, 0); delay(50); ganees(); }

void ganees() {    
      for (int thisNote = 0; thisNote < 8; thisNote++) {
        int noteDuration = 1000/noteDurations[thisNote];
        tone(piezowinlos, melody2[thisNote],noteDuration);

        int pauseBetweenNotes = noteDuration * 1.40;
        delay(pauseBetweenNotes); noTone(piezowinlos); 
      } conta2 = conta2 + 1;
      if (conta2 == 3){ conta2 = 0; delay(1000); Mensaje = "0";
                        delay(250); resetFunc(); }
       ganees(); 
    } 
//_____________SI LLEGUE AL FINAL TRETIS_______________________________________ 
void ganeee() { Serial.println("  Ganaste"); Serial.print("  You Win!!"); 
    for (int ab=0; ab<=20; ab++){ digitalWrite(ledPin, 1); digitalWrite(ledPin1, 1); delay(100);
                                  digitalWrite(ledPin, 0); digitalWrite(ledPin1, 0); delay(100); }   
  for (int bf=0; bf<=4; bf++) {
    for(contador=0, frecuencia=220; contador<20;contador++) {
     // for (int bd=0; bd<=1; bd++) {
        frecuencia=frecuencia*m;     // actualiza la frecuencia
        tone(piezowinlos, frecuencia); // emite el tono
        delay(50);                 // lo mantiene 1.5 segundos
        noTone(piezowinlos);          // para el tono
        delay(20);                  // espera medio segundo
    } } // }
    delay(200); Mensaje = "0";
    delay(250); resetFunc();
}    
//______________MENSAJES BT_____________________________________________
void preguntas(){
    while (Serial_2.available()) { delay(5); char c = Serial_2.read(); Mensaje += c; } 
   if (Mensaje.length()>0){ Serial.println(Mensaje); //" Seleccione"
   
     if (Mensaje == "1") {
      segundo(); segundooo(); segundoo(); segundoooo();  perdist(); }// 32 seg
     
     if (Mensaje == "2") { 
      segundo(); segundoo(); segundooo(); segundoooo();  perdist(); } // 32 seg
     
     if (Mensaje == "3") {
      segundoo(); segundoooo(); perdist(); } // 17 seg
     
     if (Mensaje == "4") { segP(); }   // 59 seg = 1 min
     
     if (Mensaje == "5") { segundo(); segundoo(); segP(); }   // 80 seg = 1 min y 20 seg
     
     if (Mensaje == "6") { opera03(); }   // 98 seg = 1 min y 38 seg
     
     if (Mensaje == "7") { opera0(); }   // 49 seg
     
     if (Mensaje == "8") { opera01(); }  // 24 seg
     
     if (Mensaje == "9") { opera02(); }   // 60 seg = 1 min
     
     if (Mensaje == "10") {
      segundo(); segundoo(); segundooo(); segundooo();  perdist();  }  // 
     
     if (Mensaje == "a") {
      for (int e=0; e<=20; e++){ digitalWrite(ledPin1, 1); delay(50);digitalWrite(ledPin1, 0); delay(50);}  }   // zumbador ganaste
     
     if (Mensaje == "b") {
      for (int er=0; er<=20; er++){ digitalWrite(ledPin1, 1); delay(100);digitalWrite(ledPin1, 0); delay(100);}  }   // zumbador ganaste
     
     if (Mensaje == "c") {
      for (int ea=0; ea<=15; ea++){ digitalWrite(ledPin1, 1); delay(150);digitalWrite(ledPin1, 0); delay(150);}  }   // zumbador ganaste
     
     if (Mensaje == "e") {
      for (int eb=0; eb<=10; eb++){ digitalWrite(ledPin1, 1); delay(200);digitalWrite(ledPin1, 0); delay(200);}  }   // zumbador ganaste
     
     if (Mensaje == "f") { ganee(); }    // tono ganador
     
     if (Mensaje == "g") { perdist(); }  // tono perdiste

     if (Mensaje == "h") { gane(); }    // tono ganador
     
     if (Mensaje == "i") { perdiste(); }  // tono perdiste

     if (Mensaje == "j") { ganeee(); }    // tono ganador
     
     if (Mensaje == "k") { perdistee(); }  // tono perdiste
     
     if (Mensaje == "l") {
      digitalWrite(ledPin1, 1); delay(1000);digitalWrite(ledPin1, 0); }  // zumbador perdiste
     
     if (Mensaje == "m") {
      for (int oo=0; oo<=10; oo++){ digitalWrite(ledPin1, 1); delay(250);digitalWrite(ledPin1, 0); delay(250);}  }   // zumbador ganaste
     
     if (Mensaje == "n") { delay(250); resetFunc(); }  // reset 
      Mensaje=""; }
     } 
//________________________________________________________________________________________________________________________________________________________
void setup() { 
  Serial_2.begin(9600); // Iniciamos el puerto nuevo Serial_2 a 9600 Baudios
  Serial.begin(9600);   // Iniciamos el puerto que esta conectado al PC a 9600 Baudios  
  digitalWrite(ledPin, 1);  //Serial.println(" Hola bienvenido al "); Serial.print(" PulsoGame BT "); Mensaje = "0";
  
  pinMode(piezowinlos, OUTPUT); //  pinMode(led, OUTPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(ledPin1, OUTPUT); 
//    pinMode(inic, OUTPUT);
  
  pinMode(lineajuego, INPUT_PULLUP);
  pinMode(lineameta, INPUT_PULLUP);
  pinMode(inic, INPUT_PULLUP);
  pinMode(timer, INPUT_PULLUP);
  pinMode(timer1, INPUT_PULLUP);
  pinMode(timer2, INPUT_PULLUP);
  pinMode(timer3, INPUT_PULLUP);

  digitalWrite(lineajuego, 1);
  digitalWrite(lineameta, 1);
  digitalWrite(inic, 1);
  digitalWrite(ledPin1, 0); digitalWrite(ledPin, 0); delay(250); digitalWrite(ledPin, 1);
}
//_____________________________
void loop() { // digitalWrite(inic, 1);

    if (digitalRead(inic) == 0) { 

   if (digitalRead(timer) == 0) { 
          digitalWrite(ledPin, 0); delay(250);  digitalWrite(ledPin, 1); digitalWrite(ledPin, 0);
          segundo(); segundoo(); segundooo(); segundoo(); segundoooo(); perdist(); } // 44seg
   
  if (digitalRead(timer1) == 0) {
          digitalWrite(ledPin, 0); delay(250);  digitalWrite(ledPin, 1);
          segP(); }   //59 seg = 1 min

  if (digitalRead(timer2) == 0) {
         digitalWrite(ledPin, 0); delay(250);  digitalWrite(ledPin, 1);
          opera02(); }

  if (digitalRead(timer3) == 0) {
         digitalWrite(ledPin, 0); delay(250);  digitalWrite(ledPin, 1);
         opera01(); } // 25 seg

        preguntas();  }
}
