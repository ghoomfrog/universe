# Chain

A chain is a variable-sized data format, consisting of <ins>chainlets</ins>:

Field   |Size |Description
--------|-----|-----------
More    |1b   |whether to expect more chainlets
Fragment|$8k-1$ b|fragments, concatenated together, form the actual data: the *content*<br><br>$k$ is any natural number
