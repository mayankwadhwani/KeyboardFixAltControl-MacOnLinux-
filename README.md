# KeyboardFixAltControl-MacOnLinux-
Mac keyboard layout on linux
In 16.04, here's the way I finally got this to work. Xmodmap doesn't work universally in all apps, gnome tweak tool lacked the function, dconf editing a custom altwin2 key swap (like the main answer here) failed, so I was tearing my hair out until I combined several answers into this complete, simple, and elegant solution:

sudo gedit /usr/share/X11/xkb/symbols/pc

change it to:

```
default  partial alphanumeric_keys modifier_keys
xkb_symbols "pc105" {

key <ESC>  {    [ Escape        ]   };

// The extra key on many European keyboards:
key <LSGT> {    [ less, greater, bar, brokenbar ] };

// The following keys are common to all layouts.
key <BKSL> {    [ backslash,    bar ]   };
key <SPCE> {    [    space      ]   };

include "srvr_ctrl(fkey2vt)"
include "pc(editing)"
include "keypad(x11)"

key <BKSP> {    [ BackSpace, BackSpace  ]   };

key  <TAB> {    [ Tab,  ISO_Left_Tab    ]   };
key <RTRN> {    [ Return        ]   };

key <CAPS> {    [ Caps_Lock     ]   };
key <NMLK> {    [ Num_Lock      ]   };

key <LFSH> {    [ Shift_L       ]   };
key <LCTL> {    [ Alt_L     ]   };
key <LWIN> {    [ Super_L       ]   };

key <RTSH> {    [ Shift_R       ]   };
key <RCTL> {    [ Alt_R     ]   };
key <RWIN> {    [ Super_R       ]   };
key <MENU> {    [ Menu          ]   };

// Beginning of modifier mappings.
modifier_map Shift  { Shift_L, Shift_R };
modifier_map Lock   { Caps_Lock };
modifier_map Control{ Control_L, Control_R };
modifier_map Mod2   { Num_Lock };
modifier_map Mod4   { Super_L, Super_R };

// Fake keys for virtual<->real modifiers mapping:
key <LVL3> {    [ ISO_Level3_Shift  ]   };
key <MDSW> {    [ Mode_switch       ]   };
modifier_map Mod5   { <LVL3>, <MDSW> };

key <ALT>  {    [ NoSymbol, Control_L, Control_R    ]   };
//include "altwin(meta_alt)"
key <LALT> {    [ Control_L     ]   };
key <RALT> {    [ Control_R     ]   };
modifier_map Mod1   { Alt_L, Alt_R, Meta_L, Meta_R };

key <META> {    [ NoSymbol, Meta_L, Meta_R  ]   };
modifier_map Mod1   { <META> };

key <SUPR> {    [ NoSymbol, Super_L ]   };
modifier_map Mod4   { <SUPR> };

key <HYPR> {    [ NoSymbol, Hyper_L ]   };
modifier_map Mod4   { <HYPR> };
// End of modifier mappings.

key <OUTP> { [ XF86Display ] };
key <KITG> { [ XF86KbdLightOnOff ] };
key <KIDN> { [ XF86KbdBrightnessDown ] };
key <KIUP> { [ XF86KbdBrightnessUp ] };
};

hidden partial alphanumeric_keys
xkb_symbols "editing" {
key <PRSC> {
type= "PC_ALT_LEVEL2",
symbols[Group1]= [ Print, Sys_Req ]
};
key <SCLK> {    [  Scroll_Lock      ]   };
key <PAUS> {
type= "PC_CONTROL_LEVEL2",
symbols[Group1]= [ Pause, Break ]
};
key  <INS> {    [  Insert       ]   };
key <HOME> {    [  Home         ]   };
key <PGUP> {    [  Prior        ]   };
key <DELE> {    [  Delete       ]   };
key  <END> {    [  End          ]   };
key <PGDN> {    [  Next         ]   };

key   <UP> {    [  Up           ]   };
key <LEFT> {    [  Left         ]   };
key <DOWN> {    [  Down         ]   };
key <RGHT> {    [  Right        ]   };
};
```
  
Save.

rm -rf /var/lib/xkb/*
(I don't know if this is required, but I did it.)

Reboot.
