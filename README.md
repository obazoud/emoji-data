This project provides easy-to-parse data about emoji, along with a spritesheet-style 
images for use on the web.

You can see a catalog of the emoji data here: http://unicodey.com/emoji-data/table.htm

The original images come from <a href="https://github.com/github/gemoji">gemoji</a>
and you can use the individual images directly with the data too.


## Using the data

The file you want is `emoji.json`. It contains an array of entries for emoji that 
look like this:

	[
		{
			"name": "BLACK SUN WITH RAYS",
			"unified": "2600",
			"docomo": "E63E",
			"au": "E488",
			"softbank": "E04A",
			"google": "FE000",
			"image": "2600.png",
			"sheet_x": 25,
			"sheet_y": 6,
			"short_name": "sunny",
			"text": null,
		},
		...
	]

An explanation of the various fields is in order:

* `name` - The offical Unicode name, in SHOUTY UPPERCASE
* `unified` - The Unicode codepoint, as 4-5 hex digits. Where an emoji
   needs 2 codepoints, they are specified like `1F1EA-1F1F8`.
* `docomo`, `au`, `softbank`, `google` - The Unicdoe codepoints used
   by various mobile vendors.
* `images` - The name of the image file in `gemoji/images/emoji/unicode/`
* `sheet_x` & `sheet_y` - The position of the image in the spritesheets.
* `short_name` - The commonly-agreed upon short name for the image, as
   supported in campfire, github etc via the :colon-syntax:
* `short_names` - An array of all the known short names.
* `text` - An ASCII version of the emoji (e.g. `:)`), or null where
   none exists.


## Rebuilding the data

The scripts in the `build/` sub directory allow you to rebuild the data files 
from the unicode.org source materials and the images from the gemoji project.

You can rebuild by following these steps:

	cd build/

	# Rebuild catalog.php from the original data tables
	wget http://www.unicode.org/~scherer/emoji4unicode/snapshot/full.html
	patch < source_html.patch
	php build_catalog.php full.html > catalog.php

	# Rebuild positions and naming catalogs, and make the master spritesheets
	# (This step requires ImageMagick or GraphicsMagick)
	php build_image.php # this is slow!
	php build_sheets.php
	php build_names.php

	# Finally, build a single mapping file
	wget http://www.unicode.org/Public/UNIDATA/UnicodeData.txt
	php build_map.php

	# From the final mapping, build a preview table
	php build_table.php > ../table.htm
