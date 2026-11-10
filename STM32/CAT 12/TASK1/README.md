# CAT12 – Task 12A – On-board LED Bring-Up

| Field | Details |
|-------|---------|
| EXP | CAT12 |
| DATE | 14/10/2026 |
| NAME OF EXP | STM32F401RE Cortex-M4: GPIO and User Input Programming |
| HARDWARE SETUP / PLATFORM | STM32 NUCLEO-F401RE (STM32F401RET6, ARM Cortex-M4, up to 84 MHz), on-board ST-LINK/V2-1, STM32CubeMX + Keil MDK-ARM (HAL) |
| TASK NO | Task 12A – On-board LED Bring-Up |

## 1. Objective

Develop and program an STM32 application that verifies the NUCLEO-F401RE hardware and GPIO output configuration by blinking the on-board LED at a fixed interval.

## 2. Circuit Diagram

![Circuit Diagram](circuit.png)

**Pin Connections:**

| Signal / Component | MCU Pin | Description |
|--------------------|---------|-------------|
| Power / programming | USB (ST-LINK) | Board supply and debug link over the on-board ST-LINK |
| System clock | HSE (8 MHz from ST-LINK) or HSI → PLL | Configured in CubeMX |
| LED (LD2) | PA5 | Output (on-board user LED, active-HIGH) |

## 3. Core Logic / Pseudocode / Code Base

**Working Principle:**

In CubeMX, PA5 is configured as `GPIO_Output`. Toggling the pin at a fixed interval makes the LED blink, which confirms that the toolchain, clock, programming and GPIO output are all working.

**Pseudocode:**

1. Initialise HAL, system clock and GPIO (`HAL_Init()`, `SystemClock_Config()`, `MX_GPIO_Init()`).
2. Toggle the LED with `HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5)`.
3. Wait with `HAL_Delay(500)`.
4. Repeat continuously.

**Code Base:**

`HAL_GPIO_TogglePin()` flips PA5 and `HAL_Delay()` sets the blink interval, inside `while(1)`.

## 4. Inference

**Observation:**

The on-board LED blinked at a steady interval (ON for 500 ms, OFF for 500 ms).

**Conclusion:**

A working blink confirms that the NUCLEO-F401RE, the ST-LINK programming path, the clock configuration and the GPIO output setup are correct.
