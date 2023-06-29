# Interlaced Mantissa and Exponent

Interlaced Mantissa and Exponent (IME) is a rational number format based on standard floating point, designed to be resizable with no additional computation. Though, the format doesn't inherit NaNs and infinity. All values are arithmetic.

The format is a sequence of contiguous chunks composed of 3 mantissa bits followed by 1 exponent bit. The mantissa is the reversed concatenated mantissa bits, and the exponent is the unreversed concatenated exponent bits. All bits beyond the number default to 0.

The mantissa is obtained as 0.*m* (where *m* is the mantissa).

The mantissa and the exponent can independently be signed. The signs are the least significant bits of their respective parts. A sign of 0 denotes positivity.

Here's a comparison of a fully signed number (with both a signed mantissa and exponent) formatted in both 64-bit floating-point and 64-bit IME. Bold bits are part of the exponent, and italicized ones are signs. The least significant bits are to the right.

Format|Value
------|-----
64-bit Floating point|*0*​***1*1111011011**0000001011110000100010111100010101110001001001000010
64-bit IME|**0**010**0**000**0**100**0**100**0**100**1**110**1**101**1**010**1**001**0**111**1**010**1**001**0**000**1**011**1**110***1***10*1*
