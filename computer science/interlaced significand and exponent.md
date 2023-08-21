# Interlaced Significand & Exponent

Interlaced Significand & Exponent (ISE) is a rational number format based on [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) (floating point), designed to be resizable with no additional computation. Though, the format doesn't inherit infinities, and all values are valid.

The format is a sequence of contiguous chunks composed of 3 significand bits followed by 1 exponent bit. The significand is the reversed concatenated significand bits, and the exponent is the unreversed concatenated exponent bits. All bits beyond the number default to 0.

When the significand is 0, the exponent can be used as non-standard metadata. This is similar to floating-point NaNs, except that the number is valid and should be treated as 0.

The mantissa is $0.s$ where $s$ is the significand.

The significand and the exponent can independently be signed. The signs are the least significant bits of their respective parts. A sign of 0 denotes positivity.

Here's a comparison of a fully signed number (with both a signed significand and exponent) formatted in both 64-bit floating-point and 64-bit ISE. **Bold** bits are part of the exponent, and *italicized* ones are signs. The least significant bits are to the right.

Format               |Value
---------------------|-----
64-bit floating point|*0* ​***1* 1 1 1 1 0 1 1 0 1 1** 0 0 0 0 0 0 1 0 1 1 1 1 0 0 0 0 1 0 0 0 1 0 1 1 1 1 0 0 0 1 0 1 0 1 1 1 0 0 0 1 0 0 1 0 0 1 0 0 0 0 1 0
64-bit ISE           |**0** 0 1 0 **0** 0 0 0 **0** 1 0 0 **0** 1 0 0 **0** 1 0 0 **1** 1 1 0 **1** 1 0 1 **1** 0 1 0 **1** 0 0 1 **0** 1 1 1 **1** 0 1 0 **1** 0 0 1 **0** 0 0 0 **1** 0 1 1 **1** 1 1 0 ***1*** 1 0 *0*
