# Codis ESP32 (MicroPython i C/C++)

Alumne: Iván Cárdenas Polonio - Projecte 2on ASIX - Institut Camí de Mar - Cada projecte té dues versions (MicroPython i C/C++) executades a Wokwi.

---

## Projecte 1 - ESP32 + sensor DHT22

- Original MicroPython: https://wokwi.com/projects/412218784278139905
- Original C/C++: https://wokwi.com/projects/403463453021350913
- Modificació: avís "Fa calor!" quan la temperatura supera els 30 °C.

### Projecte 1 - MicroPython (main.py)

```python
from machine import Pin   # Importa Pin per controlar les pates de l'ESP32
from time import sleep     # Importa sleep per fer pauses
import dht                 # Importa la llibreria del sensor DHT22

sensor = dht.DHT22(Pin(17))   # Crea el sensor DHT22 a la pata 17

while True:                   # Bucle infinit: es repeteix per sempre
    sensor.measure()          # Fa una lectura del sensor
    temp = sensor.temperature()   # Guarda la temperatura (°C)
    humitat = sensor.humidity()   # Guarda la humitat (%)
    print("Temperatura:", temp, "Humitat:", humitat)   # Mostra les dades per la consola
    if temp > 30:             # MODIFICACIÓ: si fa més de 30 °C...
        print("Fa calor!")    # ...avisa que fa calor
    sleep(2)                  # Espera 2 segons
```

### Projecte 1 - C/C++ (sketch.ino)

```cpp
#include <DHT.h>              // Importa la llibreria del sensor DHT

DHT dht(15, DHT22);           // Crea el sensor DHT22 al pin 15

void setup() {                // S'executa una sola vegada en arrencar
  Serial.begin(115200);       // Inicia la consola sèrie
  dht.begin();                // Inicia el sensor
}

void loop() {                 // S'executa una vegada i una altra, per sempre
  float temp = dht.readTemperature();   // Llegeix la temperatura (°C)
  float humitat = dht.readHumidity();   // Llegeix la humitat (%)
  Serial.print("Temperatura: ");        // Mostra el text de la temperatura
  Serial.print(temp);                   // Mostra el valor de la temperatura
  Serial.print(" Humitat: ");           // Mostra el text de la humitat
  Serial.println(humitat);              // Mostra el valor de la humitat i salta de línia
  if (temp > 30) {                      // MODIFICACIÓ: si fa més de 30 °C...
    Serial.println("Fa calor!");        // ...avisa que fa calor
  }
  delay(2000);                          // Espera 2 segons
}
```

---

## Projecte 2 - ESP32 + potenciòmetre

- Original MicroPython: https://wokwi.com/projects/393408851555021825
- Original C/C++: https://wokwi.com/projects/392663730313084929
- Modificació: conversió del valor a percentatge (0-100 %).

### Projecte 2 - MicroPython (main.py)

```python
from machine import Pin, ADC   # Importa Pin i ADC (lector de senyals analogics)
from time import sleep         # Importa sleep per fer pauses

pot = ADC(Pin(34))             # Crea el potenciometre a la pata 34
pot.atten(ADC.ATTN_11DB)       # Permet llegir tot el rang de voltatge (0 - 3,3 V)

while True:                    # Bucle infinit: es repeteix per sempre
    valor = pot.read()         # Llegeix el valor del potenciometre (0 - 4095)
    percentatge = valor * 100 // 4095   # MODIFICACIO: converteix el valor a percentatge (0 - 100)
    print("Valor:", valor, "Percentatge:", percentatge, "%")   # Mostra les dades per la consola
    sleep(0.5)                 # Espera mig segon
```

### Projecte 2 - C/C++ (sketch.ino)

```cpp
const int POT = 34;                  // Pin del potenciometre (GPIO 34)

void setup() {                       // S'executa una sola vegada en arrencar
  Serial.begin(115200);              // Inicia la consola serie
}

void loop() {                        // S'executa una vegada i una altra, per sempre
  int valor = analogRead(POT);       // Llegeix el valor del potenciometre (0 - 4095)
  int percentatge = valor * 100 / 4095;   // MODIFICACIO: converteix el valor a percentatge (0 - 100)
  Serial.print("Valor: ");           // Mostra el text del valor
  Serial.print(valor);               // Mostra el valor llegit
  Serial.print(" Percentatge: ");    // Mostra el text del percentatge
  Serial.print(percentatge);         // Mostra el percentatge
  Serial.println(" %");              // Acaba la linia
  delay(500);                        // Espera mig segon
}
```

---

## Projecte 3 - ESP32 + LED parpellejant

- Original MicroPython: https://wokwi.com/projects/393493930105498625
- Original C/C++: https://wokwi.com/projects/318171894571336257
- Modificació: el LED parpelleja 3 vegades ràpid i fa una pausa d'1 segon; es mostra l'estat per la consola.

### Projecte 3 - MicroPython (main.py)

```python
from machine import Pin   # Importa Pin per controlar les pates de l'ESP32
from time import sleep    # Importa sleep per fer pauses

led = Pin(21, Pin.OUT)    # Crea el LED a la pata 21, com a sortida

while True:               # Bucle infinit: es repeteix per sempre
    for i in range(3):    # MODIFICACIO: repeteix 3 vegades el parpelleig
        led.on()          # Encen el LED
        print("LED ences")    # Mostra el missatge per la consola
        sleep(0.2)        # Espera 0,2 segons
        led.off()         # Apaga el LED
        print("LED apagat")   # Mostra el missatge per la consola
        sleep(0.2)        # Espera 0,2 segons
    sleep(1)              # MODIFICACIO: pausa d'1 segon abans de tornar a comencar
```

### Projecte 3 - C/C++ (sketch.ino)

```cpp
const int LED = 2;                // Pin del LED (GPIO 2, el LED integrat de l'ESP32)

void setup() {                    // S'executa una sola vegada en arrencar
  pinMode(LED, OUTPUT);           // Configura el pin del LED com a sortida
  Serial.begin(115200);           // MODIFICACIO: inicia la consola serie per veure missatges
}

void loop() {                     // S'executa una vegada i una altra, per sempre
  for (int i = 0; i < 3; i++) {   // MODIFICACIO: repeteix 3 vegades el parpelleig
    digitalWrite(LED, HIGH);      // Encen el LED
    Serial.println("LED ences");  // Mostra el missatge per la consola
    delay(200);                   // Espera 200 mil.lisegons
    digitalWrite(LED, LOW);       // Apaga el LED
    Serial.println("LED apagat"); // Mostra el missatge per la consola
    delay(200);                   // Espera 200 mil.lisegons
  }
  delay(1000);                    // MODIFICACIO: pausa d'1 segon abans de tornar a comencar
}
```

---

## Projecte 4 - ESP32 + botó i LED

- Original MicroPython: https://wokwi.com/projects/452780197373810689
- Original C/C++: https://wokwi.com/projects/461362571481332737
- Modificació: comptador de premudes; el LED s'encén amb les imparelles i s'apaga amb les parelles.

### Projecte 4 - MicroPython (main.py)

```python
from machine import Pin   # Importa Pin per controlar les pates de l'ESP32
from time import sleep    # Importa sleep per fer pauses

led = Pin(2, Pin.OUT)               # Crea el LED a la pata 2, com a sortida
boto = Pin(4, Pin.IN, Pin.PULL_UP)  # Crea el boto a la pata 4 amb pull-up (sense premer = 1, premut = 0)

premuts = 0               # MODIFICACIO: comptador de vegades que s'ha premut el boto
ultim = 1                 # Guarda l'estat anterior del boto (1 = sense premer)

while True:               # Bucle infinit: es repeteix per sempre
    actual = boto.value() # Llegeix l'estat actual del boto
    if ultim == 1 and actual == 0:   # Si abans no estava premut i ara si, s'acaba de premer
        premuts = premuts + 1        # Suma 1 al comptador
        led.value(premuts % 2)       # MODIFICACIO: LED encés amb imparelles, apagat amb parelles
        print("Boto premut", premuts, "vegades")   # Mostra el comptador per la consola
        sleep(0.05)                  # Pausa curta per evitar rebots del boto
    ultim = actual        # Guarda l'estat actual per a la propera volta
    sleep(0.01)           # Pausa de 10 ms
```

### Projecte 4 - C/C++ (sketch.ino)

```cpp
const int BOTO = 4;               // Pin del boto (GPIO 4)
const int LED = 2;                // Pin del LED (GPIO 2)

int premuts = 0;                  // MODIFICACIO: comptador de vegades que s'ha premut el boto
int ultim = HIGH;                 // Estat anterior del boto (HIGH = sense premer)

void setup() {                    // S'executa una sola vegada en arrencar
  pinMode(BOTO, INPUT_PULLUP);    // Boto com a entrada amb pull-up (sense premer = HIGH, premut = LOW)
  pinMode(LED, OUTPUT);           // LED com a sortida
  Serial.begin(115200);           // Inicia la consola serie
}

void loop() {                     // S'executa una vegada i una altra, per sempre
  int actual = digitalRead(BOTO); // Llegeix l'estat actual del boto
  if (ultim == HIGH && actual == LOW) {   // Si abans no estava premut i ara si, s'acaba de premer
    premuts++;                            // Suma 1 al comptador
    digitalWrite(LED, premuts % 2);       // MODIFICACIO: LED encés amb premudes imparelles, apagat amb parelles
    Serial.print("Boto premut ");         // Mostra el missatge per la consola
    Serial.print(premuts);                // Mostra el valor del comptador
    Serial.println(" vegades");           // Acaba el missatge amb salt de linia
    delay(50);                            // Pausa curta per evitar rebots del boto
  }
  ultim = actual;                 // Guarda l'estat actual per a la propera volta
  delay(10);                      // Pausa de 10 ms
}
```
