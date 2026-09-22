# CapsLock to Ctrl / CapsLock を Ctrl に変更する

Setup notes for remapping CapsLock to Ctrl on Windows, macOS and Linux.

CapsLock を Ctrl として使うための設定手順集。Windows / macOS / Linux 対応。

## Docs / 手順

- **[English](en/README.md)**
- **[日本語](ja/README.md)**

## Supported / 対応環境

| OS | Method | Reboot required |
|----|--------|-----------------|
| Windows | `.reg` file / Registry Editor | Yes |
| macOS | System Settings / `hidutil` | No |
| Linux (X11) | `setxkbmap` / `xmodmap` | No |
| Linux (Wayland) | Compositor config (Hyprland etc.) | No |

## Files

- [`windows/CapslockToCtrl.reg`](windows/CapslockToCtrl.reg) — registry file for Windows

## License

[MIT](LICENSE)
