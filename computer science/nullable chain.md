# Nullable Chain

A nullable chain is a [chain](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) that can convey no information. All chainlets are identical to [regular](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) chainlets except the first one which is as follows:

Field   |Size       |Description
--------|-----------|-----------
Full    |1b         |Whether the chain is not null. The remaining fields are ignored if this is 0.
More    |1b         |Whether to expect more chainlets
Fragment|$8k-2$ bits|Fragments, concatenated together, form the actual data (the content). $k$ is any natural number.
