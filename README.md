# car

Magnús og Stígur

verkefni 1 grunnkröfur
https://youtube.com/shorts/N-P8ADtX34g?feature=share


verkefni 2, betri árekstarvörn
https://youtube.com/shorts/n2WanGPyzAY?feature=share


verkefni 3, stýripinni
https://youtube.com/shorts/UvLGh_-ZDfk?feature=share


Það vill svo óheppilega til að hálfvitinn hann Stígur gleymdi að taka upp þegar við vorum að prufukeyra stefnuljósin og lögguljósin, en það sést að í þau virka þegar við vorum að prufukeyra með stýripinnanum


Kóði:
// Sonic skynjari
#include "tdelay.h"

int rautt = 2; // pinninn sem LED peran er tengd í
int blatt = A5; // pinninn sem LED peran er tengd í
bool LED_on = true; // breytan heldur utan um hvort á að vera kveikt eða slökkt á LED perunni
TDelay LED_bidtimi(250);

const int x = A1;
const int y = A3;
const int takki = 12;

int x_hreyfing = 0;
int y_hreyfing = 0;
int takki_stada = 0;

int echoPin = 9;
int trigPin = 10;

// Mótor A
int pwmA = 3;
int Ai1 = 5;
int Ai2 = 4;

// Mótor B
int pwmB = 6;
int Bi1 = 7;
int Bi2 = 8;

int hae = A0;
int vin = A2;


void setup() {
  pinMode(x, INPUT);
  pinMode(y, INPUT);
  pinMode(takki, INPUT_PULLUP); 

  pinMode(rautt, OUTPUT);
  pinMode(blatt, OUTPUT);
  // Sonic pinnar
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  // Mótor pinnar
  pinMode(pwmA, OUTPUT);
  pinMode(pwmB, OUTPUT);
  pinMode(Ai1, OUTPUT);
  pinMode(Ai2, OUTPUT);
  pinMode(Bi1, OUTPUT);
  pinMode(Bi2, OUTPUT);

  pinMode(A0, OUTPUT);
  pinMode(A2, OUTPUT);

  Serial.begin(9600);

}

void loop() {
 x_hreyfing = analogRead(x);
y_hreyfing = analogRead(y);
  takki_stada = digitalRead(takki);
  
  x_hreyfing = map(x_hreyfing, 0, 1023, -255, 255);
  y_hreyfing = map(y_hreyfing, 0, 1023, -127, 127);
  Serial.print("X: ");
  Serial.print(x_hreyfing);
  Serial.print(" | Y: ");
  Serial.print(y_hreyfing);
  Serial.print(" | Takki: ");
  Serial.println(takki_stada);

  if (x_hreyfing > 10) { 
    bakka(x_hreyfing);
  }
  if (x_hreyfing < -10) {
    afram(-x_hreyfing);
  }

if (x_hreyfing > -10 && x_hreyfing < 10 && y_hreyfing > -10 && y_hreyfing < 10) {
  stoppa(1);
}

  if (y_hreyfing > 10) {
    haegri(y_hreyfing);
  }  
  if (y_hreyfing < -10) {
    vinstri(-y_hreyfing);
  }

  if (takki_stada == 1) {
    stoppa(1);    
  }
  //  int maela = maelaFjarlaegd();
  // Serial.println(maela);
  // if (maela < 50) {
  //   stoppa(1);
  //   delay(1000);
  //   vinstri(90);
  //   delay(700);
  //   stoppa(1);
  //   delay(1000);
  //   int v = maelaFjarlaegd();
  //   stoppa(1);
  //   delay(1000);
  //   haegri(90);
  //   delay(1400);
  //   stoppa(1);
  //   delay(1000);
  //   int h = maelaFjarlaegd(); 
  //   if (h < 50 && v < 50) {      
  //   stoppa(1);
  //   delay(1000);
  //   haegri(90);
  //   delay(700);
  //   stoppa(1);
  //   delay(1000);    
  //   }
  //   if (v > h && v > 50) {
  //     stoppa(1);
  //     delay(1000);
  //     vinstri(90);
  //     delay(1400);
  //     stoppa(1);
  //     delay(1000);
  //   }
  //   if (h > v && h > 50) {
  //   stoppa (1);
  //   delay(1000);
  //   }
  //   afram(245);
  //   delay(5);
  //   }
  //   afram(245);




  //   int rtala = random(0, 2);
  //   Serial.println(rtala);
  //   if (rtala == 0) {
  //     stoppa(1);
  //     delay(1000);
  //     vinstri(100);
  //     delay(700);
  //     stoppa(1);
  //     delay(1000);
  //   }
  //   if (rtala == 1) {
  //     stoppa(1);
  //     delay(1000);
  //     haegri(100);
  //     delay(700);
  //     stoppa(1);
  //     delay(1000);
  //   }
  // }
  //   afram(245);
  //   delay(5);   
  

  //stoppa(1);
  //delay(5000);
  //afram(255);
  //delay(1000);
  //afram(127);
  //delay(1000);
  //stoppa(1);
  //delay(1000);
  //haegri(100);
  //delay(700);
  //stoppa(1);
  //delay(1000);
  //bakka(127);
  //delay(1000);
  //stoppa(1);
  //delay(1000);
  // vinstri(100);
  // delay(700);
  // stoppa(1);
  // delay(1000);
  // haegri(100);
  // delay(1400);
  // stoppa(1);
  // delay(1000);
}



void afram(int hradi) {
  digitalWrite(vin, LOW);
  digitalWrite(hae, LOW);
  // Set Motor A forward
  digitalWrite(Ai1, HIGH);
  digitalWrite(Ai2, LOW);

  // Set Motor B forward
  digitalWrite(Bi1, HIGH);
  digitalWrite(Bi2, LOW);

  // Set the motor speeds
  analogWrite(pwmA, hradi);
  analogWrite(pwmB, hradi);

  if(LED_bidtimi.timiLidinn() == true) { 
    // víxla (e. toggle) stöðunni á breytunni sem heldur utan um hvort kveikt eða slökkt er á LED perunni
    LED_on = !LED_on; 
  }
   // kveikir eða slekkur á LED perunni eftir stöðu LED_on breytunnar
  digitalWrite(rautt, LED_on);
  digitalWrite(blatt, !LED_on);
}

void bakka(int hradi) {
  digitalWrite(vin, LOW);
  digitalWrite(hae, LOW);

  digitalWrite(Ai1, LOW);
  digitalWrite(Ai2, HIGH);

  digitalWrite(Bi1, LOW);
  digitalWrite(Bi2, HIGH);

  analogWrite(pwmA, hradi);
  analogWrite(pwmB, hradi);

  digitalWrite(rautt, LOW);
  digitalWrite(blatt, LOW);
}

void vinstri(int hradi) {
  digitalWrite(vin, HIGH);
  digitalWrite(hae, LOW);

  digitalWrite(Ai1, HIGH);
  digitalWrite(Ai2, LOW);

  digitalWrite(Bi1, LOW);
  digitalWrite(Bi2, HIGH);

  analogWrite(pwmA, hradi);
  analogWrite(pwmB, hradi);
  
  digitalWrite(rautt, LOW);
  digitalWrite(blatt, LOW);
}

void haegri(int hradi) {
  digitalWrite(hae, HIGH);
  digitalWrite(vin, LOW);

  digitalWrite(Ai1, LOW);
  digitalWrite(Ai2, HIGH);

  digitalWrite(Bi1, HIGH);
  digitalWrite(Bi2, LOW);

  analogWrite(pwmA, hradi);
  analogWrite(pwmB, hradi);

  digitalWrite(rautt, LOW);
  digitalWrite(blatt, LOW);
}

void stoppa(int hradi) {
  digitalWrite(vin, LOW);
  digitalWrite(hae, LOW);

  digitalWrite(Ai1, LOW);
  digitalWrite(Ai2, LOW);

  digitalWrite(Bi1, LOW);
  digitalWrite(Bi2, LOW);

  analogWrite(pwmA, hradi);
  analogWrite(pwmB, hradi);

  digitalWrite(rautt, LOW);
  digitalWrite(blatt, LOW);
}

int maelaFjarlaegd() {
  // sendir út púlsar
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // mælt hvað púlsarnir voru lengi að berast til baka
  return pulseIn(echoPin, HIGH) / 58.0;  // deilt með 58 til að breyta í cm
}
