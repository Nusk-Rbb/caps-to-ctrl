# Capslock as Ctrl on Wayland

## How to set up

The configuration depends on your compositor. See the subdirectories for specific instructions.

- [Hyprland](hyprland/README.md)

### General (environment variable)

Some compositors respect `XKB_DEFAULT_OPTIONS`. You can set it in your shell profile:

```bash
export XKB_DEFAULT_OPTIONS="caps:ctrl_modifier"
```
