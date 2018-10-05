# Software PWM

Description: Software LED PWM - uses TimerA0 interrupt with multiple capture control registers to increase an LEDs brightness by 10% everytime the button is pressed

After 100% brightness the LED goes to 0% brightness and continues to increase by 10% per button press

Button debounced using timer1 A0

# Code description:

The code begins by stopping the watchdog timer and setting LED pins to outputs, turning one on initial.

Timer0 A0 is then set up. CCR1 is set to 1000 (max count) and CCR0 and CCR2 are set to 500 (50% of max). The Timer interrupt is enabledand the timer is started with SM clock (1MHz) in upmode.

Next timer1 A0 is set up. This timers purpose is to debounce the button press so the button works more smoothly. CCR0 is set to 25000 and the timer interrupt is enabled.

The button interrupt is then set up enabling its pull up resistor, its interrupt on depress, and clearing the pending flag.

The processor is then put into low power mode 0 with general interrupts enabled.

For the button press interrupt, Timer A1 is started using SM clock (1MHz) in upmode with a /8 divider. Then the interrupt pending flag is cleared.

For Timer0 A0 interrupt, if CCR2 hold the value 0, then the led is just forced off and all other code is skipped. Otherwise, if CCR0 is not equal to the max count value, it sets ccr0 to the max and turns off the LED. If CCR0 is set to the max, it sets CCR0 back to its previous value (stored in CCR2) and turns the LED on.

For Timer1 A0 interrupt, a different LED is toggled as a means to debug, making sure the button press with debounce is working. Next, if CCR2 is less than the max, it is incremented by 10% of the max. If CCR2 reaches the max value and is incremented again, it is set to 0% of the max value, and the process will continue from there. After this, Timer1 A0 is stopped and clear for the button debouncing.
