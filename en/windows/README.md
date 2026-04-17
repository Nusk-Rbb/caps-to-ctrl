# Capslock as Ctrl on Windows

## How to set up

### Using a registry file

1. Double-click `CapslockToCtrl.reg` to apply it
2. Restart your PC

### Manually editing the registry

1. Open Registry Editor (`regedit`)
2. Navigate to the following key

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout
```

3. Create a new binary value named `Scancode Map` with the following data

```
00 00 00 00 00 00 00 00
02 00 00 00 1d 00 3a 00
00 00 00 00
```

4. Restart your PC
