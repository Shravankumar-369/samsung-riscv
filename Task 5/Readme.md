    1 .COUNTER

OVERVIEW:

A sequence counter is a specialized digital counter that follows a predefined sequence rather than a standard binary or decimal count. It is commonly used in embedded systems, digital electronics, and automation applications where a specific counting pattern is required.
The Squadron Mini Board, powered by a RISC-V processor, provides a flexible platform for implementing such counters using software and hardware approaches.
This project involves designing and implementing a sequence counter on the Squadron Mini Board, ensuring the counter follows a specific non-linear sequence (e.g., {0, 3, 5, 2, 7, 1}) using GPIO output to visualize the counting pattern. 

COMPONENTS REQURIED:

VSD Squadron Mini Board: RISC-V-based development kit
Push Button 
LEDs for visualisation of count
Breadboard
Jumper Wires
Resistors (330 Ohm)
VS Code with PlatformIO: For writing and Uploading code.

CIRCUIT DIAGRAM:

![WhatsApp Image 2025-02-17 at 21 39 56_3533c81c](https://github.com/user-attachments/assets/dab53c66-1258-4fa6-b8bc-fce7c8614841)


CODE:

    #include "ch32v00x.h"

    void GPIO_Config(void) {
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE); // Enable GPIOC clock
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE); // Enable GPIOD clock

    GPIO_InitTypeDef GPIO_InitStructure = {0};

    // Configure LEDs (PC0 - PC4) as outputs
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2 | GPIO_Pin_3 | GPIO_Pin_4;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);

    // Configure Button (PD5) as input with pull-up resistor
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
    GPIO_Init(GPIOD, &GPIO_InitStructure);
    }

     // Debounce Delay Function
    void delay_ms(uint32_t ms) {
    for (volatile uint32_t i = 1000 * ms; i > 0; i--); // Simple delay
    }

    int main() {
    uint8_t bit_pattern = 0b00001; // Start with first LED ON
    SystemCoreClockUpdate();
    GPIO_Config();

    while (1) {
        GPIO_Write(GPIOC, bit_pattern); // Update LEDs

        if (GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_5) == 0) { // Button pressed (active low)
            delay_ms(200); // Debounce delay
            
            if (bit_pattern < 0b11111) {  // Shift left only if not full
                bit_pattern = (bit_pattern << 1) | 0b00001; // Add 1 at LSB
            } else {
                bit_pattern = 0b00001; // Reset if all bits are ON
            }

            while (GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_5) == 0); // Wait until button is released
        }
    }
    }
    
CONCLUSION:
Implementing a sequence counter on the RISC-V Squadron Mini Board demonstrates a practical approach to embedded systems design, combining software and hardware techniques. By leveraging the RISC-V architecture, the counter follows a predefined non-linear sequence and outputs values through GPIO, making it useful for various digital applications.

This project highlights:

✅ Embedded Programming – Writing C and RISC-V assembly to control hardware.

✅ Digital Design – Implementing a finite state machine (FSM) for a sequence counter.

✅ Hardware-Software Integration – Using GPIO for real-world interfacing.





    2. DIGITAL VOTING MACHINE

OVERVIEW :

RISC-V (Reduced Instruction Set Computing - Version 5) is an open-source instruction set architecture (ISA) that offers: Transparency: Open-source nature allows for full auditability of hardware and software. Security: Custom security features can be implemented at the hardware level. Customization: Ability to develop purpose-built processors optimized for secure and efficient voting processes. Cost-Effectiveness: Open-source design eliminates licensing fees, making voting machines more affordable.A Digital Voting Machine (DVM) is an electronic device used to record, store, and count votes in an election. It replaces traditional paper ballots with a computerized system to enhance efficiency, accuracy, and security.

COMPONENTS REQURIED:

VSD Squadron Mini Board: RISC-V-based development kit
Push Buttons for voting 
LEDs for vote indication 
Buzzer for the confirmation of the vote 
I2C LCD Display (16x2) 
I2C EEPROM for the vote storage
Breadboard 
Jumper Wires 
Resistors (330 Ohm and 1k Ohm) 
VS Code with PlatformIO: For writing and Uploading code.

CIRCUIT DIAGRAM:

![WhatsApp Image 2025-02-13 at 22 33 20_4682fce5](https://github.com/user-attachments/assets/9f827997-c378-4da5-8c49-61f00772a841)

CODE:

    // Digital Voting Machine Code for CH32V003 Microcontroller

    #include <stdio.h>
    #include <ch32v00x.h>
    #include "i2c_lcd.h"  // Include necessary libraries for LCD
    #include "i2c_eeprom.h" // Include EEPROM handling functions

    // GPIO Configuration
    void GPIO_Config(void) {
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);

    // Push Button Inputs (PD0, PD1, PD2)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;  // Input with pull-up
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // LED Outputs (PC6, PC7, PD4)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6 | GPIO_Pin_7;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOC, &GPIO_InitStructure);

    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_4;
    GPIO_Init(GPIOD, &GPIO_InitStructure);

    // Buzzer Output (PC5)
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_5;
    GPIO_Init(GPIOC, &GPIO_InitStructure);
    }

    // Vote Counting and Display
    void processVote(uint8_t candidate) {
    uint8_t votes;
    EEPROM_ReadByte(candidate, &votes);
    votes++;
    EEPROM_WriteByte(candidate, votes);

    LCD_Clear();
    LCD_SetCursor(0, 0);
    LCD_Print("Candidate ");
    LCD_PrintNum(candidate + 1);
    LCD_SetCursor(1, 0);
    LCD_Print("Votes: ");
    LCD_PrintNum(votes);

    // LED Indication
    if (candidate == 0) GPIO_WriteBit(GPIOC, GPIO_Pin_6, RESET);
    if (candidate == 1) GPIO_WriteBit(GPIOC, GPIO_Pin_7, RESET);
    if (candidate == 2) GPIO_WriteBit(GPIOD, GPIO_Pin_4, RESET);

    // Buzzer Alert
    GPIO_WriteBit(GPIOC, GPIO_Pin_5, RESET);
    Delay_Ms(200);
    GPIO_WriteBit(GPIOC, GPIO_Pin_5, SET);
    }

    int main(void) {
    NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);
    SystemCoreClockUpdate();
    Delay_Init();
    GPIO_Config();
    I2C_LCD_Init();
    I2C_EEPROM_Init();

    LCD_Print("Digital Voting");
    Delay_Ms(1000);
    LCD_Clear();

    while (1) {
        if (GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_0) == 0) processVote(0);
        if (GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_1) == 0) processVote(1);
        if (GPIO_ReadInputDataBit(GPIOD, GPIO_Pin_2) == 0) processVote(2);

        GPIO_WriteBit(GPIOC, GPIO_Pin_6, SET);
        GPIO_WriteBit(GPIOC, GPIO_Pin_7, SET);
        GPIO_WriteBit(GPIOD, GPIO_Pin_4, SET);
    }
    }

CONCLUSION:

Digital Voting Machines provide a modern, efficient, and secure method for conducting elections. However, their reliability depends on strong cybersecurity measures, transparency, and voter trust. Future innovations, such as blockchain and AI-driven verification, could further enhance the integrity of digital voting systems.


