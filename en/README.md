# CapsLock to Ctrl

How to remap CapsLock to Ctrl on each OS.

- [Windows](#windows)
- [macOS](#macos)
- [Linux (X11)](#linux-x11)
- [Linux (Wayland)](#linux-wayland)
  - [Hyprland](#hyprland)

---

## Windows

Requires a reboot to take effect.

### Using the registry file

1. Download [`CapslockToCtrl.reg`](../windows/CapslockToCtrl.reg) and double-click it
2. Restart your PC

### Editing the registry manually

1. Open Registry Editor (`regedit`)
2. Navigate to:

   ```
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout
   ```

3. Create a binary value named `Scancode Map` with this data:

   ```
   00 00 00 00 00 00 00 00
   02 00 00 00 1d 00 3a 00
   00 00 00 00
   ```

4. Restart your PC

### Verify

```powershell
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Keyboard Layout" /v "Scancode Map"
```

### Revert

Delete the `Scancode Map` value and restart your PC:

```powershell
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Keyboard Layout" /v "Scancode Map" /f
```

---

## macOS

### GUI

1. Open **System Settings** → **Keyboard** → **Keyboard Shortcuts** → **Modifier Keys**
2. Change **Caps Lock** to **^ Control**

This persists across reboots.

### Terminal

```bash
hidutil property --set '{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000039,"HIDKeyboardModifierMappingDst":0x7000000E0}]}'
```

> **Note:** `hidutil` is reset on reboot. To persist it, create a LaunchAgent as below.

#### Persist with a LaunchAgent

1. Create `~/Library/LaunchAgents/com.local.caps-to-ctrl.plist`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>Label</key>
       <string>com.local.caps-to-ctrl</string>
       <key>ProgramArguments</key>
       <array>
           <string>/usr/bin/hidutil</string>
           <string>property</string>
           <string>--set</string>
           <string>{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000039,"HIDKeyboardModifierMappingDst":0x7000000E0}]}</string>
       </array>
       <key>RunAtLoad</key>
       <true/>
   </dict>
   </plist>
   ```

2. Load it:

   ```bash
   launchctl load ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
   ```

### Verify

```bash
hidutil property --get "UserKeyMapping"
```

### Revert

Clear the mapping, and remove the LaunchAgent if you created one:

```bash
hidutil property --set '{"UserKeyMapping":[]}'
launchctl unload ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
rm ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
```

For the GUI method, set **Caps Lock** back to **Caps Lock** in Modifier Keys.

---

## Linux (X11)

### setxkbmap

```bash
setxkbmap -option caps:ctrl_modifier
```

> **Note:** This is reset on reboot. To persist it, add the command to `~/.xinitrc` or `~/.xprofile`.

### Xmodmap

1. Add these lines to `~/.Xmodmap`:

   ```
   clear lock
   keycode 66 = Control_L
   add control = Control_L
   ```

2. Apply:

   ```bash
   xmodmap ~/.Xmodmap
   ```

> **Note:** To persist it, add `xmodmap ~/.Xmodmap` to `~/.xinitrc` or `~/.xprofile`.

### Verify

```bash
setxkbmap -query        # check the options line
xmodmap -pm             # check modifier assignments
```

### Revert

```bash
setxkbmap -option       # clear all options
```

For Xmodmap, remove the lines from `~/.Xmodmap` and log out and back in.

---

## Linux (Wayland)

Configuration depends on your compositor.

### General (environment variable)

Some compositors respect `XKB_DEFAULT_OPTIONS`. Set it in your shell profile:

```bash
export XKB_DEFAULT_OPTIONS="caps:ctrl_modifier"
```

To revert, remove that line and restart your session.

### Hyprland

Add this to `$HOME/.config/hypr/hyprland.conf`:

```conf
input {
    kb_options = "caps:ctrl_modifier"
}
```

#### Verify

```bash
hyprctl getoption input:kb_options
```

#### Revert

Remove the `kb_options` line (or set it to `""`) and reload with `hyprctl reload`.
