# Numlet-Based Arbitrary-Precision Number

A numlet-based arbitrary-precision number constitutes of a sequence of <ins>numlets</ins>:

Field     |Size|Description
----------|----|-----------
More      |1b  |whether to expect more numlets
Fractional|1b  |whether this numlet is part of the fractional part[^integer-part]
Fragment  |30b |integer fragments concatenated together form the integer part, and vice versa[^vice-versa]

Both parts default to 0.

[^integer-part]: otherwise part of the integer part
[^vice-versa]: fractional fragments concatenated together form the fractional part
