# Overwrite/Insert Difference Format

The Overwrite/Insert Difference Format (OIDF) is an efficient, computer-friendly format for describing the byte-level difference between two pieces of data. The difference is a sequence of structures, called subdiffs, in the following format:

Field          |Size   |Description
---------------|-------|-----------
Difference type|1b     |<ol start="0"><li>Overwrite<li>Insert
Index          |7b min.|The relative index at which the difference is, formatted as a [chain](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) where the first chainlet is 7-bit, and subsequent chainlets are 8-bit. *Relative* means that the index is relative to the end of its previous subdiff's overwriting/inserted content in this subdiff's previous snapshot. A snapshot is the state of the differentiated data after subdiffs, up to a certain one, are applied to it.
Content length |8b min.|The byte-length of Content, formatted as a chain with 8-bit chainlets
Content        |8b min.|The array of bytes in the difference
