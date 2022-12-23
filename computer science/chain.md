# Chain

A chain is a variable-sized sequence of <ins>chainlets</ins>:

Field   |Size|Description
--------|----|-----------
More    |1b  |whether to expect more chainlets
Fragment|63b |fragments, concatenated together, form the actual data: the <ins>content</ins>
