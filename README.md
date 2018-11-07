#include <SoftwareSerial.h>
SoftwareSerial serial1(10, 11); // RX, TX
 
// Motor 1: Direita
// Montar no ralay 3 e 4 do shield para motor
const int relay3 = 7; //porta D7 Arduino
const int relay4 = 6; //porta D6 Arduino
 
//Motor 2: Esquerda
// Montar no Relay 1 e 2 do shiel para motor
const int relay1 = 8; //porta D8 Arduino
const int relay2 = 9; //porta D9 Arduino
 
// Shield para Bomba 1 (relay 3) e Bomba 2 (relay 4)
const int bomb1 = 5; // porta D5 Arduino
const int bomb2 = 4; // porta D4 Arduino
 
int B1Status = 0;
int B2Status = 0;
 
void setup() {
  // Inicializar relay e serial monitor
  Serial.begin(9600);
  serial1.begin(9600);
  pinMode(relay1, OUTPUT);
  pinMode(relay2, OUTPUT);
  pinMode(relay3, OUTPUT);
  pinMode(relay4, OUTPUT);
  pinMode(bomb1, OUTPUT);
  pinMode(bomb2, OUTPUT);
 
 
  desliga();
  digitalWrite(bomb1, LOW);
  digitalWrite(bomb2, LOW);
 
}
 
void loop() {
  /*frente();
  delay(2000);
  desliga();
  delay(2000);
  tras();
  delay(2000);
  desliga();
  delay(2000);*/
  if(serial1.available()){
    byte byteRecebido = serial1.read();
    //int acao = bitRead(byteRecebido,3);
    //Serial.println(byteRecebido);
    /*while(byteRecebido == 65){
       frente();
   
    }*/
   
    if(byteRecebido == 51){
      desliga();
      digitalWrite(bomb1, LOW);
      digitalWrite(bomb2, LOW);	
    }
    //Serial.println(byteRecebido);
    if(byteRecebido == 65){
     Serial.println("Frente");
      frente();
      //delay(2000);
      //desliga();
    }else if (byteRecebido == 66){
     Serial.println("tras");
      tras();
      //delay(2000);
      //desliga();
    } else if (byteRecebido == 68){
      Serial.println("Direita");
      direita();
    } else if(byteRecebido == 67){
      Serial.println("Esquerda");
      esquerda();
    }else if (byteRecebido == 90){
      //Serial.println("Parado");
      desliga();
    }
 
    if(byteRecebido == 49){
      if(B1Status == 0){
        B1Status = 1;
        Serial.println("Bomba 1 Ligada");
        digitalWrite(bomb1,HIGH);
      } else if (B1Status == 1){
        B1Status = 0;
        Serial.println("Bomba 1 Desligada");
        digitalWrite(bomb1,LOW);
       
      }
    }
    if(byteRecebido == 50){
      if(B2Status == 0){
        B2Status = 1;
        Serial.println("Bomba 2 Ligada");
        digitalWrite(bomb2,HIGH);
      } else if (B2Status == 1){
        B2Status = 0;
        Serial.println("Bomba 2 Desligada");
        digitalWrite(bomb2,LOW);
       
      }
    }
 
   
  
  }
 
 
 
}
 
void tras(){
  digitalWrite(relay1, HIGH);
  digitalWrite(relay2, LOW);
  digitalWrite(relay4, LOW);
  digitalWrite(relay3, HIGH);
  
}
 
void frente(){
  digitalWrite(relay1, LOW);
  digitalWrite(relay2, HIGH);
  digitalWrite(relay3, LOW);
  digitalWrite(relay4, HIGH);
}
void desliga(){
  digitalWrite(relay1, LOW);
  digitalWrite(relay2, LOW);
  digitalWrite(relay3, LOW);
  digitalWrite(relay4, LOW);
}
 
void direita(){
  digitalWrite(relay1, LOW);
  digitalWrite(relay2, HIGH);
  digitalWrite(relay3, HIGH);
  digitalWrite(relay4, LOW);
 
}
void esquerda(){
  digitalWrite(relay1, HIGH);
  digitalWrite(relay2, LOW);
  digitalWrite(relay3, LOW);
  digitalWrite(relay4, HIGH);
}
