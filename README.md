# Digital Keypad Security Door Lock using Arduino
<br><img src="./img/Posterr.png" width=632 height=895>
## บทคัดย่อ
<br>ชื่อโครงงานภาษาไทย :  ระบบล็อคประตูโดยใช้ Arduino
<br>ชื่อโครงงานภาษาอังกฤษ : Digital Keypad Security Door Lock using Arduino
<br>ชื่อผู้ทำโครงงาน : นาย กิตตินันท์ เจริญทรง 64070007
<br>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;นาย พัสกร คำแก้ว 64070073
<br>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;นาย พุฒิพงษ์ ชอบงาม 64070079
<br>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;นาย ธนวรรษ นันทะกูล 64070158
<br>ปีที่ทำโครงงาน : 2565

<br>&emsp;&emsp;&emsp;&emsp;โครงงานเรื่อง ระบบล็อคประตูโดยใช้ Arduino จัดทำขึ้นโดยมีวัตถุประสงค์เพื่อใช้ในการศึกษาเกี่ยวกับการทำงานของ Arduino และยังสามารถนำไปต่อยอดโดยการเอาไปใช้จริงในชีวิตประจำวันได้
<br>&emsp;&emsp;&emsp;&emsp;สรุปผลการทำโครงงาน ได้ทำการต่อบอร์ด Arduino เข้ากับวงจรจากนั้นได้ทำการเขียนโค้ดให้บอร์ด Arduino ซึ่งวงจรกับบอร์ด Arduino สามารถทำงานได้ปกติ โดยรวมแล้วระบบล็อคประตูโดยใช้ Arduino สามารถทำงานได้ปกติ
<br>&emsp;&emsp;&emsp;&emsp;คำสำคัญ ระบบล็อค, บอร์ด Arduino
## เกี่ยวกับโปรเจค
<br>&emsp;&emsp;&emsp;&emsp;ระบบล็อคประตูโดยใช้ Arduino board จัดทำขึ้นโดยมีวัตถุประสงค์เพื่อใช้ในการศึกษาเกี่ยวกับการทำงานของ Arduino และยังสามารถนำไปใช้เพื่อยกระดับความปลอดภัยได้
## Arduino Board
<br><img src="./img/arduino.png" width=442 height=391>
## Start
```cpp
#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Servo.h>
<!--ความยาวpassword  -->
#define Password_Length 5

Servo myservo;
LiquidCrystal lcd(A0, A1, A2, A3, A4, A5);

int pos = 0;

char Data[Password_Length];
<!--password-->
char Master[Password_Length] = "1234";
byte data_count = 0, master_count = 0;

bool Pass_is_good;
bool door = false;
char customKey;
```
## Keypad
```cpp
const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};


byte rowPins[ROWS] = {0, 1, 2, 3};
byte colPins[COLS] = {4, 5, 6, 7};

Keypad customKeypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS);
```
## Servo
```cpp
void ServoClose()
{
  for (pos = 90; pos >= 0; pos -= 10) { 
    myservo.write(pos);
  }
}

void ServoOpen()
{
  for (pos = 0; pos <= 90; pos += 10) {
    myservo.write(pos);  
  }
}
```
## Main Action
```cpp
void setup()
{
  myservo.attach(9, 2000, 2400);
  ServoClose();
  lcd.begin(16, 2);
  lcd.print("Protected Door");
  loading("Loading");
  lcd.clear();
}


void loop()
{
  if (door == true)
  {
    customKey = customKeypad.getKey();
    if (customKey == '#')
    {
      lcd.clear();
      ServoClose();
      lcd.print("Door is closed");
      delay(3000);
      door = false;
    }
  }
  else
    Open();

}

void loading (char msg[]) {
  lcd.setCursor(0, 1);
  lcd.print(msg);

  for (int i = 0; i < 9; i++) {
    delay(1000);
    lcd.print(".");
  }
}

void clearData()
{
  while (data_count != 0)
  { 
    Data[data_count--] = 0;
  }
  return;
}

void Open()
{
  lcd.setCursor(0, 0);
  lcd.print("Enter Password");
  
  customKey = customKeypad.getKey();
  if (customKey)
  {
    Data[data_count] = customKey;
    lcd.setCursor(data_count, 1);
    lcd.print(Data[data_count]);
    data_count++;
  }

  if (data_count == Password_Length - 1)
  {
    if (!strcmp(Data, Master))
    {
      lcd.clear();
      ServoOpen();
      lcd.print(" Door is Open ");
      door = true;
      delay(5000);
      loading("Waiting");
      lcd.clear();
      lcd.print(" Time is up! ");
      delay(1000);
      ServoClose();
      door = false;      
    }
    else
    {
      lcd.clear();
      lcd.print(" Wrong Password ");
      door = false;
    }
    delay(1000);
    lcd.clear();
    clearData();
  }
}
```
## แบบจำลองวงจร
<br>https://wokwi.com/projects/331185567322604116
## สมาชิก
<br>นาย กิตตินันท์ เจริญทรง 64070007
<br>นาย พัสกร คำแก้ว 64070073
<br>นาย พุฒิพงษ์ ชอบงาม 64070079
<br>นาย ธนวรรษ นันทะกูล 64070158
