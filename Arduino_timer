// Пины
const int potPin = A0;        // Пин для подключения потенциометра
const int buttonPin = 2;      // Пин для подключения кнопки
const int ledPin = 13;        // Пин для подключения светодиода

// Переменные
int potValue = 0;             // Значение с потенциометра
int timerDuration = 0;        // Продолжительность таймера в секундах
unsigned long startTime = 0;  // Время начала таймера
bool timerRunning = false;    // Флаг состояния таймера
bool ledBlinking = false;     // Флаг мигания светодиода
unsigned long blinkStartTime = 0; // Время начала мигания

void setup() {
  pinMode(buttonPin, INPUT_PULLUP); // Установка кнопки с подтягивающим резистором
  pinMode(ledPin, OUTPUT);          // Установка светодиода как выхода
  Serial.begin(9600);               // Инициализация последовательного порта для отладки
}

void loop() {
  // Чтение значения с потенциометра
  potValue = analogRead(potPin);
  
  // Преобразование значения в диапазон времени (например, от 1 до 30 минут)
  timerDuration = map(potValue, 0, 1023, 60, 1800);

  // Проверка нажатия кнопки
  if (digitalRead(buttonPin) == LOW) {
    delay(50); // Задержка для устранения дребезга
    if (digitalRead(buttonPin) == LOW) { // Проверка повторно после задержки
      startTime = millis(); // Запоминаем время начала
      timerRunning = true;  // Устанавливаем флаг запущенного таймера
      ledBlinking = false;  // Сбрасываем флаг мигания светодиода
      Serial.print("Таймер запущен на ");
      Serial.print(timerDuration / 60);
      Serial.println(" минут.");
    }
  }

  // Если таймер запущен
  if (timerRunning) {
    unsigned long currentTime = millis();
    unsigned long elapsedTime = (currentTime - startTime) / 1000; // Время в секундах

    // Если истекло время таймера
    if (elapsedTime >= timerDuration) {
      digitalWrite(ledPin, HIGH); // Включаем светодиод
      Serial.println("Таймер завершен!");
      timerRunning = false;       // Сбрасываем флаг запущенного таймера
      ledBlinking = true;         // Устанавливаем флаг мигания светодиода
      blinkStartTime = currentTime; // Запоминаем время начала мигания
    } else {
      digitalWrite(ledPin, LOW);  // Выключаем светодиод
      Serial.print("Осталось: ");
      Serial.print((timerDuration - elapsedTime) / 60);
      Serial.print(" минут ");
      Serial.print((timerDuration - elapsedTime) % 60);
      Serial.println(" сек.");
    }
  }

  // Мигание светодиода
  if (ledBlinking) {
    unsigned long currentTime = millis();
    unsigned long blinkElapsedTime = (currentTime - blinkStartTime) / 1000; // Время в секундах

    //светодиод или любой зуммер(не зумер!) для звука оповещения
    // Мигаем каждые 2 секунды
    if (blinkElapsedTime % 4 < 2) {
      digitalWrite(ledPin, HIGH); // Включаем светодиод
    } else {
      digitalWrite(ledPin, LOW);  // Выключаем светодиод
    }
  }
}
