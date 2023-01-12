# hl_manager.nvim
 manage tweak highlights in nvim

### Solve highlights problem

Let's say plugin 'indent line' is using highlight `indent_hl`, which is set to random color - 'red'.
Instead of setting it up manually for each theme, it would be great to match it to `normal` background color but 10% darker:
 `indent_hl` = theme.background - 10%
This way no matter which theme you use, `indent_hl` will match your colorsheme.

### Usage
```lua
local hl_manager = require "hl_manager"

hl_manager.highlight_link(from_highlight, to_highlight)  -- link new or existing 'to_highlight' to 'from_highlight' (works even if theme will try to override it)
hl_manager.match_hl_to_highlight(adjusted_highlight_name, ref_highlight, opts)  -- similar to above but new_highlight_name can be adjusted contrast using opts
hl_manager.highlight_from_src(new_highlight_name, src_highlight, opts)  -- create new  'new_highlight_name' using 'from_highlight' props, with option to adjust contrast
hl_manager.match_color_to_highlight(base_color, ref_highlight, new_highlight_name, mode)  -- create 'new_highlight_name', set it color to base_color, and adjust to ref_highlight
```

### Examples

If **opts = {bg=10}** It makes bright color brighter, and dark colors darker (10%).

If **opts = {bg=-5}** Bright color will get darker, dark will get brighter (by 5%).

Bright color -> its value > 0.5
Dark color -> its value < 0.5

```lua
local hl_manager = require "hl_manager"
```

- ```hl_manager.highlight_link("Whitespace", "Comment")```<br>
Link  Whitespace highlight,  to "Comment" highlight  (updated after theme change, thus u are sure new theme won't override Whitespace to something else  - normal vim hl link way does not guarantee this)


- ```hl_manager.match_hl_to_highlight("WinBar", "Normal", {fg = -14, bg = 2})```<br>
use existing WinBar highlight, and update its foreground to be same as normal highlight but 14 less contrast, and background with 2% more contrast (updated on theme change)


- ```hl_manager.highlight_from_src("NormalNC", "Normal", {bg = -5})```<br>
create NormalNC highlight, that is -5 darker than current theme background (color is updated on theme change)


 - match_color_to_highlight - it will take input color, and adjust it brightness to `reference_hl` -> finally outputing new highlight based on that color. Used below  for 'nvim-ts-rainbow'  plugin -  which gives random color to matching brackets.  With `match_color_to_highlight`  each bracket brightness will automatically match dark/light theme   (dark brackets for light theme,  bright brackets for dark theme -  uses '@punct.bracket' as reference for output  absolute brightness)

  ```lua
    local hl_manager = require "hl_manager"
    hl_manager.match_color_to_highlight("#ebcb8b", "@punct.bracket", "rainbowcol1", "foreground")
    hl_manager.match_color_to_highlight("#a3be8c", "@punct.bracket", "rainbowcol2", "foreground")
    hl_manager.match_color_to_highlight("#88c0d0", "@punct.bracket", "rainbowcol3", "foreground")
    hl_manager.match_color_to_highlight("#6ea1ec", "@punct.bracket", "rainbowcol4", "foreground")
    hl_manager.match_color_to_highlight("#b48ead", "@punct.bracket", "rainbowcol5", "foreground")
    hl_manager.match_color_to_highlight("#df717a", "@punct.bracket", "rainbowcol6", "foreground")
    hl_manager.match_color_to_highlight("#d08770", "@punct.bracket", "rainbowcol7", "foreground")
```

### INFO
hl_manager.nvim is using parts of nightfox.color library, for color matching, brightness/contrast adjustment, etc.



