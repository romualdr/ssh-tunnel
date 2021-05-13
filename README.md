# 🚇 ssh-tunnel

macOS + ssh port forwarding + easy - dependencies = ssh-tunnel ✨

No more command hassles.
Easily manage multiple SSH tunnels (standard or dynamic) from your MacOS CLI.

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

## Install

> **warning** - only tested on MacOS

Download the executable and voila.
```
$> wget -q https://raw.githubusercontent.com/romualdr/ssh-tunnel/main/ssh-tunnel && \
   ([[ ! "`sha1sum ssh-tunnel`" = "35d35b286571877edeb453c05e3257c8c08f3310  ssh-tunnel" ]] && rm ssh-tunnel) || chmod +x ssh-tunnel
```

You should have the executable file if checksum passed
```
$> ./ssh-tunnel help
Usage:
  ssh-tunnel [action]

actions:
  up -h <host> -p <port> (-l <local> -n <name> -s <connection>)           Create a tunnel. (local, name: defaults to remote [port]) (-s: activate socks proxy on <connection>)
  down -n <name> -p <port>                                                Remove a tunnel by name or port.
  list                                                                    List all active tunnels.
  kill-all                                                                Remove all tunnels.
$>
```

## state

- works well with `.ssh/config` ssh configurations
- i.e: untested with user+password credentials ssh (feel free to PR)
- SOCK functionality uses MacOS tooling for now (feel free to PR)
- multiplateform untested
- basically fits my needs for now but open for improvements