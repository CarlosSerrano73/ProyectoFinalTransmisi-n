CODIGO DEL PROGRAMA (EXPLICADO):


#include <Wire.h> // Incluye la biblioteca para comunicación I2C
#include <LiquidCrystal_I2C.h> // Incluye la biblioteca para el controlador de pantalla LCD I2C
#include <DHT.h> // Incluye la biblioteca para el sensor de temperatura y humedad DHT

#define DHTPIN 15 // Define el pin al que está conectado el sensor DHT
#define DHTTYPE DHT11 // Define el tipo de sensor DHT utilizado

LiquidCrystal_I2C lcd(0x27, 16, 2); // Crea un objeto de pantalla LCD (dirección I2C, columnas, filas)
DHT dht(DHTPIN, DHTTYPE); // Crea un objeto de sensor DHT

void setup() {
  Serial.begin(115200); // Inicia la comunicación serial a 115200 bps
  dht.begin(); // Inicia el sensor DHT
  lcd.init(); // Inicia la pantalla LCD
  lcd.backlight(); // Enciende la retroiluminación de la pantalla LCD
}

void loop() {
  delay(5000); // Espera 5 segundos
  float t = dht.readTemperature(); // Lee la temperatura del sensor DHT
  float h = dht.readHumidity(); // Lee la humedad del sensor DHT
  if (isnan(h) || isnan(t)) { // Verifica si los valores leídos son 'NaN' (no un número)
    Serial.println("¡Error al leer del sensor DHT!"); // Imprime un mensaje de error si los valores son 'NaN'
  } else {
    lcd.clear(); // Limpia la pantalla LCD
    lcd.setCursor(0, 0); // Posiciona el cursor en la primera columna de la primera fila
    lcd.print("Temp: "); // Imprime el texto "Temp:"
    lcd.print(t); // Imprime la temperatura leída
    lcd.print(" C"); // Imprime la letra 'C' de Celsius

    lcd.setCursor(0, 1); // Posiciona el cursor en la primera columna de la segunda fila
    lcd.print("Hum: "); // Imprime el texto "Hum:"
    lcd.print(h); // Imprime la humedad leída
    lcd.print(" %"); // Imprime el símbolo '%'
  }
}
