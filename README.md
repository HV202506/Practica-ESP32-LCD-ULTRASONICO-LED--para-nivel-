# Practica-ESP32-LCD-ULTRASONICO-LED (para-nivel)

En este repositorio se mostrara como programar un ESP32 con un sensor ULTRASONICO, con el cual obtendremos las la magnitud de la distancia de un tanque para aproximar el porcentaje de llenado que tiene y mostrar el intervalo de nivel en el que se encuentra a traves de una pantalla LCD. A su vez se colcaran 4 leds para indicar en que intervalo de llenado se encuentra el tanque.

## Introducción 

La placa ESP32 la empleamos para adquirir los datos del sensor ultrasonico y asi aproximar el intervalo porcentual de llenado en el que se encuentra el tanque. Ademas, se desean colocar 4 leds que nos indiquen el nivel de llenado del tanque para corroborar que el programa funciona adecuadamente.

La pantalla LCD se empleara para mostrar los datos solicitados:
- Nombre del diplomado
- Nombre
- Carrera profesional
- Fecha
- Distancia (determinada por el sensor ultrasonico)


Esta practica se realizo empleando un simulador: [WOKWI](www.wokwi.com) la cual nos permite simular los elementos y el codigo.

# Material necesario

Para realizar esta practica se necesito
- [WOKWI](www.wokwi.com)
- Una tarjeta ESP32 (para adquisicion de datos)
- Sensor ultrasonico "HC-SR04 Ultrasonic Distance Sensor"
- LCD 16x2 (I2C)
- 4 Leds
- 4 Resistencias de 200 ohms

# Instrucciones
1. Abrir la terminal del codigo:

Codigo empleado:

```
// defines pins numbers
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int trigPin = 4;
const int echoPin = 15;
const int led1 = 19;
const int led2 = 18;
const int led3 = 5;
const int led4 = 17;

// defines variables
long duration;
int distance;
int safetyDistance;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
pinMode(echoPin, INPUT); // Sets the echoPin as an Input
pinMode(led1, OUTPUT);
pinMode(led2, OUTPUT);
pinMode(led3, OUTPUT);
pinMode(led4, OUTPUT);
Serial.begin(9600); // Starts the serial communication
lcd.init();
lcd.backlight();


}


void loop() {
  long d; //valor del tanque
// Clears the trigPin
digitalWrite(trigPin, LOW);
delayMicroseconds(2);

// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(echoPin, HIGH);

// Calculating the distance
distance= duration*0.034/2;

safetyDistance = distance;
if (safetyDistance>=2 && safetyDistance<=25)
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
 

}
else if(safetyDistance>=26 && safetyDistance<=50) 
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
 

}
else if(safetyDistance>=51 && safetyDistance<=75) 
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, LOW);
}
else if(safetyDistance>=76 && safetyDistance<=100) 
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, HIGH);
}

// Prints the distance on the Serial Monitor
Serial.print("Distance: ");
Serial.println(distance);
delay (2000);

  //Mostrar Diplomado (fila 1); mostrar Automatizacion (fila 2)
  lcd.setCursor(0, 0);
  lcd.print("   Diplomado    ");
  lcd.setCursor(0, 1);
  lcd.print(" Automatizacion ");
  delay(2000);
  lcd.clear();

  //Mostrar nombre (fila 1); mostrar carrera (fila 2)
  lcd.setCursor(0, 0);
  lcd.print(" Hector Vega R. ");
  lcd.setCursor(0, 1);
  lcd.print(" Ing. Mecanica  ");
  
  delay(2000);
  lcd.clear();

  //Mostrar fecha (fila 1)
  lcd.setCursor(0, 0);
  lcd.print("07 de Junio 2025");
  lcd.setCursor(0, 1);
  lcd.print(" ");
  delay(2000);
  lcd.clear();

//Mostrar el nivel
if (safetyDistance>=2 && safetyDistance<=25)
{
  lcd.setCursor(0, 0);
  lcd.print("Nivel entre:");
  lcd.setCursor(0, 1);
  lcd.print("  0% y 25%  ");
  delay(2000);
  lcd.clear();
}
else if (safetyDistance>=26 && safetyDistance<=50)
{
  lcd.setCursor(0, 0);
  lcd.print("Nivel entre:");
  lcd.setCursor(0, 1);
  lcd.print("  25% y 50%  ");
  delay(2000);
  lcd.clear();

}

if (safetyDistance>=51 && safetyDistance<=75)
{
  lcd.setCursor(0, 0);
  lcd.print("Nivel entre:");
  lcd.setCursor(0, 1);
  lcd.print("  50% y 75%  ");
  delay(2000);
  lcd.clear();

}

if (safetyDistance>=76 && safetyDistance<=100)
{
  lcd.setCursor(0, 0);
  lcd.print("Nivel entre:");
  lcd.setCursor(0, 1);
  lcd.print("  75% y 100% ");
  delay(2000);
  lcd.clear();

}

}
```

2. Instalar la libreria de DHT sensor library for ESPx y en la siguiente imagen se muestran las librerias empleadas.

![](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/librerias.png?raw=true)

3. Hacer las concexiones del sensor ultrasonico HC-SR04 y la pantalla LCD con el ESP32  tal cual se muestra la siguiente imagen.

![](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/conexiones.png?raw=true)

## Librerias empleadas
1. DHT sensor library for ESPx
2. LiquidCrystal I2C

# Intrucciones de operación

1. Iniciar la simulación.
2. Observar los datos del monitor serial.
3. Colorar la distancia simulada haciendo doble click en el Sensor ultrasonico HC-SR04 (para suponer una distancia que reconoce el sensor)

4. Observar los datos mostrados en la pantalla LCD y los LEDS indicadores de nivel

# Resultados

Una vez realizados los pasos anteriores deberas visualizar los elementos solicitados en la pantalla LCD

## Pantalla 1:

![](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/diplomado.png?raw=true)

## Pantalla 2:

![](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/nombre.png?raw=true)

## Pantalla 3:

![](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/fecha.png?raw=true)
## Pantallas posibles segun el nivel de llenado del tanque

![image](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/25.png?raw=true)

![image](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/50.png?raw=true)

![image](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/75.png?raw=true)

![image](https://github.com/HV202506/Practica-ESP32-LCD-ULTRASONICO-LED--para-nivel-/blob/main/100.png?raw=true)

# Créditos

Desarrollado por: Ing. Héctor Angel Vega Rodríguez
