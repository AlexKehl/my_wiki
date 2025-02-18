# Zerotier

Is a vpn kind of service that we use to add extra security for ssh connections

Problem is it is tough to connect with ssh to the server

## Fixes

- Try again several times

- Restart the server
```bash
sudo launchctl unload /Library/LaunchDaemons/com.zerotier.one.plist
sudo launchctl load /Library/LaunchDaemons/com.zerotier.one.plist
```

- If still no success then:

```bash
sudo route -n flush
```

And restart the network by turning off and on the wifi or by plugging the lan cable

Eventually wait for a while and try again
