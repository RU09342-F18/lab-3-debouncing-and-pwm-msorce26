# Software Debouncing

The purpose of this program is to toggle an LED using button press enabled interrupts, making use of a built in timer to de-bounce the button. When a button is pressed, it is typical for it to "bounce", or trigger the interrupt action multiple times. This program used tha timer to create a 150ms delay after the button is pressed, so that the action is not done more than desired.

The Program begins by stopping the watchdog timer. Timer A0 is set up, enabling its interrupt, setting its capture compare register to 18750, and finally beginning the timer using SM Clock(1MHz) in upmode, with a /8 delay.

Next, the button interrupt in enabled. LED pins are set to outputs and button pins set to inputs. Pull up resistor is enabled on the button, and the interrupt is enabled for a button depress, then the pending flag is cleared.

The Processor is then put into low powermode 0 with general interrupts enabled.

In the button interrupt function, all it does is start the previously set up timer, then clear the interrupt pending flag.

In the timer A0 function, the led is toggled, then the Timer stops itself and clears the timer.

This allows the LED to only toggle once per button press.
