# Floating Point

* IEEE 754 standard



#### Q1. What decimal number does the bit pattern 0x0C000000 represent if it is a floating point number? Use the IEEE 754 standard.

> Answer
>
> 0x0C000000 = 0000 1100 0000 0000 0000 0000 0000 0000 
>
> = 0 00011000 0000 0000 0000 0000 0000 000
>
> sign is positive
>
> exp = 0x18 = 24 - 127 = -103
>
> there is a hidden 1
>
> mantissa(가수부) = 0
>
> -> answer = 1.0 * 2^(-103)
