# 🚇 ssh-tunnel

ssh port forwarding + easy - dependencies = ssh-tunnel ✨

No more command hassles.
Easily manage multiple SSH tunnels (standard or dynamic) from your CLI.

> Tested on Debian & macOS. SOCKS support only on macOS

## TL;DR - [Install](#install)

Create a port forwarding tunnel
```
$> ssh-tunnel up --host ns1.domain.com --port 5900 --name vnc
[action:up] Creating ns1.domain.com:5900 tunnel (port: 5900, name: vnc)
[action:up] Connected
```

List all active tunnels
```
$> ssh-tunnel list
List of active tunnels:
  Name: vnc [PID: 8233]
```

Stop a tunnel
```
$> ssh-tunnel down -n vnc
[action:down] Closing tunnel vnc - PID: 8233 ...
[action:down] Tunnel closed
```

Create a SOCKS forwarding tunnel
```
$> ssh-tunnel up --host ns1.domain.com --port 32000 --name socks-wifi --socks MyWiFi
[action:up] Creating ns1.domain.com:32000 tunnel (port: 32000, name: socks-wifi)
[action:up] Proxyfing connection MyWiFi through tunnel
[action:up] Connected
```

Kill all tunnels
```
$> ssh-tunnel kill-all
[action:kill-all] Killing all tunnels ...
[action:down] Closing tunnel socks-wifi - PID: 5903 ...
[action:down] Removing proxy configuration for MyWiFi
[action:down] Tunnel closed
```

Connect to a SSH server with a specific user, key and SSH port
```
# Assuming your SSH is listening on port 23
# everything after '--' will be passed as options to the ssh command
$> ssh-tunnel up -h root@ns1.domain.com -p 32000 -- -p 23 -i id_rsa_mykey
[action:up] Creating root@ns1.domain.com:32000 tunnel (port: 32000, name: 32000)
[action:up] Connected
```

Works with interactive prompt as well (password, otp, ...)
```
$> ssh-tunnel up -h root@ns1.domain.com -p 32000
[action:up] Creating root@ns1.domain.com:32000 tunnel (port: 32000, name: 32000)
Password:
[action:up] Connected
```

## Install

> **warning** - only tested on MacOS and Debian

Run this command that download the executable and voila.
```
wget -q https://raw.githubusercontent.com/romualdr/ssh-tunnel/main/ssh-tunnel && \
  ([[ ! "`sha1sum ssh-tunnel`" = "42c6b8338242f68f8537447794c379633c01e9c8  ssh-tunnel" ]] && rm ssh-tunnel) \
  || chmod +x ssh-tunnel
```

You then should have the executable file on your machine if checksum was correct. Test it:
```
$> ./ssh-tunnel help
Usage:
  ssh-tunnel [action]

actions:
  up -h <host> -p <port> (-l <local> -n <name> -s <connection>)           Create a tunnel. (local and name defaults to remote [port]) (-s: activate socks proxy on <connection>)
  down -n <name> -p <port>                                                Remove a tunnel by name or port.
  list                                                                    List all active tunnels.
  kill-all                                                                Remove all tunnels.
$>
```

## state

- SOCK functionality uses MacOS tooling for now (feel free to PR)
- multiplateform untested but should probably work - handles lsof or fuser.
- basically fits my needs for now but open for improvements