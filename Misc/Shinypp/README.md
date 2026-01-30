# Shiny++
This module allows you to generate animated effects (such as the one the shiny item produces) and apply it to anything in the game.

## How to Use
1. Make a new instance using the `new` keyword.
2. After that, initialise the effect using the `InitEffect` functions, providing either a UI component or a `[width, height]` array.
3. Next, you need to create a function which determines where the effect should be drawn.
    * If you are using it for a UI component, its best the same function to position the element is used. If your function takes in the UI element and sets its position, add that function to the effect by setting `UIPositionCallback`.
    * Otherwise, create a function that takes in no arguments and returns an `[x, y]` array, and set `PositionCallback` to it. Note that advanced prints such as `>h` draw over canvases, and hence cant be made shiny through this module.
4. Lastly, make sure to run the `Main` method every frame to render the effect (the others should be run when `loc.begin | loc.loop`). It takes in a mask, which is a bitstring which encodes which tiles are empty and shouldnt be drawn over by the effect. You can generate a mask from the `Mask` module at `Utils`. If you want all tiles to have the effect, use the custom mask `"1"`.

Here is some sample code to create a shiny hat using the module.
```
var cut = import Utils/Canvas
var ms = import Utils/Mask

func SetHatPos(comp)
  var x = screen.FromWorldX(pos.x) + hatOff[0]
  var y = screen.FromWorldZ(pos.z-pos.y) +
         ^hatOff[1]

  comp.x = x
  comp.y = y

var hat_ascii = ascii
#,.
´#)\_
#` # ´
asciiend

var hatOff = [-3, -4]
var hatSize = [6, 3]
var hatCol1 = #ff446f
var hatCol2 = #50b
var hatMask = ms.GenerateMask
^(hat_ascii, hatSize[0])

var hatCanvas
var hatspp = new Misc/Shinypp/Main
?loc.begin | loc.loop
  hatCanvas = ui.AddCanvas()
  hatCanvas.dock = "top_left"
  hatCanvas.anchor = "top_left"
  hatCanvas.w = hatSize[0]
  hatCanvas.h = hatSize[1]
  cut.SetCanvasSprite(hatCanvas, hat_ascii)
  cut.SetCanvasGradient(hatCanvas, hatCol1,
  ^hatCol2, "top_center")

  hatspp.InitShinyEffect(hatCanvas)
  hatspp.UIPositionCallback = SetHatPos

SetHatPos(hatCanvas)
hatspp.Main(hatMask)
```

## Features
* Create a shiny effect using the `InitShinyEffect` function
* Pause all operation by toggling the `canPerform` variable.
* Hijack the logic of the effect by adding flags to or removing flags from the `conditions` array. See the "User Facing" section of the code for more details.
