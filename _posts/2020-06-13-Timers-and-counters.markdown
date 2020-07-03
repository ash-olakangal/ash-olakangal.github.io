---
layout: post
title:  "Timers and Counters"
date:   2020-06-13 1:10:04 +0530
categories: jekyll update
author: "Ashwin"
---
We need to use counter registers to count an event or to generate time delays. When we want to count an event, we connect the source of that event to the clock pin of the counter register and each time that event takes place the counter increments by one.

When we want to generate time delays, we connect the clock-source (oscillator) to the clock pin of the counter register and each time the clock will tick, the counter will be incremented.

So, we can use the counter register to create time delays, by counting and waiting for the desired number of ticks in a clock.

# Let's start with the most vanilla task - Blinking an LED 

Albeit of a different kind. Here we have been given a situation of the following order - There are leds connected to each pin of the PORTB, our task is to blink the leds such that each value between 00000000-11111111 (0-255) is shown on the leds.

```c

#include <avr/io.h> // standard avr header

int main()
{

    DDRB = 0xFF; // setting up all the pins of PORTB as output pins

    PORTB = 0x00; 

    while(1)
    {
        PORTB += 1;
    }

    return 0;
}
```

Explaination - As explained in the previous post we need to set the value of DDRx in order to send data to PORTx. In the next line we initialize the value of PORTB as 0x00, while this is not necessary as all the ports are set to zero by default on resetting, still it is a good practice. Next we enter in the while loop where we increment the current value of PORTB by one. Once it reaches 255, it will overflow back to 0 and this cycle will continue.

# Simple enough? Let's look at something a little more advanced...

We will now attach a single led to the **3rd Pin of Portb** and a push button to **5th pin of PortC**. And you guessed it right, we will now toggle an led using the push button.

But wait before you rant about the elementary nature of this task, you need to understand the fact that this time we need to only manipulate a single pin instead of working with the whole port. And in order to do that we need to [learn about bit manipulation in c](https://www.tutorialspoint.com/ansi_c/c_bits_manipulation.htm).

Now that we have got that out of the way, lets look at what the code should be - 

```c

#include <avr/io.h>

#define ledpin_portB 2  // note that this is not 3
#define button_portC 4  // and neither is this 5

int main()
{
    DDRB |= 1<<ledpin_portB;
    DDRC &= 0<<button_portC;

    while(1)
    {
        if (PINC & 1<<button_portC)
        {
            PORTB |= 1<<ledpin_portB; 
        }
        else
        {
            PORTB &= 0<<ledpin_portB;
        }
    }

}
```

The reason we have taken ledpin_portB as 2 and button_portC as 4 is because after the bits will be shifted, the position of 1 and 0 respectively will be 3rd pin(or 2nd bit) and 5th pin(or 4th bit). Basically, there is a difference between the Pin(1-8) and bits that can be used(0-7). So, when I say first pin, I mean the zero bit.

The rest of the code has purposely been left uncommented, so that you can test your understanding.

# Final exercise - LCD interfacing

The data pins of LCD are connected to PORTB and the enable pin is connected to the **6th pin of PORTC**, the LCD will display a message only when the enable pin goes from high to low. 

```c
#include <avr/io.h>

#define lcd_enable 5

int main()
{
    
    DDRB = 0xFF; // Setting PORTB as output

    DDRC |= 1<<lcd_enable; // Setting 6th pin (or 5th bit) as output

    unsigned char message[] = "Congrats!"

    sizeofmessage = sizeof(message)/sizeof(message[0]);

    for (uint8_t i = 0; i < sizeofmessage; ++i)
    {
        PORTB = message[i];
        
        PORTC |= 1<<lcd_enable; // setting the enable pin to high
        PORTC &= 0<<lcd_enable; // setting the enable pin to low
    } 

    return 0;
}
```

Same as the earlier program we are setting the 6th pin (1-8 pins) or the 5th bit(0-7 bits) as output in portC, each time we enter the For loop, we set the value of enable pin from high to low and output a character from our message.

It might seem difficult and intimidating at first, but no good thing comes without a cost. Here the cost is your dedication and perseverence.

*Author - amm0*

Check out the book by [Mazidi - AVR Microcontroller embedded programming][Mazidi book], for more info. Also the bread and butter of every firmware engineer, [AtMega328 datasheet][Atmega datasheet]. 
If you have questions or suggestions, you can reach out to me via [email][mail-id] or on [facebook][facebook page].

[Mazidi book]: https://www.pearson.com/us/higher-education/program/Mazidi-AVR-Microcontroller-and-Embedded-Systems-Using-Assembly-and-C/PGM171768.html
[Atmega datasheet]: https://www.microchip.com/wwwproducts/en/ATmega328
[facebook page]: www.facebook.com/ashwin.olakangal
[mail-id]: f20180544@hyderabad.bits-pilani.ac.in
