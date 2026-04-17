# Mac で CapsLock を Ctrl に変更する

## 設定方法

### GUI

1. **システム設定** → **キーボード** → **キーボードショートカット** → **修飾キー** を開く
2. **Caps Lock** を **^ Control** に変更する

### ターミナル

1. 以下のコマンドを実行する

```bash
hidutil property --set '{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000039,"HIDKeyboardModifierMappingDst":0x7000000E0}]}'
```

> 注意: この変更は再起動するとリセットされる。永続化するには LaunchAgent を作成する。

2. `~/Library/LaunchAgents/com.local.caps-to-ctrl.plist` を作成する

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

3. LaunchAgent を読み込む

```bash
launchctl load ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
```
