# Proyecto-Tue-Tue-ELECTRONICA
Documentación de proyecto final/examen de Tecnología Aplicada I: Electrónica.
Premisa del proyecto: "Exploración de la leyenda Chilena del Tue Tue desde el ámbito femenino".

## Descripcion conceptual del proyecto:
Tenía la idea de hacer mi trabajo sobre un ser mitológico y/o monstruoso, ya que me gustan mucho. Por éste motivo, escogí realizarlo sobre una Tue Tue o Chonchona, ya que pienso que es mejor enfocarse en criaturas Chilenas. Esta leyenda es sobre brujos que pueden separar su cabeza de su cuerpo mediante magia, tranformando sus orejas en alas para salir volando exclusivamente de noche. Esta leyenda es bastante famosa, sin embargo prácticamente siempre se describe al Tue Tue como un ser masculino, por lo cual quize enfocarme específicamente en una criatura femenina ya que la mitología cuenta que puede ser tanto un brujo como una bruja. Tambien se me hace mas interesante.

## Descripción técnica del proyecto:
En este aspecto, elegí dos actuadores servomotor para hacer funcionar el movimiento de las alas, y para el sensor que activaría el funcionamiento de las alas escogi un sensor ultrasónico, ya que me gustaba/acomodaba más como este funcionaba.

## Código:
#include <Servo.h>

const int trigPin = 2;     // Pin disparador del sensor ultrasónico
const int echoPin = 3;     // Pin de eco del sensor ultrasónico
const int servoPin1 = 9;   // Pin de control del servomotor 1
const int servoPin2 = 10;  // Pin de control del servomotor 2

Servo servo1; // Objeto servomotor 1
Servo servo2; // Objeto servomotor 2

void setup() {
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
servo1.attach(servoPin1); // Adjunte el servo 1 a su pin de control
servo2.attach(servoPin2); // Adjunte el servo 2 a su pin de control
}

void loop() {
long duration, distance;

// Enviar pulso ultrasónico
digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

// Medir la duración del eco
duration = pulseIn(echoPin, HIGH);

// Calcular la distancia en centímetros
distance = duration * 0.034 / 2;

// Limitar la distancia a un rango adecuado para los servos
if (distance < 5) {
distance = 5;
} else if (distance > 40) {
distance = 40;
}

// Asignar el valor de distancia al rango de ángulo del servo
int angleRange = 60; // Rango de ángulo deseado
int angleOffset = (180 - angleRange) / 2; // Calcular el desplazamiento

int angle1 = map(distance, 5, 40, angleOffset, angleOffset + angleRange);
int angle2 = map(distance, 5, 40, angleOffset + angleRange, angleOffset);

// Mover cada servo al ángulo deseado
servo1.write(angle1);
servo2.write(angle2);

delay(2000); // Retraso de 2 segundos antes de la próxima medición
}
