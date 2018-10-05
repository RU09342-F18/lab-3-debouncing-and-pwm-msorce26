# Software PWM

Description: Software LED PWM - uses TimerA0 interrupt with multiple capture control registers to increase an LEDs brightness by 10% everytime the button is pressed

After 100% brightness the LED goes to 0% brightness and continues to increase by 10% per button press

Button debounced using timerA1

# Code description:

The code begins by stopping the watchdog timer and setting LED pins to outputs, turning one on initial.

Timer A0 is then set up. CCR1 is set to 1000 (max count) and CCR0 and CCR2 are set to 500 (50% of max). The Timer interrupt is enabledand the timer is started with SM clock (1MHz) in upmode.

Next timer A1 is set up. This timers purpose is to debounce the button press so the button works more smoothly. CCR0 is set to 25000 and the timer interrupt is enabled.

The button interrupt is then set up enabling its pull up resistor, its interrupt on depress, and clearing the pending flag.

The processor is then put into low power mode 0 with general interrupts enabled.

For the button press interrupt, Timer A1 is started using SM clock (1MHz) in upmode with a /8 divider. Then the interrupt pending flag is cleared.


