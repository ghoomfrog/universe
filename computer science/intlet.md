# Intlet

An intlet is a partial object that can sequentially form an arbitrary-precision integer. An intlet is structured as follows:

Field   |Size|Description
--------|----|-----------
More    |1b  |whether to expect more intlets
Fragment|63b |fragments, concatenated together, form the <ins>actual integer</ins>
