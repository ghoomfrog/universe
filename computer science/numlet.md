# Numlet

A numlet is an object that can sequentially form an arbitrary-precision number. A numlet is structured as follows:

Field     |Size|Description
----------|----|-----------
More      |1b  |Whether to expect more numlets
Fractional|1b  |<ol start="0"><li>The fragment is part of the integer part.<li>The fragment is part of the fractional part.</ol>
Fragment  |30b |Integer fragments, [combined](#combining-fragments) together, form the integer part. Fractional fragments, combined together, form the fractional part.

Both parts default to 0.

## Combining Fragments

Parts are obtained using the following formula:

$$(\sum_{f=f_0}^{f_{-2}} 2^{30}f) + f_{–1}$$

where $f_0$ is the first fragment of the part, $f_{-1}$ is the last, and $f_{–2}$ is the second last.

$f_{–1}$ defaults to $f_{0}$, and $f_{–2}$ defaults to $0$.
