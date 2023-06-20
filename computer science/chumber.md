# Chumber

A chumber is a rational number format consisting of numlets. Here's the numlet structure:

Field     |Size       |Description
----------|-----------|-----------
Fractional|1b         |<ol start="0"><li>The fragment is part of the integer part.<li>The fragment is part of the fractional part.</ol>
Fragment  |$8k-1$ bits|Fragments, [combined](#combining-fragments) together, form their respective part. $k$ is any natural number.

The fragment of the first numlet can contain the sign. Both parts default to 0.

## Combining Fragments

Parts are obtained using the following formula:

$$(\sum_{f=f_0}^{f_{-2}} 2^{8k-1}f) + f_{–1}$$

where $f_0$ is the first fragment of the part, $f_{-1}$ is the last, and $f_{–2}$ is the second last.

$f_{–1}$ defaults to $f_{0}$, and $f_{–2}$ defaults to $0$.
