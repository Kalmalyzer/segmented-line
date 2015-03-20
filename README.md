# segmented-line
2D line drawing routine for Atari ST

This package continues routines for clipping and drawing 1-bitplane lines on Atari ST.

## Technique

The line routine uses a DDA algorithm and pre-generated code. The pre-generated code variants will render line segments from location (x,y) to (x+a,y+b) where a=0..15, b=-15..+15.

The line routine will check whether there are at least 16 more pixels to go along the major axis; if so, it will call the appropriate pre-generated code variant, and advance the DDA counters by 16 pixels. If there are less than 16 pixels to go, the routine will perform DDA stepping pixel-by-pixel instead.

## Quality and performance

The resulting line will not be perfect but it will never be off by more than one pixel from an imagined perfect line. It will always begin and end at the right pixel.

This routine can draw 22 diagonals (lines from 0,0 to 319,199) in one vblank.

## Usage

Call SegmentedLineSetup to pre-generate all code. This will take about a second. Once this has been done, the line routine is ready for use.

Then do the following for each line you want to draw:

Call ClipLine with the x0,y0,x1,y1 coordinates of your line. This will return a clipped version of a line, and a flag which indicates whether or not the line is at least partially on-screen. If the line is entirely off-screen, you should not pass it to the drawing routine. If you know beforehand that the line does not need to be clipped, you do not need to call ClipLine.
All input coordinates must be within the interval (-2048, +2047). Any coordinate outside this range will result in immediate rejection of the entire line.

Call SegmentedLine with the x0,y0,x1,y1 coordinates of your line and the target framebuffer, offset to the bitplane you want to draw to. This will render the line into the framebuffer. If the line happens to go off-screen, bad things will happen.

