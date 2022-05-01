# [Catena](https://github.com/alysoid/catena) Ansible Role: systemd-timesyncd

Manage a simple SNTP client via systemd using [systemd-timesyncd](https://man.archlinux.org/man/systemd-timesyncd.8), a system service that may be used to synchronize the local system clock with a remote Network Time Protocol server. It also saves the local time to disk every time the clock has been synchronized.

## Role variables

| Variable                   | Default | Info                                                                                  |
| -------------------------- | ------- | ------------------------------------------------------------------------------------- |
| `systemd_timesyncd_conf`   | `[]`    | Set `/etc/systemd/timesyncd.conf` file that control NTP network time synchronization. |
| `systemd_timesyncd_conf_d` | `[]`    | Create custom configuration files in `/etc/systemd/timesyncd.conf.d/*.conf`           |
| `timesync_cleanup`         | `no`    | Remove existing configuration files from `/etc/systemd/resolved.conf.d/*.conf`.       |

### `systemd_timesyncd_conf`

Manage [timesyncd.conf](https://man.archlinux.org/man/timesyncd.conf.5) - Network Time Synchronization main configuration file.

By default, the configuration file `/etc/systemd/timesyncd.conf` contains commented out entries showing the defaults as a guide to the administrator. This file can be compiled by this role to create local overrides:

```yaml
systemd_timesyncd_conf:
  - options:
      # Space-separated list of NTP server host names or IP addresses.
      NTP: ""
      # Space-separated list of NTP server host names or IP addresses to be used as the fallback NTP servers.
      FallbackNTP: "0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org"
      # Maximum acceptable root distance. Takes a time value (in seconds). Defaults to 5 seconds.
      RootDistanceMaxSec: "5"
      # Minimum poll intervals for NTP messages. Defaults to 32 seconds.
      # It must not be smaller than 16 seconds.
      PollIntervalMinSec: "32"
      # Maximum poll intervals for NTP messages. Defaults to 2048 seconds.
      # It must be larger than `PollIntervalMinSec`.
      PollIntervalMaxSec: "2048"
```

### `systemd_timesyncd_conf_d`

Manage [timesyncd.conf.d](https://man.archlinux.org/man/timesyncd.conf.5) - Network Time Synchronization configuration files.

Configuration files will have the .conf extension and will be placed in the configuration snippets directory `/etc/systemd/timesyncd.conf.d`.

```yaml
systemd_timesyncd_conf_d:
  # Create `/etc/systemd/resolved.conf.d/main_ntp.conf`
  - name: main_ntp
    options:
      # Space-separated list of NTP server host names or IP addresses.
      NTP: "ntp2.hetzner.de ntp1.hetzner.de ntp3.hetzner.de"
  # Create `/etc/systemd/resolved.conf.d/fallback_ntp.conf`
  - name: fallback_ntp
    options:
      # Space-separated list of NTP server host names or IP addresses to be used as the fallback NTP servers.
      FallbackNTP: "0.it.pool.ntp.org 1.it.pool.ntp.org 2.it.pool.ntp.org 3.it.pool.ntp.org"
```
