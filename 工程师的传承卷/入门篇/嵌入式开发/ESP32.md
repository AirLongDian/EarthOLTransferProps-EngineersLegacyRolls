### 3.2.5.4 Arduino + ESP32S3R8 Arduino库
```cpp
/*
  ESP32-S3R8 Arduino 点灯示例
  功能：板载 LED 每隔 1 秒闪烁一次
*/

// 定义板载 LED 引脚
// ESP32-S3R8 板载 LED 一般连接在 GPIO48
#define LED_PIN 48

void setup() {
  // 设置 LED 引脚为输出模式
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  // 点亮 LED
  digitalWrite(LED_PIN, HIGH);
  delay(1000); // 延时 1 秒

  // 熄灭 LED
  digitalWrite(LED_PIN, LOW);
  delay(1000); // 延时 1 秒
}
```

[嵌入式与 MCU 入门](%E5%B5%8C%E5%85%A5%E5%BC%8F%E4%B8%8EMCU%E5%85%A5%E9%97%A8.md)
