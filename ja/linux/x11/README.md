# X11 で CapsLock を Ctrl に変更する

## 設定方法

### setxkbmap

1. 以下のコマンドを実行する

```bash
setxkbmap -option caps:ctrl_modifier
```

> 注意: この変更は再起動するとリセットされる。永続化するには `~/.xinitrc` や `~/.xprofile` にコマンドを追加する。

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

> 注意: 永続化するには `~/.xinitrc` や `~/.xprofile` に `xmodmap ~/.Xmodmap` を追加する。
