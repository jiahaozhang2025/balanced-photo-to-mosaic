# BRC-Mosaic: Balanced Random Color-Matched Mosaic

An alternative-approach photomosaic generator that balances tile usage randomly and color-matches each placement to its target patch.

This avoids the classic failure mode where large areas of similar colors—or a tile set that doesn’t sample the RGB space enough—cause the same few tiles to repeat over huge swathes of the image.

Traditional “match-and-select” mosaics choose, for each grid cell, the single best-matching tile from your library. When the target image has big regions with similar RGB values (sky, walls, skin, ocean) or your tile dataset doesn’t sample the RGB space enough, the same tile wins over and over, creating visible repetition and banding.

For example, you might be tasked with generating a mosaic of your organization's logo using members' profile photos as tiles. Member A has their entire face fill up the photo, while Member B has a full-body portrait on top of a snowy mountain as the photo. Then, all white areas of the company logo will be dominated by Member B's photo, whereas Member A is nowhere to be found. In practice, photos that are not this obviously different can suffer such problem just as well.

This approach solves this issue by:

* Variety first: Assign tiles randomly but balanced (so the whole set gets used evenly).

* Fidelity per cell: Recolor each chosen tile to match the local patch’s statistics (mean, mean+std, or just luma).

You keep global target fidelity and local variety—even with small or color-limited tile sets.

# Installation

    pip install pillow numpy
    
# Usage

    python photomosaic.py target.jpg tiles_folder --tile-size 50 --enlargement 8 --mode meanstd --seed 123 --out mosaic.jpeg

or just

    python photomosaic.py target.jpg tiles_folder

# Acknowledgements

Inspired by the classic best-fit photomosaic approach by codebox: https://github.com/codebox/mosaic.git
