# Capslock as Ctrl on Mac

## How to set up

### GUI

1. Open **System Settings** -> **Keyboard** -> **Keyboard Shortcuts** -> **Modifier Keys**
2. Change **Caps Lock** to **^ Control**

### Terminal

1. Run the following command

```bash
hidutil property --set '{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000039,"HIDKeyboardModifierMappingDst":0x7000000E0}]}'
```

> Note: This change is reset on reboot. To persist it, create a LaunchAgent.

2. Create `~/Library/LaunchAgents/com.local.caps-to-ctrl.plist`

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

3. Load the LaunchAgent

```bash
launchctl load ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
```
