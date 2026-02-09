# Shiny++
This module allows you to generate animated effects (such as the one the shiny item produces) and apply it to anything in the game.

**Note:** Currently, this module is under development and may recieve updates that make previous code using it invalid. Use with caution, and be prepared to update your code if necessary. Backwards compatability will be ensured, but not at this stage.

## How to Use
1. Make a new instance using the `new` keyword, and save it to a variable.
2. Initialise the effect using the InitEffect functions, providing either a UI component or a `[x, y, width, height]` array.
    * If you provide a UI component, the effect will be 'bound' to it, and its position will follow the element every frame (unless bypassed or prevented).
    * If you don't want this behavior, it can be changed as follows
        * Pass in a function (say `MyFunction`) that has 1 parameter which accepts a UI element. It should set the x and y positions of that element as needed. Call `UIPositionCallback = MyFunction` when initialising.
        * Pass in a function that has no parameters, and returns a suitable `[x, y]` array. Call `PositionCallback = MyFunction` when initialising.
3. Lastly, make sure to run the `Main` method every frame to render the effect. It takes in a mask, which is a bitstring which encodes which tiles are empty and shouldnt be drawn over by the effect. You can generate a mask from the `Mask` module at `Utils`. If you want all tiles to have the effect, use the mask `"1"`.

**Notes**:
* Advanced prints such as `>h` draw over canvases, and hence cant be made shiny through this module.
* `Main` must be called every frame after an instance of the module has been created. All other commands should only be run when needed, and commands regarding initialisation should be run on `loc.begin | loc.loop`.

## Features
* Create a shiny effect using the `InitShinyEffect` function
    * `InitShinyEffect` defaults to `InitShinyEffectFG`, which runs the effect on the foreground, as in the base game. If you want to apply it to the background, use `InitShinyEffectBG`.
* Pause all effects by toggling the `canPerform` variable.
* Pause movement by toggling the `canMove` variable. It can be bypassed using `positionX` and `positionY`.
* Use `ToggleCondition` to control effects more precisely. The truth value of each condition is inversed each time the function is called. A list of all meaningful parameters is given below.
    * **Shiny**
        * `"pause_shiny"`/`"pause_shade"`: Stops the progression of the effect. It does not reset the progress of the effect, and it is still visible.
        * `"disable_shade"`/`"disable_shiny"`: It is same as pause but also resets progress. After reenabling, the animation instantly plays again.
        * `"playonce_shade"`/`"playonce_shade"`: It allows the effect to play once before being disabled. It overrides disables, but not pauses.

## Examples
Here is some sample code to create a shiny hat using the module.
```
var cut = import Utils/Canvas
var ms = import Utils/Mask

var hat_ascii = ascii
#,.
´#)\_
#` # ´
asciiend

var hatOff = [-3, -4]
var hatSize = [6, 3]
var hatCol1 = #ff446f
var hatCol2 = #50b
var hatMask = ms.GenerateMask(hat_ascii, hatSize[0])

var hatCanvas
var hatspp = new Misc/Shinypp/Main
?loc.begin | loc.loop
  hatCanvas = ui.AddCanvas()
  hatCanvas.dock = "top_left"
  hatCanvas.anchor = "top_left"
  hatCanvas.w = hatSize[0]
  hatCanvas.h = hatSize[1]
  cut.SetCanvasSprite(hatCanvas, hat_ascii)

  hatspp.InitShinyEffect(hatCanvas)

hatCanvas.x = screen.FromWorldX(pos.x) + hatOff[0]
hatCanvas.y = screen.FromWorldZ(pos.z-pos.y) + hatOff[1]

cut.ApplyColorAnimation(hatCanvas, "twilight", hatMask, 1, 2, totaltime)
hatspp.Main(hatMask)
```
