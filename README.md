# Proyecto-Tue-Tue-ELECTRÓNICA
Titulo: "Tue Tue en la oscuridad".
Documentación de proyecto final/examen de Tecnología Aplicada I: Electrónica.

Premisa del proyecto: "Exploración de la leyenda Chilena del Tue Tue desde el misterio".

## Descripcion conceptual del proyecto:
Tenía la idea de hacer mi trabajo sobre un ser mitológico y/o monstruoso, ya que me gustan mucho. Por éste motivo, escogí realizarlo sobre una Tue Tue o Chonchona, que es una leyenda de origen Mapuche, pues pienso que es mejor en esta ocasión enfocarse en criaturas Chilenas. Esta leyenda es sobre brujos que pueden separar su cabeza de su cuerpo mediante un ungüento mágico que se aplican en el cuello, tranformando sus orejas en alas para salir volando exclusivamente de noche. Esta leyenda es bastante famosa, sin embargo prácticamente siempre se describe al Tue Tue como un ser masculino, por lo cual trataré de enfocarme en una criatura femenina, ya que la mitología cuenta que puede ser tanto un brujo como una bruja. Tambien se me hace mucho más interesante.

## Descripción técnica del proyecto:
En este aspecto, elegí dos actuadores servomotor para hacer funcionar el movimiento de las alas, y para el sensor que activaría el funcionamiento de las alas escogi un sensor ultrasónico (aspecto descartado!!!).
El Arduino UNO iría adherido con pegamento a la superficie (posiblemente al data) donde colocare ambas alas de la criatura. Detrás de este circuito irían 1 o 2 focos para lograr la proyección de la sombra de las mismas alas. La sala estaria oscura y con las luces del techo apagadas.

![ilustarción circuito(./circuito tinkercard.png)]

~~~Codigo DEFINITIVO:

#include <Servo.h>

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
