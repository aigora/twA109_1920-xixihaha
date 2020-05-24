# Título del trabajo

caja termometro

## Integrantes del equipo
Xiaolong Ruan(54978), Wei Zheng (54911）

## Objetivos del trabajo

Una caja que nos podra medir la temperatura, con la ayuda de lenguaje c, permitira que envie señales al arduino y que se brille la led que esta conectada a la caja y por ultimo el ordenador alerta varios tipos de sonido tras el alcance de la temperatura a un rango determinado.

## Sensor y actuadores

LED.   
Los diodos emisores de luz (LED) que usamos es un compusta de led y resistecias,son capaces de emitir dos colores diferentes de luz, generalmente rojo y verde.Tiene 3 pines,que son del RED,el GREEN y lo a tierra(GND).Modificamos el valor de voltaje de los dos pines para controlar el color de LED,Por lo tanto, este tipo de LED a menudo se utiliza como luz indicadora de varios dispositivos (cámaras de video, cámaras digitales y controles remotos).

Zumbador.   
Un zumbador (en inglés: buzzer) es un transductor electroacústico que produce un sonido o zumbido continuo o intermitente de un mismo tono (generalmente agudo). Sirve como mecanismo de señalización o aviso y se utiliza en múltiples sistemas, como en automóviles o en electrodomésticos, incluidos los despertadores.

Sensor de temperatura.    
Los sensores de temperatura es componentes electrónicos que convierten la temperatura en datos electrónicos. Un sensor de temperatura que utiliza un cuerpo conductor cuya resistencia cambia con la temperatura. El elemento más utilizado es el platino, que tiene una resistencia de 100 ohmios a 0 ° C. Los sensores de temperatura de semiconductores generalmente integran circuitos de amplificación y ajuste. La frecuencia de oscilación de un oscilador de cristal varía con la temperatura, por lo que puede medir la temperatura con mucha precisión.  
 
LCD

##subprograma de LED   


int green = 5; //fija pin 5 al color verde     
int red = 8; //fija pin 8 al color rojo   

void setup() {   
  pinMode(green,OUTPUT);   
  pinMode(red,OUTPUT);   
}

void loop() {

  //apagar el luz   
  digitalWrite(green, LOW);  
  digitalWrite(red, LOW);  
  delay(1000); 

   //LED con el color verde   
  digitalWrite(green, HIGH);  
  digitalWrite(red, LOW);  
  delay(1000); 

  //LED con el color rojo   
  digitalWrite(green, LOW);  
  digitalWrite(red, HIGH);  
  delay(1000);   
}


##Zumbador

const int buzzerPin=7;//el zumbador se conecta a 

int fre// definir el variable

void setup()

  {
  pinMode(buzzerPin,OUTPUT);//establecer buzzerpin como salida
  
  }
 
 void loop()
 
 {
 
    for(int i=200;i<=800;i++) //frecuencia pasa de 200 a 800
    
    {
    
      tone(7,i),
      
      delay(5); //espera 5 milisegundos
  }
  
  delay(4000); //espera 4 segundos
  
  for(int i=800;i>=200;i--) // frecuencia pasa de 800 a 200
  
    
    {
    
      tone(7,i),
      
      delay(10); //espera 10 milisegundos
  }
  
  ##Sensor de temperatura
  
  const int digitalPin=7; 
  
  int analogPin = A0;
  
  const int LedPin=13;
  
 //las variables cambian
 
 boolean Dstate = 0;
 
 int Astate = 0;
 
 void setup()
 
 {
  pinMode(ledPin,OUTPUT); //inicializa el LED como salida
  pinMode(digitalPin,INPUT)
  Serial.begin(9600);
  }
  
void loop()
{
  Astate = analogRead(analogPin);  
  Dstate = digitalRead(digitalPin);  
  Serial.print("D0; ");  
  
  Serial.prinln(Dstate);  

## PRIMERA VERSIÓN CON ARDUINO  
#include <OneWire.h>  
#include <DallasTemperature.h>  
#include <LiquidCrystal_I2C.h>  
#include <Wire.h>  
LiquidCrystal_I2C lcd(0x27,16,2);  // configure la dirección LCD en 0x27 y 0x3F para una pantalla de 16 caracteres y 2 líneas  
#define ONE_WIRE_BUS 7 //fija bus dado a pin 7  
OneWire oneWire(ONE_WIRE_BUS);  
DallasTemperature sensors(&oneWire);  

int green = 5; //fija pin 5 a color verde  
int red = 6; //fija pin 8 a color rojo  
int buzzer = 4; //fija pin 4 a buzzer  
int i;          //variable  
char cadena[]="Apagando la caja termica";  
 char dato;  
void setup(void)  
{  
  Serial.begin(9600);  
  pinMode(green,OUTPUT);  
  pinMode(red,OUTPUT);  
  pinMode(buzzer,OUTPUT);   
  sensors.begin(); // iniciar el sensor de temperatura  
  lcd.init(); //iniciar el pantalla de lcd  
  lcd.backlight(); //encentar la luz de pantalla  
}  
void loop(void)  
{   
   //no sona el buzzer  
   digitalWrite(buzzer,HIGH);  
   if(Serial.available()>0)  
  {  
    dato = Serial.read();  
    if (dato =='1')  
    {  
      sensors.requestTemperatures(); //Envía el comando para obtener temperaturas  
      lcd.setCursor(0, 0); //fija el posición donde parece primer character  
      lcd.print("TemC: "); //print "Tem: " en lcd   
      lcd.print(sensors.getTempCByIndex(0));//print la temperatura en lcd  
      lcd.print(char(223));//print the unit" ℃ "  
      lcd.print("C");  

      Serial.print("Tem: ");  
      Serial.print(sensors.getTempCByIndex(0));  
      Serial.println(" C");  
  
       if(sensors.getTempCByIndex(0)>40)  
      {  
          //LED con el color rojo   
           digitalWrite(red, HIGH);   
           digitalWrite(green, LOW);    
           delay(1000);   
          //aviso en pantalla  
           lcd.setCursor(0, 1);  
           lcd.print("Tem inconveniente");  
           Serial.println("Temperatura inconveniente");  
           //ouput buzzer  
           for(i=0;i<80;i++)  
          {  
             digitalWrite(buzzer,HIGH);  
             delay(1);//espera 1ms  
             digitalWrite(buzzer,LOW);  
             delay(1);//espera 1ms  
          }  
           for(i=0;i<100;i++)  
          {  
             digitalWrite(buzzer,HIGH);  
             delay(2);//espera 2ms  
             digitalWrite(buzzer,LOW);  
             delay(2);//espera 2ms  
          }  
       }  
       if(sensors.getTempCByIndex(0)<40)  
       {  
           //aviso en pantalla  
           lcd.setCursor(0, 1);  
           lcd.print("Tem adecuada");  
           Serial.println("Temperatura adecuada");  
           //LED con el color verde  
           digitalWrite(green, HIGH);    
           digitalWrite(red, LOW);    
           delay(1000);   
           //no sona el buzzer  
           digitalWrite(buzzer,HIGH);  
       }  
    }  
       else   
       if (dato =='0')  
       {  
         Serial.println("Apagando la caja termica！");  
         lcd.clear();  
         lcd.setCursor(15, 1);  
         for (i = 0; i < 40; i++)  
         {  
           lcd.scrollDisplayLeft();   
           lcd.print(cadena[i]);    
           delay(500);    
         }  
       }  
  }  
}  


ULTIMA VERSION 


Codigo en C:

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "SerialPort.h"
#define MAX_DATA_LENGTH 255

// Funciones prototipo
void autoConnect(SerialPort *arduino,char*);
int main(void)
{
 //Arduino SerialPort object
 SerialPort *arduino;
 char* portName = "\\\\.\\COM3";
 // Buffer para datos procedentes de Arduino
 char incomingData[MAX_DATA_LENGTH];

 // Crear estructura de datos del puerto serie
 arduino = (SerialPort *)malloc(sizeof(SerialPort));
 // Apertura del puerto serie
 Crear_Conexion(arduino,portName);
 autoConnect(arduino,incomingData);
 return 0;
}
void autoConnect(SerialPort *arduino,char *incomingData)
{
 char sendData = 0;

   // Espera la conecion con Arduino
   while (!isConnected(arduino))
   {
     Sleep(100);
     Crear_Conexion(arduino,arduino->portName);
   }
 //Comprueba si arduino est?connectado
   if (isConnected(arduino))
   {
     printf ("Conectado con Arduino en el puerto %s\n",arduino->portName);
   }
 // Bucle de la aplicacion
    printf ("0 - CAJA TERMICA OFF, 1 - CAJA TERMICA ON");
    while (isConnected(arduino) && sendData!='9')
   {
    sendData = getch();
    writeSerialPort(arduino,&sendData, sizeof(char));
   }
    if (!isConnected(arduino))
    printf ("Se ha perdido la conecion con Arduino\n");
}



#include <stdio.h>
#include <stdlib.h>
#include "SerialPort.h"

void Crear_Conexion(SerialPort *PuertoSerie, char *portName)
{

	PuertoSerie->connected = 0;
	PuertoSerie->portName = portName;
    PuertoSerie->handler = CreateFileA((portName),
                                GENERIC_READ | GENERIC_WRITE,
                                0,
                                NULL,
                                OPEN_EXISTING,
                                FILE_ATTRIBUTE_NORMAL,
                                NULL);
    if (PuertoSerie->handler == INVALID_HANDLE_VALUE)
	{
        if (GetLastError() == ERROR_FILE_NOT_FOUND)
		{
            printf("ERROR: Handle was not attached. Reason: %s not available\n", portName);
        }
    	else
        {
            printf("ERROR!!!");
        }
    }
    else 
	{
        DCB dcbSerialParameters = {0};

        if (!GetCommState(PuertoSerie->handler, &dcbSerialParameters))
		{
            printf("failed to get current serial parameters");
        }
        else 
		{
            dcbSerialParameters.BaudRate = CBR_9600;
            dcbSerialParameters.ByteSize = 8;
            dcbSerialParameters.StopBits = ONESTOPBIT;
            dcbSerialParameters.Parity = NOPARITY;
            dcbSerialParameters.fDtrControl = DTR_CONTROL_ENABLE;

            if (!SetCommState(PuertoSerie->handler, &dcbSerialParameters))
            {
                printf("ALERT: could not set Serial port parameters\n");
            }
            else 
			{
                PuertoSerie->connected = 1;
                PurgeComm(PuertoSerie->handler, PURGE_RXCLEAR | PURGE_TXCLEAR);
                Sleep(ARDUINO_WAIT_TIME);
            }
        }
    }
    return ;
}

void CerrarConexion(SerialPort * PuertoSerie)
{
    if (PuertoSerie->connected)
	{
        PuertoSerie->connected = 0;
        CloseHandle(PuertoSerie->handler);
    }
}

int readSerialPort(SerialPort * PuertoSerie,char *buffer, unsigned int buf_size)
{
    DWORD bytesRead;
    unsigned int toRead = 0;

    ClearCommError(PuertoSerie->handler, &PuertoSerie->errors, &PuertoSerie->status);

    if (PuertoSerie->status.cbInQue > 0)
	{
        if (PuertoSerie->status.cbInQue > buf_size)
		{
            toRead = buf_size;
        }
        else toRead = PuertoSerie->status.cbInQue;
    }

    memset(buffer, 0, buf_size);

    if (ReadFile(PuertoSerie->handler, buffer, toRead, &bytesRead, NULL)) return bytesRead;

    return 0;
}

int writeSerialPort(SerialPort *PuertoSerie,char *buffer, unsigned int buf_size)
{
    DWORD bytesSend;

    if (!WriteFile(PuertoSerie->handler, (void*) buffer, buf_size, &bytesSend, 0))
	{
        ClearCommError(PuertoSerie->handler, &PuertoSerie->errors, &PuertoSerie->status);
        return 0;
    }
    else return 1;
}

int isConnected(SerialPort *PuertoSerie)
{
    if (!ClearCommError(PuertoSerie->handler, &PuertoSerie->errors, &PuertoSerie->status))
		PuertoSerie->connected = 0;
    return PuertoSerie->connected;
}


Codigo en arduino:
modo 1:
#include <OneWire.h>
#include <DallasTemperature.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h>
LiquidCrystal_I2C lcd(0x27,16,2);  // configure la dirección LCD en 0x27 y 0x3F para una pantalla de 16 caracteres y 2 líneas
#define ONE_WIRE_BUS 7 //fija bus dado a pin 7
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

int green = 5; //fija pin 5 a color verde
int red = 6; //fija pin 8 a color rojo
int buzzer = 4; //fija pin 4 a buzzer
int i,j;          //variable
void setup(void)
{
  Serial.begin(9600);
  pinMode(green,OUTPUT);
  pinMode(red,OUTPUT);
  pinMode(buzzer,OUTPUT);
  sensors.begin(); // iniciar el sensor de temperatura
  lcd.init(); //iniciar el pantalla de lcd
  lcd.backlight(); //encentar la luz de pantalla
}
void loop(void)
{ 
   //no sona el buzzer
   digitalWrite(buzzer,HIGH);
   
      sensors.requestTemperatures(); //Envía el comando para obtener temperaturas
      lcd.setCursor(0, 0); //fija el posición donde parece primer character
      lcd.print("TemC: "); //print "Tem: " en lcd
      lcd.print(sensors.getTempCByIndex(0));//print la temperatura en lcd
      lcd.print(char(223));//print the unit" ℃ "
      lcd.print("C");

      Serial.print("Tem: ");
      Serial.print(sensors.getTempCByIndex(0));
      Serial.println(" C");
  
       if(sensors.getTempCByIndex(0)>40)
      {
          //LED con el color rojo
           digitalWrite(red, HIGH); 
           digitalWrite(green, LOW);  
           delay(1); 
          //aviso en pantalla
           lcd.setCursor(0, 1);
           lcd.print("Tem inconveniente");
           Serial.println("Temperatura inconveniente");
           //ouput buzzer
           for(j=0;j<3;j++)
           {
             for(i=0;i<80;i++)
            {
             digitalWrite(buzzer,HIGH);
             delay(1);//espera 1ms
             digitalWrite(buzzer,LOW);
             delay(1);//espera 1ms
            }
             for(i=0;i<100;i++)
            {
             digitalWrite(buzzer,HIGH);
             delay(2);//espera 2ms
             digitalWrite(buzzer,LOW);
             delay(2);//espera 2ms
            }
           }
       }
       if(sensors.getTempCByIndex(0)<40)
       {
           //aviso en pantalla
           lcd.setCursor(0, 1);
           lcd.print("Tem adecuada");
           Serial.println("Temperatura adecuada");
           //LED con el color verde
           digitalWrite(green, HIGH);  
           digitalWrite(red, LOW);  
           delay(1000); 
           //no sona el buzzer
           digitalWrite(buzzer,HIGH);
       }
  
}


modo 2:

#include <OneWire.h>
#include <DallasTemperature.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h>
LiquidCrystal_I2C lcd(0x27,16,2);  // configure la dirección LCD en 0x27 y 0x3F para una pantalla de 16 caracteres y 2 líneas
#define ONE_WIRE_BUS 7 //fija bus dado a pin 7
OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

int green = 5; //fija pin 5 a color verde
int red = 6; //fija pin 8 a color rojo
int buzzer = 4; //fija pin 4 a buzzer
int i,j;          //variable
char cadena[]="Apagando la caja termica";
void setup(void)
{
  Serial.begin(9600);
  pinMode(green,OUTPUT);
  pinMode(red,OUTPUT);
  pinMode(buzzer,OUTPUT);
  sensors.begin(); // iniciar el sensor de temperatura
  lcd.init(); //iniciar el pantalla de lcd
  lcd.backlight(); //encentar la luz de pantalla
}
void loop(void)
{ 
  char dato;
   //no sona el buzzer
   digitalWrite(buzzer,HIGH);
   if(Serial.available()>0)
  {
    dato = Serial.read();
    if (dato =='1')
    {
      lcd.backlight(); 
      lcd.clear();
      sensors.requestTemperatures(); //Envía el comando para obtener temperaturas
      lcd.setCursor(0, 0); //fija el posición donde parece primer character
      lcd.print("TemC: "); //print "Tem: " en lcd
      lcd.print(sensors.getTempCByIndex(0));//print la temperatura en lcd
      lcd.print(char(223));//print the unit" ℃ "
      lcd.print("C");

      Serial.print("Tem: ");
      Serial.print(sensors.getTempCByIndex(0));
      Serial.println(" C");
  
       if(sensors.getTempCByIndex(0)>30)
      {
          //LED con el color rojo
           digitalWrite(red, HIGH); 
           digitalWrite(green, LOW);  
           delay(1000); 
          //aviso en pantalla
           lcd.setCursor(0, 1);
           lcd.print("Tem inconveniente");
           Serial.println("Temperatura inconveniente");
           //ouput buzzer
           for(j=0;j<10;j++)
           {
              for(i=0;i<80;i++)
             {
             digitalWrite(buzzer,HIGH);
             delay(1);//espera 1ms
             digitalWrite(buzzer,LOW);
             delay(1);//espera 1ms
             }
              for(i=0;i<100;i++)
             {
              digitalWrite(buzzer,HIGH);
              delay(2);//espera 2ms
              digitalWrite(buzzer,LOW);
              delay(2);//espera 2ms
              }
           
            }
       }
       if(sensors.getTempCByIndex(0)<30)
       {
           //aviso en pantalla
           lcd.setCursor(0, 1);
           lcd.print("Tem adecuada");
           Serial.println("Temperatura adecuada");
           //LED con el color verde
           digitalWrite(green, HIGH);  
           digitalWrite(red, LOW);  
           delay(1000); 
           //no sona el buzzer
           digitalWrite(buzzer,HIGH);
       }
    }
       else 
       if (dato =='0')
       {
         Serial.println("Apagando la caja termica！");
         //LED con el color verde
           digitalWrite(green, LOW);  
           digitalWrite(red, LOW);  
         lcd.clear();
         lcd.setCursor(15, 1);
         for (i = 0; i < 40; i++)
         {
           lcd.scrollDisplayLeft(); 
           lcd.print(cadena[i]);  
           delay(500);  
         }
         lcd.clear();
         lcd.noBacklight();
       }
  }
}
