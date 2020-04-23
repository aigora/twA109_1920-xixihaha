# Título del trabajo

caja termometro

## Integrantes del equipo
Xiaolong Ruan(54978), Wei Zheng (54911）

## Objetivos del trabajo

Una caja que nos podra medir la temperatura, con la ayuda de lenguaje c, permitira que envie señales al arduino y que se brille la led que esta conectada a la caja y por ultimo el ordenador alerta varios tipos de sonido tras el alcance de la temperatura a un rango determinado

## Sensor y actuadores

LED.   
Los diodos emisores de luz (LED) que usamos es un compusta de led y resistecias,son capaces de emitir dos colores diferentes de luz, generalmente rojo y verde.Tiene 3 pines,que son del RED,el GREEN y lo a tierra(GND).Modificamos el valor de voltaje de los dos pines para controlar el color de LED,Por lo tanto, este tipo de LED a menudo se utiliza como luz indicadora de varios dispositivos (cámaras de video, cámaras digitales y controles remotos).

Zumbador.   
Un zumbador (en inglés: buzzer) es un transductor electroacústico que produce un sonido o zumbido continuo o intermitente de un mismo tono (generalmente agudo). Sirve como mecanismo de señalización o aviso y se utiliza en múltiples sistemas, como en automóviles o en electrodomésticos, incluidos los despertadores.

Sensor de temperatura.    
Los sensores de temperatura es componentes electrónicos que convierten la temperatura en datos electrónicos. Un sensor de temperatura que utiliza un cuerpo conductor cuya resistencia cambia con la temperatura. El elemento más utilizado es el platino, que tiene una resistencia de 100 ohmios a 0 ° C. Los sensores de temperatura de semiconductores generalmente integran circuitos de amplificación y ajuste. La frecuencia de oscilación de un oscilador de cristal varía con la temperatura, por lo que puede medir la temperatura con mucha precisión.

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
