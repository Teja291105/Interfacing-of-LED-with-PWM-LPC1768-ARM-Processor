# NAME: JANANI V
# REG NO: 212223060099
# Interfacing-of-LED-with-PWM-LPC1768-ARM-Processor

**AIM:**

To write an embedded C program to interface an LED and PWM with ARM processor LPC1768.

**COMPONENTS REQUIRED:**

**Hardware**

ARM LPC1768 board

LED

DSO (Digital Storage Oscilloscope)

**Software**

Keil µVision 4 IDE (Keil MDK-ARM)

**PROCEDURE** 

1. Open Keil µVision and choose Project → New µVision Project....

2. Browse to your project folder, enter a project name and click Save.

3. In the Select Device for Target dialog, choose the device NXP: LPC1768 and click OK.

4. Keil will prompt to add startup code for LPC17xx. Click Yes to include the LPC17xx startup file.

5. Create a new source file: File → New. Write your C program (main.c).

6. Save the source file (e.g., main.c) into the project folder.

7. Right-click the Target in the Project window and Add New Group / Add Existing Files to Group 'Source Group 1'. Add main.c and other C sources (delay.c, gpio.c, pwm.c, system_LPC17xx.c, and the startup file Startuplpc17xx.s).

8. Add required header files to the project (e.g., delay.h, gpio.h, pwm.h, stdutils.h) in the Header folder or keep them in the project directory and include their paths.

9. Configure Include Paths: Project → Options for Target → C/C++ → Include Paths. Add any folders needed (for example ..\inc or C:\Users\<you>\Desktop\00-libfiles) so the compiler finds your headers.

10. Build the project: click Build (or Project → Build Target). Fix any compiler errors/warnings and rebuild until compilation succeeds.

11. To generate a .bin file, open Target Options (right-click Target → Options for Target) and ensure the memory map/ROM start is set correctly for your application.

12. Set IROM1 start address to 0x2000 because the bootloader occupies 0x0000–0x1FFF. User application should start from 0x2000.

13. After a successful build you will have an .axf (ELF) file in the project output folder. Use the Keil utility fromelf to generate the binary from the ELF file:

       fromelf --bin projectname.axf --output projectname.bin
    
14. Rebuild if necessary; verify that projectname.bin is generated in the project folder.
    
15. Program/flash the .bin to the LPC1768 using your preferred flash tool or debugger.
    
16. Test the board: verify LED toggling and PWM output using the DSO.

**FILES TO ADD**

Target1:

Source Group 1:

Startuplpc17xx.s

delay.c

gpio.c

pwm.c

system_LPC17xx.c

main.c

Header files:

delay.h

gpio.h

pwm.h

stdutils.h

**PIN DIAGRAM:**

<img width="874" height="484" alt="image" src="https://github.com/user-attachments/assets/53f7309c-6e86-475e-b890-98322be1e9cc" />

**CIRCUIT DIAGRAM:**

<img width="891" height="442" alt="image" src="https://github.com/user-attachments/assets/7a1acbba-450d-48eb-92ec-0d21b909eabd" />


**PVM OUTPUT:**

<img width="734" height="216" alt="image" src="https://github.com/user-attachments/assets/81c9ae19-18a3-4498-8c33-c6107300587c" />

**CODE:**
```
#include <lpc17xx.h>
#include "pwm.h"
#include "delay.h"

#define CYCLE_TIME 100  /* PWM period resolution (Ton + Toff) */

int main(void)
{
    int dutyCycle;

    SystemInit();                  /* Clock and PLL configuration */

    /* Initialize PWM module with cycle time */
    PWM_Init(CYCLE_TIME);

    /* Enable PWM output on PWM_3 (adjust constant if your pwm.h uses a different name) */
    PWM_Start(PWM_3);

    while (1)
    {
        /* Increase LED brightness (0 -> CYCLE_TIME) */
        for (dutyCycle = 0; dutyCycle <= CYCLE_TIME; dutyCycle++)
        {
            PWM_SetDutyCycle(PWM_3, dutyCycle);
            DELAY_ms(10);          /* small delay to make ramp visible */
        }

        /* Decrease LED brightness (CYCLE_TIME -> 0) */
        for (dutyCycle = CYCLE_TIME; dutyCycle >= 0; dutyCycle--)
        {
            PWM_SetDutyCycle(PWM_3, dutyCycle);
            DELAY_ms(10);
        }
    }

    /* never reached, but good practice */
    return 0;
}
```

**OUTPUT:**

<img width="710" height="539" alt="image" src="https://github.com/user-attachments/assets/9e89510e-64d1-4bc9-8f44-727a6bcd63f5" />

**RESULT:**

Thus, interfacing the LED and PWM with the ARM processor LPC1768 is done and the outputs are verified.
