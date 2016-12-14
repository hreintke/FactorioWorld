# FactorioWorld

A mod for factorio that changes the map into a real-world map.

Note that it only changes the terrain, and not the spawning of trees and resources.

# The Mod

## Installation

1. Create folder named "factorio-world_0.1.0" in your mods folder
1. Download this repo
1. Put everything from this repo into that folder

Note: The folders "Screenshots" and "MapGenerator" aren't strictly needed to install the mod, but they don't harm either.

## Usage

Simply start a new game, you should then start in England.

Note that it can take a long time to load all the required data.
This means your game will hang for a while when starting a map.
To see progress of the loading run your game from the console (so you can see debug messages).
In steam add "cmd /c %command%" to your launch options, as this will also show the console.

## Limitations

* Currently only grass, water and dirt is generated
    * The mod is written to support everything, however the image-converter only supports those 3 for the moment.
* You always start in England
* The map should be infinite, many many times the same world next to eachother. However this is untested.
    * Actually, there hasn't been much testing yet, so there could be many bugs.

# Generating your own maps

The mod reads lua files generated by the converter.
These files are generated based on images.
Currently I'm using the "Natural Earth II with Shaded Relief, Water, and Drainages" image from [Natural Earth](http://www.naturalearthdata.com/downloads/10m-raster-data/10m-natural-earth-2/).

The generator simply iterates over each pixel, and assigns a tile-type to it.
Currently it only support grass, water and dirt.
It simply makes the pixel grass if the green-channel is highest, water if the blue-channels wins and dirt if the red-channel is victorious.
This is then encoded as "g" for grass, "w" for water and "d" for dirt.
To compress the data a little I use a scheme where every tile-letter is followed by how many tiles there are of this type of that row.
The mod then takes these strings (one for each line of pixels), decompresses it to something it can read very quickly, and assigns the tiles a new type when a chunk is generated.

## Usage

Currently to generate a new lua file and load in into your you'll have to do a little manual labour:

1. In ConvertMap.py change the last line:
    1. It should be `convert("NE2_LR_LC_SR_W_DR_50.tif", "map_50_compressed.lua")`
    1. The first argument is the image you want to convert
    1. The second argument is the file it will end up in (make sure it end in .lua)
1. Run the lua file
1. Copy the created lua file into the mod's folder
1. In control.lua change the first line:
    1. It should be `require "map_100_compressed"`
    1. Change it to be `require "your_newly_generated_lua_files_name"` (note there is no .lua here)
1. Again in control.lua, but now a few lines below, change the 'settings':
    1. It should read `local offset = {x = 2000 * 4, y = 400 * 4}`
    1. Change this offset to something so your guy doesn't drown instantly
        * The offset is in pixel-coordinates. 0,0 is the most upper left pixel of the image.
1. Run the game and start a new game.
    * I think if you use the mod on an existing save, it will start working on newly generated chunks, but that's untested atm.

