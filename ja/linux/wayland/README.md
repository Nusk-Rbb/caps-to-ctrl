# Wayland で CapsLock を Ctrl に変更する

## 設定方法

設定はコンポジタによって異なる。各コンポジタの手順はサブディレクトリを参照。

- [Hyprland](hyprland/README.md)

### 汎用的な方法（環境変数）

一部のコンポジタは `XKB_DEFAULT_OPTIONS` を参照する。シェルのプロファイルに以下を追加する:

```bash
export XKB_DEFAULT_OPTIONS="caps:ctrl_modifier"
```
