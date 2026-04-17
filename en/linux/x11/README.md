# Capslock as Ctrl on X11

## How to set up

### setxkbmap

1. Run the following command

```bash
setxkbmap -option caps:ctrl_modifier
```

> Note: This change is reset on reboot. To persist it, add the command to your `~/.xinitrc` or `~/.xprofile`.

### Xmodmap

1. Add the following lines to `~/.Xmodmap`

```
clear lock
keycode 66 = Control_L
add control = Control_L
```

2. Apply the changes

```bash
xmodmap ~/.Xmodmap
```

> Note: To persist it, add `xmodmap ~/.Xmodmap` to your `~/.xinitrc` or `~/.xprofile`.
