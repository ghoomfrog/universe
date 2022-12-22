# Intlet-Based Arbitrary-Precision Integer

An intlet-based arbitrary-precision integer constitutes of a sequence of <ins>intlets</ins>:

Field   |Size|Description
--------|----|-----------
More    |1b  |whether to expect more intlets
Fragment|63b |fragments concatenated together form the actual integer (the first of which can have a sign bit)
