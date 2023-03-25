语音合成模块Demo

耳机插着就行，别戴着，声音真的很大！

Serial1的Write值根据手册中的数据帧格式写的。可以直接用“SYN6658语音合成芯片PC端演示程序”生成。

ESP32-C3的Serial有两个。第0个就是和电脑通信的串口。第1个就是4PIN-UART接口。第1个接口记得是Serial1，不是Serial。

```c
void setup() {
  // put your setup code here, to run once:
  Serial1.begin(115200);

}

void loop() {
  // put your main code here, to run repeatedly:
  Serial1.write(0xFD);
  Serial1.write(0x00);
  Serial1.write(0x08);
  Serial1.write(0x01);
  Serial1.write(0x01);
  Serial1.write(0xD2);
  Serial1.write(0xBB);
  Serial1.write(0xB6);
  Serial1.write(0xFE);
  Serial1.write(0xC8);
  Serial1.write(0xFD);
  delay(3000);
}
```



EEPROM模块Demo

注意写操作之后需要delay。

```c
#include<Wire.h>

#define eepADDR 0x50

void setup() {
  // put your setup code here, to run once
  Serial.begin(115200);
  // 写入操作
  Wire.begin();
  Wire.beginTransmission(eepADDR);
  Wire.endTransmission();
  Wire.beginTransmission(eepADDR);
  Wire.write(0x00); // 地址高8位
  Wire.write(0x0C); // 地址低8位
  Wire.write(0x06); // 写入的数据
  Wire.endTransmission();
  delay(200);
  
}

void loop() {
  // put your main code here, to run repeatedly:
  // 读操作
  Wire.beginTransmission(eepADDR);
  Wire.write(0x00); //地址高8位
  Wire.write(0x0C); //地址低8位
  Wire.endTransmission();
  delay(100);
  Wire.requestFrom(eepADDR, 1);
  if (Wire.available()) {
    int c = Wire.read();
    Serial.println(c);
  }
  delay(1000);
}
```



DS1307和LED16x16直接看Arduino自带的示例程序吧
