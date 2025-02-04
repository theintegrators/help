# Logging

1. Create a logroate conf file
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
