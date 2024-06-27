# Stripped Blended Maps

As mentioned, the game doesn't carry the map as shown in-game (marker icons and placenames are also separate) but in two files, one having the 'real' map and another one carrying the banner with the zone name, the borders and some parchment-like texture.

This folder include a version of the maps, blended with the texture but without the banner or borders.

## Process

First step was to retrieve the background images. There's one for each map. For example, the `Yanxia` zone map is in file `e3f200_m.tex` and has the associated background in `e3f200m_m.tex`.

Analysis of a few samples show that there are three versions of the backgrounds (except for the banner): one for ARR+HW, one for SB and one for ShB.

For each of these three versions, the next step is to process the mask to remove the banner and borders. This was done with GIMP with the resynthetizer plug-in.

1. Banner: this is easy, as we just need to find two zones from the same set of backgrounds for which the banner placement is different. From there, we cut the banner in one of the zone, layer the other zone below, et voila!
2. Borders: this is a bit more involved and outcome is less 'perfect'.

    * for ARR+HW and SB: I first used `Select by Color Tool` on the characterstic colors of the border.

      Then I refined the selection with the `Fuzzy Select Tool` to capture parts of the border which share colors with the background (the contiguous constraint prevent capturing the background compared to the first tool)

      Finally, I used the `Heal Selection` tool from the resynthetizer plugin with sampling width of 25.

    * for ShB, the previous method doesn't really work as the border covers completely the background up to the edge so the `Heal Selection` tool hasn't enough data to work with.

      Instead, I selected the border by doing a rectangle selection of the inside and inverting the selection, made sure the layer had an alpha channel and use the `Heal Selection` tool, this time with a sample width of 50.

Once the masks are done, it's just a matter of blending the map and the appropriate background. Credit here to [SaintCoinach](https://github.com/ufx/SaintCoinach) from which i took the blending method.

Since ShB, it seems the (parchment) texture is unchanged, only the borders and map title design is different so even though I made masks for EW, the tool will use the Shb ones for ShB, EW and DT.

This part is implemented in [ffixv-huntmaps-maker](https://github.com/RKI027/ffxiv-huntmaps-maker) along with the commands to go through all maps.
