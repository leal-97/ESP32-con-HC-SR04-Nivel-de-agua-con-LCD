# ESP32-con-HC-SR04-Nivel-de-agua-con-LCD

## Práctica de una ESP32 con un sensor ultrasonico HC-SR04 y un LCD de 12C

En este repositorio muestraré como programar una ESP32 con el sensor HC-SR04 y mostrar los datos optenidos en un LCD 12c.

## ESP32
Es un microcontrolador revolucionario que se ha vuelto esencial en la tecnología moderna. Es un sistema en chip (SoC) de bajo costo y bajo consumo de energía con una combinación de capacidades Wi-Fi y Bluetooth. 

1.	**30 Pines de E/S:** Ofrece una amplia variedad de pines para la conexión de sensores, actuadores y otros dispositivos.
2.	**Alimentación:** Puede ser alimentado a través de un puerto MicroUSB o mediante otros métodos de suministro de energía.
3.	**Wi-Fi y Bluetooth Integrados:** Facilita la conectividad inalámbrica para la comunicación y el control remoto.
4.	**Capacidades de Programación:** Se puede programar utilizando el entorno de desarrollo de Arduino o herramientas específicas de Espressif.
Aplicaciones Comunes:
1.	**Proyectos de IoT (Internet de las Cosas):** Ideal para dispositivos conectados a la red que requieren comunicación inalámbrica.
2.	**Sistemas Embebidos:** Puede ser utilizado en sistemas embebidos para controlar y monitorear dispositivos.
3.	**Desarrollo de Prototipos:** Ampliamente utilizado en el desarrollo de prototipos debido a su facilidad de programación y su capacidad de conexión a una variedad de dispositivos.

## Sensor DHT22
El DHT22 es un sensor digital de humedad y temperatura que ofrece mediciones precisas y de alta calidad. Está compuesto por un pequeño circuito integrado y un sensor capacitivo de humedad y temperatura. 
Es ampliamente utilizado en aplicaciones que requieren un monitoreo preciso del entorno.
Características Principales:
-	**Medición de Humedad y Temperatura:** Proporciona mediciones digitales de humedad relativa y temperatura ambiente.
- **Bajo Consumo de Energía:** Opera con un bajo consumo de energía, lo que lo hace adecuado para aplicaciones con restricciones de energía.
- **Tiempo de Respuesta Rápido:** Proporciona mediciones actualizadas en intervalos cortos de tiempo.

 Aplicaciones Comunes:

- Sistemas de monitoreo de clima interior y exterior.
- Proyectos de automatización del hogar.
- Dispositivos de control ambiental y climatización.

## HC-SR04: Sensor Ultrasónico de Distancia
El HC-SR04 es un sensor ultrasónico de distancia ampliamente utilizado para medir distancias sin contacto de manera precisa y eficiente. Este dispositivo utiliza ondas ultrasónicas para calcular la distancia entre el sensor y un objeto, proporcionando así una solución efectiva para proyectos de detección de proximidad.
**Características Principales:**
-	**Principio de Funcionamiento:** Emite pulsos ultrasónicos y mide el tiempo que tardan en regresar después de rebotar en un objeto.
-	**Rango de Medición:** Generalmente, tiene un rango de medición de 2 cm a 4 metros.
-	**Bajo Costo:** Es una opción asequible para aplicaciones de medición de distancia.

**Cómo Funciona:**

-	**Trigger (Disparador):** Se envía un pulso de 10 µs para activar el sensor.
- **Transmisión de Pulsos Ultrasónicos:** El sensor emite una serie de pulsos ultrasónicos.
-	**Recepción de Eco:** El sensor espera el eco reflejado desde el objeto.
-	**Cálculo de Distancia:** La distancia se calcula midiendo el tiempo que tarda en regresar el eco.


## Material
- https://wokwi.com/
- ESP32
- DHT22
- LCD 16x2 ILC
- Sensor HC-SR04
- Módulo de Relé  

## Instrucciones:
- **1.- Entrar al siguiente enlace:** https://wokwi.com/
- **2-. Seleccionar la tarjeta ESP32**

![]( https://github.com/leal-97/ESP32-con-sensor-DHT22/blob/main/esp32kd.jpeg )
  
- **3.- Bajar el cursor hasta starter templates y seleccionar ESP32 una vez más**

![]( https://github.com/leal-97/ESP32-con-sensor-DHT22/blob/main/starter.jpeg )


- **4.- En el cuadro de programación borrar el código default y pegar este:**

```
// defines pins numbers
const int trigPin = 4;
const int echoPin = 15;
const int led1 = 2;
const int led2 = 5;
const int led3 = 18;
const int led4 = 17;

#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

// defines variables
long duration;
int distance;
int distancia;
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
// Clears the trigPin
digitalWrite(trigPin, LOW);
delayMicroseconds(2);

// Sets the trigPin on HIGH state for 10 micro seconds
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

// Reads the echoPin, returns the sound wave travel time in microseconds
duration = pulseIn(echoPin, HIGH);

// Calculo de la distancia
//distance= duration*0.034/2;
distancia= int(0.01716*duration);

safetyDistance = distancia;
if (safetyDistance>=2 && safetyDistance<=5) //90% TANQUE
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);

lcd.clear();                //Funcion para limpiar pantalla
  
  lcd.setCursor(2, 0);       // Coordenadas para centrar texto
  lcd.print("Diplomado v");
  lcd.setCursor(2, 1);
  lcd.print("Mecatronica");
  delay(2000);              //Retardo en milisegundos

  lcd.clear();             //Funcion para limpiar pantalla

  lcd.setCursor(2, 0);    // Coordenadas para centrar texto
  lcd.print("Eduardo Leal");
  lcd.setCursor(2, 1);
  lcd.print("Ing. Mecanico");
  delay(2000);            //Retardo en milisegundos

  lcd.clear();          //Funcion para limpiar pantalla

  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 90%");
  delay(2000);
}
else if(safetyDistance>=5 && safetyDistance<=10) //75% TANQUE
{
  digitalWrite(led1, HIGH);
  digitalWrite(led2, HIGH);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);

lcd.clear();                //Funcion para limpiar pantalla
  
  lcd.setCursor(2, 0);       // Coordenadas para centrar texto
  lcd.print("Diplomado v");
  lcd.setCursor(2, 1);
  lcd.print("Mecatronica");
  delay(2000);              //Retardo en milisegundos

  lcd.clear();             //Funcion para limpiar pantalla

  lcd.setCursor(2, 0);    // Coordenadas para centrar texto
  lcd.print("Eduardo Leal");
  lcd.setCursor(2, 1);
  lcd.print("Ing. Mecanico");
  delay(2000);            //Retardo en milisegundos

  lcd.clear();          //Funcion para limpiar pantalla

  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 75%");
  delay(2000);
}
else if(safetyDistance>=10 && safetyDistance<=45) //50% TANQUE
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, LOW);
lcd.clear();                //Funcion para limpiar pantalla
  
  lcd.setCursor(2, 0);       // Coordenadas para centrar texto
  lcd.print("Diplomado v");
  lcd.setCursor(2, 1);
  lcd.print("Mecatronica");
  delay(2000);              //Retardo en milisegundos

  lcd.clear();             //Funcion para limpiar pantalla

  lcd.setCursor(2, 0);    // Coordenadas para centrar texto
  lcd.print("Eduardo Leal");
  lcd.setCursor(2, 1);
  lcd.print("Ing. Mecanico");
  delay(2000);            //Retardo en milisegundos

  lcd.clear();          //Funcion para limpiar pantalla

  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 50%");
  delay(2000);
}
else if(safetyDistance>=45 && safetyDistance<=70) //35% TANQUE
{
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, HIGH);
  digitalWrite(led4, HIGH);
lcd.clear();                //Funcion para limpiar pantalla
  
  lcd.setCursor(2, 0);       // Coordenadas para centrar texto
  lcd.print("Diplomado v");
  lcd.setCursor(2, 1);
  lcd.print("Mecatronica");
  delay(2000);              //Retardo en milisegundos

  lcd.clear();             //Funcion para limpiar pantalla

  lcd.setCursor(2, 0);    // Coordenadas para centrar texto
  lcd.print("Eduardo Leal");
  lcd.setCursor(2, 1);
  lcd.print("Ing. Mecanico");
  delay(2000);            //Retardo en milisegundos

  lcd.clear();          //Funcion para limpiar pantalla
  lcd.setCursor(0, 0);
  lcd.print(" Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 35%");
  delay(2000);
}
else  //5% TANQUE O MENOS
{
 digitalWrite(led1,  LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
lcd.clear();                //Funcion para limpiar pantalla
  
  lcd.setCursor(2, 0);       // Coordenadas para centrar texto
  lcd.print("Diplomado v");
  lcd.setCursor(2, 1);
  lcd.print("Mecatronica");
  delay(2000);              //Retardo en milisegundos

  lcd.clear();             //Funcion para limpiar pantalla

  lcd.setCursor(2, 0);    // Coordenadas para centrar texto
  lcd.print("Eduardo Leal");
  lcd.setCursor(2, 1);
  lcd.print("Ing. Mecanico");
  delay(2000);            //Retardo en milisegundos

  lcd.clear();          //Funcion para limpiar pantalla

  lcd.setCursor(0, 0);
  lcd.print("Distancia: " + String(distancia)+"cm");
  lcd.setCursor(2, 1);
  lcd.print("Tanque: 5%");
  delay(2000);
}

// Prints the distance on the Serial Monitor
Serial.print("Distancia: "  );
Serial.println(distancia);
delay (2000); 
}
```

- **5.- En la sección de library manager buscar las siguientes librerías y agregarlas:**
- LiquidCristal IC2
- DHT sensor library for ESPx

- 6.- Seleccionar el sensor en la parte de Simulacion con el botón **+** y buscar **LCDI2C**, **HC-SR04**, **Relay** Agregar y conectar de la siguiente manera.

![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/conexion%20nivel%20de%20agua.jpeg )

## Instrucción de operación:
- Correr el simulador
- Manipular los valores del sensor para observar los resultados de la medición arrojados en la pantalla LCD
- visualizar la secuencia de leds dependiendo del nivel del tanque de agua
- Colocar la distancia dando click al sensor ultrasonico HC-SR04

## Resultados
Cuando haya compilado el código y corra la simulación, verás los valores y los textos en la pantalla LCD como se muestran en las siguentes imagenes cada 2s.

![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/dip.jpeg )

![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/leal.jpeg )


-Tanque al 90% de nivel de agua.

![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/90%25.jpeg )


-led 1 encendido, led 2,3,4 apagados.


-Tanque al 75% de nivel de agua.

![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/75.jpeg )


-led 1 y 2 encendidos, led 3,4 apagados.

-Tanque al 50% de nivel de agua.


![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/50.jpeg )


-led 3 encendido, led 1,2,4 apagados.


-Tanque al 35% de nivel de agua.

![]( https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/35.jpeg )


-led 3 y 4 encendidos, led 1,2 apagados.


-Tanque al 5% de nivel de agua.


![](  https://github.com/leal-97/ESP32-con-HC-SR04-Nivel-de-agua-con-LCD/blob/main/5.jpeg )


-led 1,2,3,4 apagados.


## Referencias

- Sensor ultrasónico HC-SR04. (s.f.). Transtronix. https://transtronix.com.mx/productos/sensor-ultrasonico-hc-sr04/?srsltid=AfmBOoo2PRtZH3TYMrjSWNY0K-zpGZCoLoRpQKGxj_-p06fbH3TMoraF
- ESP32 30 Pines. (s.f.). Transtronix. https://transtronix.com.mx/productos/esp32-30-pines/?srsltid=AfmBOoo9Q1Vc1XVCLC0ZsFSZCIyZ97hdDYoH6Qf2gwDvs2M-5pUDvXv5
- Sensor de humedad y temperatura DHT22. (s.f.). Transtronix. https://transtronix.com.mx/productos/sensor-de-humedad-y-temperatura-dht22/?srsltid=AfmBOoo2GcDGu0hr44E75_ZvkYeEb_3f_b8i974KTgGTsq6kfgWZMjGH

## Créditos
Desarrollado por: **Ing. Mecánico Eduardo Leal**

- https://github.com/leal-97

