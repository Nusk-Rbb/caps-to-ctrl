# Windows で CapsLock を Ctrl に変更する

## 設定方法

### レジストリファイルを使う方法

1. `CapslockToCtrl.reg` をダブルクリックして実行する
2. PCを再起動する

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

4. PCを再起動する
