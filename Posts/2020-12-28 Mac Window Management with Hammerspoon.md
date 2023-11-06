---
published: true
title: Mac Window Management with Hammerspoon
thumbnail: "![[keyboard.jpeg]]"
---
```dataviewjs
dv.span(await dv.io.load("_Includes/Header.md"))
```
# Mac Window Management with Hammerspoon

[Hammerspoon](https://github.com/Hammerspoon/hammerspoon) is a really powerful but simple automation tool for OSX.

Below, I show the Lua script that I use to make Hammerspoon my basic OSX keyboard controlled window manager to move and resize windows easily.

## What it looks like

Note: The key bindings are only showing for demonstration purposes. Those overlays won’t show for you.

![[hammerspoon-web.mp4#loop&play|480x297]]
## The Hammerspoon Script

```lua
-- GRID
hs.window.animationDuration=0.2
local hotkey = require "hs.hotkey"
local grid = require "hs.grid"

grid.MARGINX = 20
grid.MARGINY = 20
grid.GRIDHEIGHT = 4
grid.GRIDWIDTH = 6

local mod_resize = {"ctrl", "cmd"}
local mod_move = {"ctrl", "alt"}

-- Move Window
hotkey.bind(mod_move, 'j', grid.pushWindowDown)
hotkey.bind(mod_move, 'k', grid.pushWindowUp)
hotkey.bind(mod_move, 'h', grid.pushWindowLeft)
hotkey.bind(mod_move, 'l', grid.pushWindowRight)

-- Resize Window
hotkey.bind(mod_resize, 'k', grid.resizeWindowShorter)
hotkey.bind(mod_resize, 'j', grid.resizeWindowTaller)
hotkey.bind(mod_resize, 'l', grid.resizeWindowWider)
hotkey.bind(mod_resize, 'h', grid.resizeWindowThinner)
```

## How to use

It’s pretty self explanatory from the code above, but you can customize the animation duration and the grid sizing to your liking.

**Note:** to make these modifier keys more ergonomic, I recommend remapping your Caps Lock key to Control. You can do so in **System Preferences -> Keyboard -> Modifier Keys**.

### Hotkeys

**Ctrl + Opt + j** : Move window down  
**Ctrl + Opt + k** : Move window up  
**Ctrl + Opt + h** : Move window left  
**Ctrl + Opt + l** : Move window right

**Ctrl + Cmd + j** : Resize window down  
**Ctrl + Cmd + k** : Resize window up  
**Ctrl + Cmd + h** : Resize window left  
**Ctrl + Cmd + l** : Resize window right