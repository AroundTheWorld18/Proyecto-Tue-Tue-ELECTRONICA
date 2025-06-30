# Proyecto-Tue-Tue-ELECTRONICA
Titulo: "Tue Tue en la oscuridad"
Documentación de proyecto final/examen de Tecnología Aplicada I: Electrónica.
Premisa del proyecto: "Exploración de la leyenda Chilena del Tue Tue desde el misterio".

## Descripcion conceptual del proyecto:
Tenía la idea de hacer mi trabajo sobre un ser mitológico y/o monstruoso, ya que me gustan mucho. Por éste motivo, escogí realizarlo sobre una Tue Tue o Chonchona, ya que pienso que es mejor enfocarse en criaturas Chilenas. Esta leyenda es sobre brujos que pueden separar su cabeza de su cuerpo mediante magia, tranformando sus orejas en alas para salir volando exclusivamente de noche. Esta leyenda es bastante famosa, sin embargo prácticamente siempre se describe al Tue Tue como un ser masculino, por lo cual tratar de enfocarme en una criatura femenina, ya que la mitología cuenta que puede ser tanto un brujo como una bruja. Tambien se me hace mucho más interesante.

## Descripción técnica del proyecto:
En este aspecto, elegí dos actuadores servomotor para hacer funcionar el movimiento de las alas, y para el sensor que activaría el funcionamiento de las alas escogi un sensor ultrasónico (aspecto descartado).
El Arduino UNO iría adherido con pegamento a la superficie (posiblemente data) donde coloque ambas alas de la criatura. Detras de este circuito irian uno o  más focos para lograr la proyección de la sombra de estas mismas alas. La sala estaria oscura y con las luces del techo apagadas.

~~~Código FALLIDO (Pre exámen):
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
}~~~

Codigo DEFINITIVO:
~~~#include <Servo.h>

// Crea dos objetos Servo
Servo servo1;
Servo servo2;

// Establece los pines donde se conectan los servos
const int servo1Pin = 9;
const int servo2Pin = 10;

// Variables para rastrear las posiciones de los servos
int pos1 = 0; // Posición inicial del servo1
int pos2 = 180; // Posición inicial del servo2
bool forward = true;

void setup() {
servo1.attach(servo1Pin);
servo2.attach(servo2Pin);
servo1.write(pos1);
servo2.write(pos2);
lay(1000); // Esperar 1 segundo al inicio
}
void loop() {
if (forward) {
// Mover servo1 de 0 a 110, servo2 de 180 a 70 (dirección opuesta)
for (int i = 0; i <= 110; i++) {
servo1.write(i);
servo2.write(180 - i);
lay(1000 / 110); // Extender el movimiento a lo largo de 1 segundo
}
pos1 = 110;
pos2 = 70;
forward = false;
} else {
// Mover servo1 de 110 a 0, servo2 de 70 a 180 (dirección opuesta)
for (int i = 110; i >= 0; i--) {
servo1.write(i);
servo2.write(180 - i);
delay(1000 / 110); // Distribuye el movimiento a lo largo de 1 segundo
}
pos1 = 0;
pos2 = 180;
forward = true;
}
lay(500); // Pausa opcional entre movimientos
}~~~
