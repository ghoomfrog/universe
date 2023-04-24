# Compressed Chunk Image Format

Compressed Chunk Image Format (CCIF) is a lossless image format that uses advanced compression techniques. It supports a variety of color modes and includes features such as transparency support and flexible palette definitions. It also offers faster decoding times.

## Header

Field        |Size|Description
-------------|----|-----------
Width        |16b |Image width + 1
Height       |16b |Image height + 1
Color Mode   |3b  |<ol start="0"><li>Grayscale<li>Red<li>Green<li>Blue<li>Red-green<li>Red-blue<li>Green-blue<li>RGB
Color Depth  |1b  |<ol start="0"><li>8 bits<li>16 bits<br><br>Numbers whose size is equal to Color Depth are said to be *color-deep*.
Alpha Mode   |1b  |This bit specifies whether there is an alpha channel
Explicit Type|1b  |<ol start="0"><li>Explicit chunks contain channel values<li>Explicit chunks contain a palette color index
Pixel Array  |1b  |<ol start="0"><li>Pixel arrays aren't defined<li>Pixel arrays are defined
Padding Bit  |1b  |

## Palette

If the explicit type is 1, a palette definition immediately follows the header. It contains an 8-bit item count followed by [chunks](#chunks). Explicit chunks in the palette can't be palette item indices: only channel values.

## Pixel Arrays

If the pixel array existence is 1, an 8-bit array count and pixel array definitions immediately follow the palette (or header if there is no palette). Each array contains an 8-bit chunk count followed by [chunks](#chunks).

## Chunks

Chunks are sequenced in groups of three preceded by a padding bit and three 3-bit tags that specify the type of their respective chunks in the group:

Chunk Type       |Tag Value|Description
-----------------|---------|-----------
Explicit         |0        |<ul><li>If the explicit type is 0, the chunk is an array of color-deep channel values (ending with an alpha value if the alpha mode is 1)<li>If the explicit type is 1, the chunk is a 8-bit palette item index
Mono-Indexive    |1        |An 8-bit relative chunk index, referencing farther preceding chunks as it increases where 0 refers to the immediately preceding chunk[^preceding]
Run-Length       |2        |An 8-bit run length of the preceding chunk[^preceding]
Full Difference  |3        |<ul><li>If there is one channel, the chunk is a 4-bit delta followed by a padding bit and a 3-bit tag for the next chunk outside the current group<li>If there are two channels, the chunk is two respective 4-bit deltas<li>If there are three channels, the chunk is three respective 4-bit deltas followed by a padding bit and a 3-bit tag for the next chunk outside the current group
Luma Difference  |4        |A color-deep luma delta from the preceding chunk[^preceding]
Chroma Difference|5        |<ul><li>If the color mode is bi-channel, the chunk is a color-deep chroma delta from the preceding chunk[^preceding]<li>If the color mode is tri-channel, the chunk is one 4-bit A chroma delta and one 4-bit B chroma delta from the preceding chunk[^preceding]
Poly-Indexive    |6        |An 8-bit pixel array index

[^preceding]: if the preceding chunk doesn't exist, it defaults to fully opaque black

## Padding Bits

Padding bits are indirectly sequenced into groups of three that act as tags for the following chunks outside their current group. Padding bit tags take precedence over tags inside full difference chunks.