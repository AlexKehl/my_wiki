# OSX specific stuff

## Reset bluetooth

Bluetooth at os x devices seems to be pretty bad out of the stock. Sometimes it helps to reset it

```bash
sudo pkill bluetoothd
```

## Hibernate modes

3 is default but keeps powersupply for external usb on.
25 disables that, but is slow to wake up after standby.
```bash
pmset -g | grep hibernatemode
sudo pmset -a hibernatemode 25
```

## Remove input sources bubble popup

```bash
sudo defaults delete /Library/Preferences/FeatureFlags/Domain/UIKit.plist redesigned_text_cursor

defaults write kCFPreferencesAnyApplication TSMLanguageIndicatorEnabled 0
```

See [https://discussions.apple.com/thread/255161577?sortBy=rank](https://discussions.apple.com/thread/255161577?sortBy=rank)
