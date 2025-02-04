# Logging

## Configuring the log file
1. Create a logrotate conf file
```
sudo touch /etc/logrotate.d/<APPLICATION_NAME>.conf
```

2. Add contents to the conf file
```
/var/log/<APPLICATION_NAME>.log {
    weekly
    size 100M
    minsize 100M
    rotate 10
    notifempty
    missingok
    compress
    delaycompress
    copytruncate
}
```

3. Create log file
```
sudo touch /var/log/<APPLICATION_NAME>.log
```

4. Change write permissions
```
sudo chmod 666 /var/log/<APPLICATION_NAME>.log
```

## Logging to the file

Just append to the file
```
echo "my msg" >> /var/log/<APPLICATION_NAME>.log
```

## View logs

```
tail -f /var/log/<APPLICATION_NAME>.log
```
