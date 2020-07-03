---
layout: post
title:  "Introducing Ports"
date:   2020-05-20 19:14:04 +0530
categories: jekyll update
author: "Ashwin"
---
# Learning about PORTS!

The AtMega328 comes with 40 pins, out of these 32 are used for input and output purposes. Now, these 32 pins are divided into 4 groups called PORTS, and each PORT has 8 pins which it can control.

Now, there are 3, 8-bit I/O registers which are used to manipulate the aforementioned ports. Namely - DDRx, PINx and PORTx

***DDRx***

DDRx stands for Data Direction Register, the purpose of this register is to make a given port either input or output. To make a port an output we set DDRx as 1. To make it input we set it as 0.

***PINx***

PINx stands for **P**ort **IN**put pins. This register should be used only to deal with reading the data from pins.

***PORTx***

PORTx, sadly this is the only one which does not stand for anything cool. Port is used to send data out to the pins. If PORTx is set to 1, the value it sets out is 1 ( Or rather more accurately - LOGIC HIGH ). 

**NOTE:** We will only be able to read inuput from pins using PIN if DDR is set to 0. Also we will be able to send out Logic High/Low to pins only if DDR is set to 1.

**NOTE Again:** We can also manipulate individual bit/register/pin using Bit Maths.


# Some nitty-gritty details

***Port A***

Port A besides having the functionality of I/O, also has the ability to work as Analog to Digital converter (ADC). Since there is no other port on the AtMega328 which provides us this functionality, we often avoid using Port A for simple I/O.

***Ports B, C and D***

Similar to Port A, each of the remaining port also provides a variety of alternate functionality.


*Author - amm0*

Check out the book by [Mazidi - AVR Microcontroller embedded programming][Mazidi book], for more info. Also the bread and butter of every firmware engineer, [AtMega328 datasheet][Atmega datasheet]. 
If you have questions or suggestions, you can reach out to me via [email][mail-id] or on [facebook][facebook page].

[Mazidi book]: https://www.pearson.com/us/higher-education/program/Mazidi-AVR-Microcontroller-and-Embedded-Systems-Using-Assembly-and-C/PGM171768.html
[Atmega datasheet]: https://www.microchip.com/wwwproducts/en/ATmega328
[facebook page]: www.facebook.com/ashwin.olakangal
[mail-id]: f20180544@hyderabad.bits-pilani.ac.in
