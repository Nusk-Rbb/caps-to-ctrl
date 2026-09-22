# CapsLock を Ctrl に変更する

各OSでの CapsLock を Ctrl に割り当てる手順。

- [Windows](#windows)
- [macOS](#macos)
- [Linux (X11)](#linux-x11)
- [Linux (Wayland)](#linux-wayland)
  - [Hyprland](#hyprland)

---

## Windows

反映には再起動が必要。

### レジストリファイルを使う方法

1. [`CapslockToCtrl.reg`](../windows/CapslockToCtrl.reg) をダウンロードしてダブルクリックする
2. PC を再起動する

### 手動でレジストリを編集する方法

1. レジストリエディタ (`regedit`) を開く
2. 以下のキーに移動する

   ```
   HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout
   ```

3. `Scancode Map` というバイナリ値を新規作成し、以下の値を設定する

   ```
   00 00 00 00 00 00 00 00
   02 00 00 00 1d 00 3a 00
   00 00 00 00
   ```

4. PC を再起動する

### 確認方法

```powershell
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Keyboard Layout" /v "Scancode Map"
```

### 元に戻す

`Scancode Map` の値を削除して PC を再起動する。

```powershell
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Keyboard Layout" /v "Scancode Map" /f
```

---

## macOS

### GUI

1. **システム設定** → **キーボード** → **キーボードショートカット** → **修飾キー** を開く
2. **Caps Lock** を **^ Control** に変更する

この方法は再起動しても設定が残る。

### ターミナル

```bash
hidutil property --set '{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000039,"HIDKeyboardModifierMappingDst":0x7000000E0}]}'
```

> **注意:** `hidutil` の設定は再起動でリセットされる。永続化するには下記の LaunchAgent を作成する。

#### LaunchAgent で永続化する

1. `~/Library/LaunchAgents/com.local.caps-to-ctrl.plist` を作成する

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

2. LaunchAgent を読み込む

   ```bash
   launchctl load ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
   ```

### 確認方法

```bash
hidutil property --get "UserKeyMapping"
```

### 元に戻す

マッピングを空にする。LaunchAgent を作成した場合は併せて削除する。

```bash
hidutil property --set '{"UserKeyMapping":[]}'
launchctl unload ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
rm ~/Library/LaunchAgents/com.local.caps-to-ctrl.plist
```

GUI で設定した場合は、修飾キーの設定で **Caps Lock** を **Caps Lock** に戻す。

---

## Linux (X11)

### setxkbmap

```bash
setxkbmap -option caps:ctrl_modifier
```

> **注意:** 再起動でリセットされる。永続化するには `~/.xinitrc` や `~/.xprofile` にコマンドを追加する。

### Xmodmap

1. `~/.Xmodmap` に以下の内容を追加する

   ```
   clear lock
   keycode 66 = Control_L
   add control = Control_L
   ```

2. 変更を適用する

   ```bash
   xmodmap ~/.Xmodmap
   ```

> **注意:** 永続化するには `~/.xinitrc` や `~/.xprofile` に `xmodmap ~/.Xmodmap` を追加する。

### 確認方法

```bash
setxkbmap -query        # options 行を確認
xmodmap -pm             # 修飾キーの割り当てを確認
```

### 元に戻す

```bash
setxkbmap -option       # オプションを全てクリア
```

Xmodmap の場合は `~/.Xmodmap` から該当行を削除し、ログインし直す。

---

## Linux (Wayland)

設定はコンポジタによって異なる。

### 汎用的な方法（環境変数）

一部のコンポジタは `XKB_DEFAULT_OPTIONS` を参照する。シェルのプロファイルに以下を追加する。

```bash
export XKB_DEFAULT_OPTIONS="caps:ctrl_modifier"
```

元に戻すには、この行を削除してセッションを再起動する。

### Hyprland

`$HOME/.config/hypr/hyprland.conf` に以下を追加する。

```conf
input {
    kb_options = "caps:ctrl_modifier"
}
```

#### 確認方法

```bash
hyprctl getoption input:kb_options
```

#### 元に戻す

`kb_options` の行を削除（または `""` に設定）して `hyprctl reload` を実行する。
