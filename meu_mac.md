### iterm2 : https://iterm2.com/

instalação feita via .dmg 

algumas configurações:
1. abrir as configurações (⌘ + ,)
2. `appearance` > `tabs` > :ballot_box_with_check: `Show tab bar even when there is only tab`
3. `profiles` > `Profile Name: Default` > `Colors`:
 
   1. Basic Colors:

       1. Foreground: `#cdcbcb`
       2. Background: `#0e0f19`
       3. :ballot_box_with_check: Bold: `#ffffff`
       4. Links: `#4d8ce7`
       5. Find Match: `#74fb56`
       6. Selection: `#ffb900`
       7. :ballot_box_with_check: Selected text: `#000000`

  2. Cursor Colors:

      1. Cursor: `#ffb900`
      2. Cursor text: `#000000`

  3. ANSI Colors: Normal | Bright
  
      1. Black: `#232323` | `#444444`
      2. Red: `#ff000f` | `#ff2740`
      3. Green: `#8ce10b` | `#abe15b`
      4. Yellow: `#cbc43f` | `#fcf350`
      5. Blue: `#008df8` | `#0092ff`
      6. Magenta: `#6d43a6` | `#9a5feb`
      7. Cyan: `#00d8eb` | `#67fff0`
      8. White: `#ffffff` | `#ffffff`

4. `profiles` > `Profile Name: Default` > `Text`:

   1. Cursor: :radio_button: `Undeline` | `Vertical bar` | `Box`
   2. Font: JetBrainsMonoNL Nerd Font Mono, Regular, 13

5. `profiles` > `Profile Name: Default` > `Windows`:

   1. Settings for New Windows:

      1. Columns: `120`
      2. Rows: `49`

6. `profiles` > `Profile Name: Default` > `Keys`:

   1. Left Option Key: `Normal` | `Meta` | :radio_button: `Esc+`
   2. Right Option Key: `Normal` | `Meta` | :radio_button: `Esc+`

7. `Keys` > `Key Bindings` (deixar somente os):

   1. Previous tab: `⌘ ←`
   2. Next tab: `⌘ →`

8. `Keys` > `Navigation Shortcuts` >  todos como `No shortcut`

### zellij : https://zellij.dev/

instalação feita via brew `brew install zellij`

1. criar arquivo layout

`zellij setup --dump-layout default > ~/zellij-layout-file.kdl`

2. alterar o layout para:
```
layout {
    pane size=3 borderless=true {
        plugin location="status-bar"
    }
    pane size=2 borderless=true {
        plugin location="tab-bar"
    }
    pane stacked=true {
        pane
        pane
    }
}
```
3. recuperar ou iniciar uma sessão com o layout

`zellij attach -f local || zellij --session local --layout ~/zellij-layout-file.kdl`
