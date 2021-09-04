# Description

For more information, see https://jcuenod.github.io/bibletech/2021/08/16/goaccess-analytics/

## Systemd Timer/Service

**Note**: I had originally used the `--user` flag for this timer but this fails because it relies on the user being logged into an active session. Therefore, you need to use `system`. This used to fail to find a config file but [this issue](https://github.com/allinurl/goaccess/pull/1850) resolves it.

Thus, the `analytics.timer` and `analytics.service` files should not be located in `systemd/user/` but:

- `/etc/systemd/system/analytics.timer`
- `/etc/systemd/system/analytics.service`

Now start the service (which runs hourly—this can be changed by editing the `OnCalendar` variable in the `analytics.timer` file). No `--user` flag.

```
systemctl enable analytics.timer
systemctl start analytics.timer
```

The `analytics.service` has an output path in the `ExecStart` variable. I point that to a path whose files are served statically so that I can look up recent analytics on my server (e.g. `/var/www/`).

## GoAccess Settings

The service executes goaccess, which draws its settings from `/etc/goaccess.conf`. The file is not complicated but a number of settings are worth paying attention to especially the last section entitled "Independently Added". Also, look for settings about the geolite mmdb database.
