# Compressed Chunk Image Format

Compressed Chunk Image Format (CCIF) is a lossless image format that uses advanced compression techniques. It supports a variety of color modes, and includes features such as transparency support and flexible palette definitions. It also offers faster decoding times.

This document speficies CCIF 1. All [chains](https://github.com/ghoomy/universe/blob/main/computer%20science/chain.md) described in this document have 8-bit chainlets.

## Header

Field                 |Size   |Description
----------------------|-------|-----------
Version               |8b min.|Version + 1 as a chain
Width                 |16b    |Image width + 1
Height                |16b    |Image height + 1
Color mode            |3b     |<ol start="0"><li>Monochromatic<li>Red<li>Green<li>Blue<li>Red-green<li>Red-blue<li>Green-blue<li>RGB
Color depth*          |2b     |<ol start="0"><li>8 bits<li>16 bits<li>24 bits<li>32 bits
Alpha mode            |1b     |Whether there is an alpha channel
Palette existence     |1b     |Whether a palette is defined
Macro existence       |1b     |Whether macros are defined

\* Numbers whose bit count is equal to Color Depth are said to be *color-deep*.

## Palette

If the palette existence is 1, a palette definition immediately follows the header. It contains an item count + 1 as a chain followed by [chunks](#chunks). Explicit chunks in the palette can only be channel values.

## Macros

If the macro existence is 1, a macro count + 1 as a chain, and macro definitions immediately follow the palette (or header if there is no palette). Each macro contains a chunk count + 1 as a chain followed by [chunks](#chunks).

## Chunks

Chunks are sequenced in groups of three preceded by a padding bit and three 3-bit tags that specify the type of their respective chunks in the group:

Chunk Type       |Tag|Description
-----------------|---|-----------
Explicit         |0  |An array of color-deep channel values (ending with an alpha value if the alpha mode is 1)
Palette-indexive |1  |A palette item index as a chain
Micro-indexive   |2  |A relative chunk index as a chain, referencing farther preceding chunks as it increases where 0 refers to the immediately preceding chunk[^preceding]
Run-length       |3  |As a chain, a run length of the preceding chunk[^preceding]
Full difference  |4  |<ul><li>If there is one channel, the chunk is a 4-bit delta followed by a padding bit and a 3-bit tag for the next chunk outside the current group.<li>If there are two channels, the chunk is two respective 4-bit deltas<li>If there are three channels, the chunk is three respective 4-bit deltas followed by a padding bit and a 3-bit tag for the next chunk outside the current group.
Luma difference  |5  |A color-deep luma delta from the preceding chunk[^preceding]
Chroma difference|6  |<ul><li>If the color mode is bi-channel, the chunk is a color-deep chroma delta from the preceding chunk.[^preceding]<li>If the color mode is tri-channel, the chunk is one 4-bit A chroma delta and one 4-bit B chroma delta from the preceding chunk.[^preceding]
Macro-indexive   |7  |A macro index as a chain

[^preceding]: If the preceding chunk doesn't exist, it defaults to fully opaque black.

## Padding Bits

Padding bits are non-contiguously sequenced into groups of three that act as tags for the following chunks outside their current group. Padding bit tags take precedence over regular tags inside full difference chunks.
