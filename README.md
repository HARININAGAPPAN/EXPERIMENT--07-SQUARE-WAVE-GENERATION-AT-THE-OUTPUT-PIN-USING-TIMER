## EXPERIMENT 07 SQUARE WAVE GENERATION AT THE OUTPUT PIN USING TIMER

# Aim:
To generate a PWM wave at the timer pin output and simuate it on proteus using an virtual oscilloscope

# Components required:
STM32 CUBE IDE, Proteus 8 simulator .

# Theory:
The timer modules can operate a variety of modes one of which is the PWM mode. Where the timer gets clocked from an internal source and counts up to the auto-reload register value, then the output channel pin is driven HIGH. And it remains until the timer counts reach the CCRx register value, the match event causes the output channel pin to be driven LOW. And it remains until the timer counts up to the auto-reload register value, and so on.

The resulting waveform is called PWM (pulse-width modulated) signal. Whose frequency is determined by the internal clock, the Prescaler, and the ARRx register. And its duty cycle is defined by the channel CCRx register value. The PWM doesn’t always have to be following this exact same procedure for PWM generation, however, it’s the very basic one and the easier to understand the concept. It’s called the up-counting PWM mode. We’ll discuss further advanced PWM generation techniques as we go on in this series of tutorials.

The following diagram shows you how the ARR value affects the period (frequency) of the PWM signal. And how the CCRx value affects the corresponding PWM signal’s duty cycle. And illustrates the whole process of PWM signal generation in the up-counting normal mode.

STM32 Timers – PWM Output Channels

Each Capture/Compare channel is built around a capture/compare register (including a shadow register), an input stage for capture (with a digital filter, multiplexing, and Prescaler) and an output stage (with comparator and output control). The output stage generates an intermediate waveform which is then used for reference: OCxRef (active high). The polarity acts at the end of the chain. 
![Screenshot 2025-05-09 132100](https://github.com/user-attachments/assets/a48176d2-efcf-4b43-80ba-f769ac71fe61)


STM32 Timers In PWM Mode

Pulse width modulation mode allows generating a signal with a frequency determined by the value of the TIMx_ARR register and a duty cycle determined by the value of the TIMx_CCRx register. The PWM mode can be selected independently on each channel (one PWM per OCx output) by writing 110 (PWM mode 1) or ‘111 (PWM mode 2) in the OCxM bits in the TIMx_CCMRx register. The user must enable the corresponding preload register by setting the OCxPE bit in the TIMx_CCMRx register, and eventually the auto-reload preload register by setting the ARPE bit in the TIMx_CR1 register.

OCx polarity is software programmable using the CCxP bit in the TIMx_CCER register. It can be programmed as active high or active low. For applications where you need to generate complementary PWM signals, this option will be suitable for you.

In PWM mode (1 or 2), TIMx_CNT and TIMx_CCRx are always compared to determine whether TIMx_CCRx≤TIMx_CNT or TIMx_CNT≤TIMx_CCRx (depending on the direction of the counter).

The timer is able to generate PWM in edge-aligned mode or center-aligned mode depending on the CMS bits in the TIMx_CR1 register. STM32 PWM Frequency

In various applications, you’ll be in need to generate a PWM signal with a specific frequency. In servo motor control, LED drivers, motor drivers, and many more situations where you’ll be in need to set your desired frequency for the output PWM signal.

The PWM period (1/FPWM) is defined by the following parameters: ARR value, the Prescaler value, and the internal clock itself which drives the timer module FCLK. The formula down below is to be used for calculating the FPWM for the output. You can set the clock you’re using, the Prescaler, and solve for the ARR value in order to control the FPWM and get what you want.

STM32 PWM Frequency Formula - STM32 PWM Frequency Equation
![Screenshot 2025-05-09 132202](https://github.com/user-attachments/assets/4fe7c156-f2db-4437-a80e-7c7943d067e6)


STM32 PWM Duty Cycle

In normal settings, assuming you’re using the timer module in PWM mode and generating PWM signal in edge-aligned mode up-counting configuration. The duty cycle percentage is controlled by changing the value of the CCRx register. And the duty cycle equals (CCRx/ARR) [%].
![Screenshot 2025-05-09 132227](https://github.com/user-attachments/assets/1ca65b6a-dd38-4b00-b949-2cbead9ed291)


# Procedure:
Step1: Open CubeMX & Create New Project
![Screenshot 2025-05-09 132259](https://github.com/user-attachments/assets/a5af0987-9068-4a5b-b4b5-1c0122655346)



Step2: Choose The Target MCU & Double-Click Its Name select the target to be programmed as shown below and click on next
![Screenshot 2025-05-09 132347](https://github.com/user-attachments/assets/f61f7c2d-a3d2-4fc2-a9ee-5a104240fbd0)
![Screenshot 2025-05-09 132417](https://github.com/user-attachments/assets/6fc346ec-d109-4434-b757-6cd88caaf5ad)


Step3: Configure Timer2 Peripheral To Operate In PWM Mode With CH1 Output
![Screenshot 2025-05-09 132459](https://github.com/user-attachments/assets/e6d6b82e-6939-4c99-8172-81200cf95304)

Step4: Set The RCC External Clock Source
![Screenshot 2025-05-09 132533](https://github.com/user-attachments/assets/38bbea6e-2c92-41c1-b5b8-5d49d5a49339)


STM32 RCC External Clock Selection CubeMX

Step5: Go To The Clock Configuration

Step6: Set The System Clock To Be 72MHz
![Screenshot 2025-05-09 132559](https://github.com/user-attachments/assets/30fe54d9-2d7a-46ca-8d6b-cdf585593cf5)


Step7: Name & Generate The Project Initialization Code For CubeIDE or The IDE You’re Using

Step8. Creating Proteus project and running the simulation We are now at the last part of step by step guide on how to simulate STM32 project in Proteus.

Step9. Create a new Proteus project and place STM32F40xx i.e. the same MCU for which the project was created in STM32Cube IDE. 14. After creation of the circuit as per requirement as shown below
![Screenshot 2025-05-09 132631](https://github.com/user-attachments/assets/6fc56c7e-e80a-4133-8c01-a4a1db18abf8)


Step10. Double click on the the MCU part to open settings. Next to the Program File option, give full path to the Hex file generated using STM32Cube IDE. Then set the external crystal frequency to 8M (i.e. 8 MHz). Click OK to save the changes.

Step14. click on debug and simulate using simulation as shown below 
![Screenshot 2025-05-09 132704](https://github.com/user-attachments/assets/451ca3da-b77a-4937-9410-94fccbf8e1b7)


# STM 32 CUBE PROGRAM :
```
#include "main.h"


TIM_HandleTypeDef htim2;


void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_TIM2_Init(void);

int main(void)
{

  HAL_Init();


  SystemClock_Config();


  MX_GPIO_Init();
  MX_TIM2_Init();

  HAL_TIM_Base_Start(&htim2);
  HAL_TIM_PWM_Init(&htim2);
  HAL_TIM_PWM_Start(&htim2,TIM_CHANNEL_1);

  while (1)
  {

  }

}


void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};


  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}


static void MX_TIM2_Init(void)
{



  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};


  htim2.Instance = TIM2;
  htim2.Init.Prescaler = 0;
  htim2.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim2.Init.Period = 1000;
  htim2.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim2.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim2) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim2, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_PWM_Init(&htim2) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim2, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 900;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  if (HAL_TIM_PWM_ConfigChannel(&htim2, &sConfigOC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }

  HAL_TIM_MspPostInit(&htim2);

}


static void MX_GPIO_Init(void)
{


  __HAL_RCC_GPIOA_CLK_ENABLE();

}


void Error_Handler(void)
{

  __disable_irq();
  while (1)
  {
  }

}

#ifdef  USE_FULL_ASSERT

void assert_failed(uint8_t *file, uint32_t line)
{

}
#endif
```

# Output screen shots of proteus :
![Screenshot 2025-05-09 131153](https://github.com/user-attachments/assets/a77dd1ec-f03b-4e33-b3ee-2fa0c521c0e7)

# CIRCUIT DIAGRAM (EXPORT THE GRAPHICS TO PDF AND ADD THE SCREEN SHOT HERE):
![Screenshot 2025-05-09 131140](https://github.com/user-attachments/assets/93487f1b-4dc4-411c-9637-00ea59487682)


DUTY CYCLE AND FREQUENCY CALCULATION
![Screenshot 2025-05-09 132825](https://github.com/user-attachments/assets/9b298561-db2d-4151-a901-ef11ee0b1a50)

```
TON = 3 x 10 x 10^-6
    = 0.00003
TOFF=0.00003
TOTAL TIME = TON + TOFF
           = 0.00003+0.00003 
           = 0.00006
FREQUENCY = 1/(TOTAL TIME) 
          =1/0.00006 
          = 16666.7
DUTY CYCLE = TON /(TON+TOFF)
           = 0.00003/0.00006
           = 0.5
      IN % =0.5*100 
           = 50 %
```
![Screenshot 2025-05-09 132857](https://github.com/user-attachments/assets/76d80b5a-3688-4e50-8fc5-d3a1cbe7831b)

```
TON = 4 x 10 x 10^-6
    = 0.00004
TOFF= 2 x 10 x 10^-6
    = 0.00002
TOTAL TIME = TON + TOFF
           = 0.00004+0.00002
           = 0.00006
FREQUENCY = 1/(TOTAL TIME)
          = 16666.7
DUTY CYCLE = TON /(TON+TOFF)
           = 0.00004/0.00006
           = 0.7
      IN % =0.7*100 
           = 70 %
```
![Screenshot 2025-05-09 132929](https://github.com/user-attachments/assets/4bfa2b32-98bb-426b-8403-96c5cb016ef2)

```
TON = 1 x 50 x 10^-6
    = 0.00005
TOFF= 0.1 x 50 x 10^-6
    = 0.000005
TOTAL TIME = TON + TOFF
           = 0.00005 + 0.000005
           = 0.000055
FREQUENCY = 1/(TOTAL TIME)
          = 18181.82
DUTY CYCLE = TON /(TON+TOFF)
           = 0.00005/0.000055
           = 0.9
      IN % =0.9*100 
           = 90 %
```
# Result :
A PWM Signal is generated using the following frequency and various duty cycles are simulated
