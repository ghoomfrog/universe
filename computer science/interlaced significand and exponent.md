# Interlaced Significand & Exponent

Interlaced Significand & Exponent (ISE) is a rational number format based on [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) (floating point), designed to be resizable with no special computation. All values are valid by default.

The format is a sequence of contiguous chunks composed of 3 significand bits followed by 1 exponent bit. The significand is the reversed concatenated significand bits, and the exponent is the unreversed concatenated exponent bits. All bits beyond the number default to 0.

When the significand is 0, the exponent can be used as non-standard metadata. This is similar to floating-point NaNs, except that the number is valid and should be treated as 0. The exponent shouldn't be considered metadata when it's also 0.

The maximum positive number and minimum negative number can optionally be treated as positive and negative infinity.

The mantissa is $0.s$ where $s$ is the significand.

The significand and the exponent can independently be signed. The signs are the least significant bits of their respective parts. A sign of 0 denotes positivity.

Here's a comparison between the same, fully signed number (with both a signed significand and exponent) in 64-bit IEEE 754 and 64-bit ISE. **Bold** bits are part of the exponent, and *italicized* ones are signs. The least significant bits are to the right.

Format         |Value
---------------|-----
64-bit IEEE 754|*0* ​***1* 1 1 1 1 0 1 1 0 1 1** 0 0 0 0 0 0 1 0 1 1 1 1 0 0 0 0 1 0 0 0 1 0 1 1 1 1 0 0 0 1 0 1 0 1 1 1 0 0 0 1 0 0 1 0 0 1 0 0 0 0 1 0
64-bit ISE     |**0** 0 1 0 **0** 0 0 0 **0** 1 0 0 **0** 1 0 0 **0** 1 0 0 **1** 1 1 0 **1** 1 0 1 **1** 0 1 0 **1** 0 0 1 **0** 1 1 1 **1** 0 1 0 **1** 0 0 1 **0** 0 0 0 **1** 0 1 1 **1** 1 1 0 ***1*** 1 0 *0*
