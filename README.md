# balanced-photomosaic

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


# Example

Here is my octocat (from https://myoctocat.com/)

<p align="center">
<img src="https://github.com/jiahaozhang2025/photo-mosaic/blob/main/images/octocat.png" width="500"/>
</p>

Here is the comparison of the mosaic built from a dataset of emojis. Which is better is of course a matter of personal taste, personally I do not want my octocat face to be made of emojis of poopies and aliens. 

Interestingly, in `meanstd` mode, emojis show up primarily along the contour, hence highlighting the features in the target image. You can check the math in the code, but basically: in chromatically uniform regions the color variation is low -> the ratio of target std over emoji std is low -> this ratio sets how much the original contrast of the emoji is preserved -> so the contrast of emoji tile is suppressed and blent into background.

| Target | Conventional Approach |
|:------:|:--------------:|
| <img src="https://github.com/jiahaozhang2025/photo-mosaic/blob/main/images/octocat_zoom.png" width="400"/> | <img src="https://github.com/jiahaozhang2025/photo-mosaic/blob/main/images/mosaic_zoom_conventional.jpeg" width="400"/> |

| This Approach (mean mode) | This Approach (meanstd mode) |
|:------:|:--------------:|
| <img src="https://github.com/jiahaozhang2025/photo-mosaic/blob/main/images/mosaic_zoom_mean.jpeg" width="400"/> | <img src="https://github.com/jiahaozhang2025/photo-mosaic/blob/main/images/mosaic_zoom_meanstd.jpeg" width="400"/> |

# Acknowledgements

Inspired by the classic best-fit photomosaic approach by codebox: https://github.com/codebox/mosaic.git
