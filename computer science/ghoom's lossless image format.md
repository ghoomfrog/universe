# Ghoom's Lossless Image Format

## Header

Field                |Size|Description
---------------------|----|-----------
Width                |16b |Image width + 1
Height               |16b |Image height + 1
Color Mode           |3b  |<ol start="0"><li>Grayscale<li>Red<li>Green<li>Blue<li>Red-green<li>Red-blue<li>Green-blue<li>RGB
Alpha Mode           |1b  |This bit specifies whether there is an alpha channel
Explicit Type        |1b  |<ol start="0"><li>Explicit blocks contain channel values<li>Explicit blocks contain a palette color index
Pixel Array Existence|1b  |<ol start="0"><li>Pixel arrays aren't defined<li>Pixel arrays are defined

## Palette

If the explicit type is 1, a palette definition immediately follows the header. It contains an 8-bit item count followed by [blocks](#blocks). Explicit blocks in the palette can't be palette item indices: only channel values.

## Pixel Arrays

If the pixel array existence is 1, an 8-bit array count and pixel array definitions immediately follow the palette (or header if there is no palette). Each array contains an 8-bit block count followed by [blocks](#blocks).

## Blocks

Blocks are sequenced in groups of three preceded by a padding bit and three 3-bit tags that specify the type of their respective blocks in the group:

Block Type       |Tag Value|Description
-----------------|---------|-----------
Explicit         |0        |<ul><li>If the explicit type is 0, the block is an array of 8-bit channel values (ending with an alpha value if the alpha mode is 1)<li>If the explicit type is 1, the block is an 8-bit palette item index
Mono-Indexive    |1        |An 8-bit relative block index, referencing farther preceding blocks as it increases where 0 refers to the immediately preceding block[^preceding]
Run-Length       |2        |An 8-bit run length of the preceding block[^preceding]
Full Difference  |3        |<ul><li>If there is one channel, the block is a 4-bit delta followed by a padding bit and a 3-bit tag for the next block outside the current group<li>If there are two channels, the block is two respective 4-bit deltas<li>If there are three channels, the block is three respective 4-bit deltas followed by a padding bit and a 3-bit tag for the next block outside the current group
Luma Difference  |4        |An 8-bit luma delta from the preceding block[^preceding]
Chroma Difference|5        |<ul><li>If the color mode is bi-channel, the block is an 8-bit chroma delta from the preceding block[^preceding]<li>If the color mode is tri-channel, the block is one 4-bit A chroma delta and one 4-bit B chroma delta from the preceding block[^preceding]
Poly-Indexive    |6        |An 8-bit pixel array index

[^preceding]: if the preceding block doesn't exist, it defaults to fully opaque black

## Padding Bits

Padding bits are indirectly sequenced into groups of three that act as tags for the following blocks outside their current group. Padding bit tags take precedence over tags inside full difference blocks.
