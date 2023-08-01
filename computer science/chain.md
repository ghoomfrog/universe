# Chain

A chain is a variable-sized data format consisting of chainlets. Here's the chainlet structure:

Field   |Size       |Description
--------|-----------|-----------
More    |1b         |Whether to expect more chainlets
Fragment|$8k-1$ bits|Fragments, concatenated together, form the actual data (the content). $k$ is any natural number.
