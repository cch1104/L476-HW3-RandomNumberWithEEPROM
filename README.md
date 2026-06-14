# L476-HW3-RandomNumberWithEEPROM
# STM32 EEPROM Random Number Generator (I2C + LCD)

## Overview
This project is based on STM32 Nucleo-L476 and demonstrates:
- ADC-based entropy generation (random seed)
- Pseudo-random number generation using rand()
- EEPROM read/write via I2C
- LCD display output
- Data verification between EEPROM write/read
- Scrolling ticker display

## Hardware Requirements
- STM32 Nucleo-L476RG
- I2C EEPROM (e.g., 24LC256)
- 16x2 LCD (I2C or parallel)
- ADC input noise source (potentiometer recommended)

## Features
- Generates 4-digit random number
- Stores value in EEPROM at 0x1000
- Reads back and verifies integrity
- LCD status display
- Scrolling ticker animation

## Main Flow
1. Init HAL, GPIO, I2C, ADC, LCD
2. Generate entropy (ADC + HAL_GetTick)
3. srand(seed)
4. randomNum = rand() % 10000
5. sprintf -> string
6. WRITE EEPROM
7. READ EEPROM
8. Compare results
9. Display OK / FAIL

## Key Functions

### WRITE EEPROM
```c
void WRITE(uint16_t MemLoc, uint8_t *pData, uint16_t len);
```

### READ EEPROM
```c
void READ(uint16_t MemLoc, uint8_t *pData, uint16_t len);
```

## Important Fixes

### FIX 1: Wrong strlen usage
❌ Wrong:
READ(..., strlen(rmsg)+1);

✔ Correct:
READ(0x1000, (uint8_t*)rmsg, 10);

---

### FIX 2: EEPROM address
Use:
#define I2C_ADDRESS (0x50 << 1)

---

## Expected Output
- randomNum: 1234
- EEPROM OK
- scrolling display of number
