# Utils
Smaller functions for a variety of purposes

## Canvas
Utility functions primarily revolving around the Canvas UI element

### Functions
* **SetCanvasSprite**: Given a canvas and any asciiart, renders the ascii onto the canvas from the top-left. The width of the asciiart must be lesser than or equal to that of the canvas

* **SetCanvasGradient**: Given a canvas, a starting and ending color, and a starting position, applies a gradient to the canvas. Starting position is a compound string of "top", "center" or "bottom", and "left, "center", or "right", with an `_` delimiter. It indicates where the starting color appears. The ending color is on the opposite side. Examples: `"top_right"`, `"center_left"`.

* **ApplyColorAnimation**: Must be run every frame. Creates an animated foreground color effect on the canvas. Takes in a canvas, an array of colors, a mask, x and y shears, and a time iterator (best effect when iterating by 1 each frame). The mask can be `"1"` for all tiles to recieve the effect. If the `colors` variable is not any of the specified strings, or an array of colors, the default array is used (currently `"HSV_85"`). The x and y shears are the index values on the color arrya by which subsequent tiles differ in the x and y directions respectively. A value of `±1` for x and `±2` for y are recommended. The strings used to access the built-in colors are as follows
    * `"HSV"`: Uses a 30-long array of colors generated from `OKHSV ( t° S% 100% )`, where `t` starts from 0 and incerememnts by 12 per entry. Enter S values with `"HSV_70"`, `"HSV_85"`, or `"HSV_100"`. `"HSV"` defaults to `"HSV_85"`

    * `"HSL"`: Uses a 30-long array of colors generated from `OKHSL ( t° 100% L% )`, where `t` starts from 0 and incerememnts by 12 per entry. Enter L values with `"HSL_60"`, `"HSL_70"`, or `"HSL_80"`. `"HSL"` defaults to `"HSL_70"`

    * `"twilight"`: Uses an array of colors interpolated between `#5466F2` and `#0070BC`. `"twilight_A"` and `"twilight_B"` have different interpolants. `"twilight"` defaults to `"twilight_A"`
