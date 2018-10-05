# Hardware PWM

Description: Hardware LED PWM - This program uses built in MSP430 hardware to produce a PWM signal to an LED output.
LED Pin is selected as PWM output, OUTMOD 7 (reset/set) selected to use CCR0 and CCR1 to create PWM.
When timer reaches CCR1, the LED is turned off, when it reaches CCR0, the LED is turned back on (CCR1 < CCR0).
The purpose is to increase an LEDs brightness by 10% every time the button is pressed.
When button is pressed CCR1 is increased, shortening the time the LED is off, making it appear brighter.
After 100% brightness the LED goes to 0% brightness and continues to increase by 10% per button press.
Button de-bounced using timerA1.

# Code Description

The code begines by stopping the watchdog timer.

Next, the PWM and timer are set up. The LED pins are set to outputs, P1SEL for the LED pin we want the PWM to putput to is set to 1. This makes  PWM signal to output directly to the led pin through harware (rather than software). CCR0 is set to 1000 so it is run at 1kHz. CCR1 is set to 500 so that the PWM signal begins with a 50% duty cycle. CCTL1 is set to OUTMOD_7, meaning when the timer reaches CCR1, it will toggle the pin to 0, and when it reaches CCR0, it will set the pin to 1. The clock is then started using SM clock (1MHz) in upmode.

The button interrupt is set up. P1OUT is set to the button pin. The pull up resistor is enabled for the button pin. The button interrupt is enabled for the button when the button is depressed. Finally, the interrupt pending flag is cleared.

Timer1 A0 is then set up, and used to debounce the button. TA1CCR0 is set to 25000, and the timer interrupt is enabled.

The processor is then put into low power mode 0 with general interrupts enabled.

For the button interrupt function, the timer1A0 is started using SM clock in upmode with a /8 divider. Then the interrupt pending flag is cleared. 

For the Timer1 A0 interrupt the code to increase the LED brightness runs. If CCR1 is less than 1000 (the max), it is incremented by 100, or 10% of the LEDs brightness. If CCR1 reaches 1000, it is reset to 0, turning the LED off. From there the process repeats. At the end of the interrupt, the timer is stopped and cleared until the button is pressed again.



