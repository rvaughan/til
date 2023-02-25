# Finding a Raspberry Pi

I can never remember the IP addresses of my Raspberry Pi's, but luckily you can...

If you've only got one on your network, and you haven't changed the hostname from the default then you can try the ping command:

```bash
ping raspberrypi
```

If you've got a fleet of them, or you've changed the name then you can try an nmap scan:

```bash
nmap -sn 192.168.1.0/24
```

In the output you are looking for `raspi` or `raspberrypi` as the (default) name of the device.
