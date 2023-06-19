# Numlet

A numlet is an object that can form an rational arbitrary-precision number with other sequential numlets. A numlet is structured as follows:

Field     |Size         |Description
----------|-------------|-----------
More      |1b           |Whether to expect more numlets
Fractional|1b           |<ol start="0"><li>The fragment is part of the integer part.<li>The fragment is part of the fractional part.</ol>
Fragment  |$8k - 3$ bits|Fragments are [chains](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) where the first chainlet is $8k - 3$ bits, and consequent chainlets are $8k$ bits. Fragments, [combined](#combining-fragments) together, form their respective part.<br>$k$ is any natural number.

Both parts default to 0.

## Combining Fragments

Parts are obtained using the following formula:

$$(\sum_{f=f_0}^{f_{-2}} 2^{30}f) + f_{–1}$$

where $f_0$ is the first fragment of the part, $f_{-1}$ is the last, and $f_{–2}$ is the second last.

$f_{–1}$ defaults to $f_{0}$, and $f_{–2}$ defaults to $0$.
