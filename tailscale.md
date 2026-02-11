## Tailscale

1. Download tailscale to the PLC
```sh
curl --output tailscale.tgz https://pkgs.tailscale.com/stable/tailscale_1.80.0_arm.tgz
```

2. Unzip the file
```
tar xvf tailscale.tgz
```

3. Make install directory
```
sudo mkdir /usr/local/bin
```

4. install files

```
cd ./tailscale_1.80.0_arm && sudo install -o root -g root -m 755 tailscale tailscaled /usr/local/bin
```

5. create init.d file at /etc/init.d/tailscale
```
sudo touch /etc/init.d/tailscaled && sudo chmod +x /etc/init.d/tailscaled
```

6. paste contents into the file
```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          tailscaled
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start tailscaled at boot
# Description:       A simple script to start/stop tailscaled
### END INIT INFO

DAEMON="/usr/local/bin/tailscaled"
PIDFILE="/var/run/tailscaled.pid"

start() {
    echo "Starting tailscaled..."
    start-stop-daemon --start --background --make-pidfile --pidfile $PIDFILE --exec $DAEMON -- --statedir=/var/lib/tailscale
    echo "tailscaled started."
}

stop() {
    echo "Stopping tailscaled..."
    start-stop-daemon --stop --pidfile $PIDFILE
    rm -f $PIDFILE
    echo "tailscaled stopped."
}

restart() {
    stop
    sleep 1
    start
}

case "$1" in
    start) start ;;
    stop) stop ;;
    restart) restart ;;
    *) echo "Usage: $0 {start|stop|restart}" ;;
esac

exit 0
```

7. enable tailscaled at boot
```
sudo update-rc.d tailscaled defaults
```

8. start/stop tailscaled manually
```
sudo /etc/init.d/tailscaled start

9. Turn off DNS
```
sudo tailscale up --accept-dns=false
```
sudo /etc/init.d/tailscale stop
```
