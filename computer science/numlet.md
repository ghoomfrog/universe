# Numlet

A numlet is an object that can sequentially form an arbitrary-precision number. A numlet is structured as follows:

Field     |Size|Description
----------|----|-----------
More      |1b  |whether to expect more numlets
Fractional|1b  |whether the fragment is fractional: part of the fractional part[^integer-part]
Fragment  |30b |integer fragments, [combined](#combining-fragments) together, form the integer part, and vice versa[^vice-versa]

Both parts default to 0.

## Combining Fragments

Parts are obtained using the following formula:

$$(\sum_{f=f_0}^{f_{-2}} 2^{30}f) + f_{–1}$$

where $f_0$ is the first fragment of the part, $f_{-1}$ is the last, and $f_{–2}$ is the second last.

[^integer-part]: otherwise part of the integer part
[^vice-versa]: fractional fragments, concatenated together, form the fractional part
