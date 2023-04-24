# Overwrite/Insert Difference Format

The Overwrite/Insert Difference Format (OIDF) is an efficient, computer-friendly format for describing the byte-level difference between two pieces of nullphobic[^nullphobic] data. The difference is a sequence of structures in the following format:

Field          |Size   |Description
---------------|-------|-----------
Difference Type|1b     |<ol start="0"><li>Overwrite<li>Insert
Index Length   |7b min.|This is the dynamic index at which the difference is, formatted as a [chain](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) where the first fragment is 6-bit. "Dynamic" here means that the index is relative to the last byte of its previous overwriting/inserted difference.
Difference     |8b min.|This is a null-terminated array of bytes.

[^nullphobic]: nullphobic data is data where zero is not considered a datum, like Unicode-encoded text
