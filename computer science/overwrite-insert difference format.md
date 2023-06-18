# Overwrite/Insert Difference Format

The Overwrite/Insert Difference Format (OIDF) is an efficient, computer-friendly format for describing the byte-level difference between two pieces of data. The difference is a sequence of structures in the following format:

Field          |Size   |Description
---------------|-------|-----------
Difference Type|1b     |<ol start="0"><li>Overwrite<li>Insert
Index Length   |7b min.|The dynamic index at which the difference is, formatted as a [chain](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) where the first fragment is 6-bit, and subsequent fragments are 7-bit. *Dynamic* means that the index is relative to the last byte of its previous overwriting/inserted difference.
Difference End |8b min.|The index of the last byte in the difference, formatted as a chain with 7-bit fragments
Difference     |8b min.|The array of bytes in the difference
