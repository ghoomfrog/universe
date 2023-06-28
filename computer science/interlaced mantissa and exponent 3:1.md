# Interlaced Mantissa and Exponent 3:1

Interlaced Mantissa and Exponent (IME) 3:1 is a digital rational number format based on standard floating point, where the size ratio between the mantissa and exponent is 3:1. It's designed to be convertible between bit sizes with no arithmetic, logical or bitwise operations.

The format is a sequence of contiguous chunks composed of 3 mantissa bits followed by 1 exponent bit. The mantissa is the concatenated mantissa bits. Same thing for the exponent. Missing or excluded bits default to 0.

The mantissa and the exponent can independently be signed. The signs are the least significant bits of their respective parts. A sign of 0 denotes negativity for both parts: it's like two's complement but with reversed sign roles.

Here's a comparison of a signed number formatted in both 64-bit floating-point and 64-bit IME 3:1. Bold bits are part of the exponent, and italicized ones are signs. The least significant bits are to the right.

Format|Value
------|-----
64-bit Floating point|*0*​***1*1111011011**0000001011110000100010111100010101110001001001000010
64-bit IME 3:1|**0**010**0**111**0**100**0**001**0**000**1**101**1**111**1**000**1**101**0**011**1**100**1**010**0**010**1**010**1**000***1***10*1*

Notice the reversed sign roles only between the mantissas of floating point and IME 3:1, because floating point uses two's complement for the mantissa but not for the exponent.
