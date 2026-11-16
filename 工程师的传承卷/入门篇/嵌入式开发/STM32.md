### 3.2.5.2 Clion + STM32F407VET6 HAL库
```cpp
#include "stm32f4xx_hal.h"

void GPIO_Init(void) {
    HAL_Init();             // 1. HAL 库初始化
    SystemClock_Config();   // 2. 系统时钟配置（CubeMX 生成）
    GPIO_InitTypeDef GPIO_InitStruct = {0};

    // 使能 GPIOF 时钟
    __HAL_RCC_GPIOF_CLK_ENABLE();

    // 配置 PF9 为推挽输出
    GPIO_InitStruct.Pin = GPIO_PIN_9;
    GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStruct.Pull = GPIO_NOPULL;
    GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
    HAL_GPIO_Init(GPIOF, &GPIO_InitStruct);

    // 默认熄灭 LED
    HAL_GPIO_WritePin(GPIOF, GPIO_PIN_9, GPIO_PIN_SET);
}

int main(void) {
    GPIO_Init(); 

    while (1) {
        HAL_GPIO_WritePin(GPIOF, GPIO_PIN_9, GPIO_PIN_RESET); // 点亮
        HAL_Delay(1000);
        HAL_GPIO_WritePin(GPIOF, GPIO_PIN_9, GPIO_PIN_SET);   // 熄灭
        HAL_Delay(1000);
    }
}


```
### 3.2.5.3 VS-Code + STM32F103C8T6 标准库
```cpp
#include "stm32f10x.h"

// 简单延时函数
void Delay_ms(uint32_t ms) {
    uint32_t i;
    while (ms--) {
        for (i = 0; i < 8000; i++); // 大约 1ms @ 72MHz
    }
}

void gpioinit() {
	// 使能 GPIOC 时钟
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);

    // 配置 PC13 用于推挽输出
    GPIO_InitTypeDef GPIO_InitStructure;
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP; // 推挽输出
    GPIO_Init(GPIOC, &GPIO_InitStructure);
}

int main(void) {
	gpioinit();
    while (1) {
        GPIO_ResetBits(GPIOC, GPIO_Pin_13); // 低电平点亮 LED
        Delay_ms(1000);                     // 延时 1 秒

        GPIO_SetBits(GPIOC, GPIO_Pin_13);   // 高电平熄灭 LED
        Delay_ms(1000);                     // 延时 1 秒
    }
}
```

[嵌入式与 MCU 入门](%E5%B5%8C%E5%85%A5%E5%BC%8F%E4%B8%8EMCU%E5%85%A5%E9%97%A8.md)
