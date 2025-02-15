# btrfs-snp

Create BTRFS snapshots, manually or from cron.

## Usage

```
# btrfs-snp
Usage: btrfs-snp <dir> (<tag>) (<limit>) (<seconds>) (<destdir>)

  dir     │ create snapshot of <dir>
  tag     │ name the snapshot <tag>_<timestamp>
  limit   │ keep <limit> snapshots with this tag. 0 to disable
  seconds │ don't create snapshots before <seconds> have passed from last with this tag. 0 to disable
  destdir │ store snapshot in <destdir>, path absolute or relative to <dir>
```

## Examples 

### Manual

Snapshot of _home_

```
# btrfs-snp /home
```

Tagged snapshot of _root_

```
# btrfs-snp / preupgrade
```

Tagged snapshot of _root_, but keep maximum 10

```
# btrfs-snp / preupgrade 10
```

### Cron 

Hourly snapshot for one day, daily for one week, weekly for one month, and monthly for one year.

```
# cat > /etc/cron.hourly/btrfs-snp <<EOF
#!/bin/bash
/usr/local/sbin/$BIN /home hourly  24 3600
/usr/local/sbin/$BIN /home daily    7 86400
/usr/local/sbin/$BIN /home weekly   4 604800
/usr/local/sbin/$BIN /     weekly   4 604800
/usr/local/sbin/$BIN /home monthly 12 2592000
EOF
chmod +x /etc/cron.hourly/btrfs-snp
```

### Systemd

1. Edit and rename the example `daily-snapshot.service` file as needed and copy to `/etc/systemd/system/`.
2. Rename and copy the `daily-snapshot.timer` file to the same location.
3. Reload systemd services files: `sudo systemctl daemon-reload`
4. Start the timer: `sudo systemctl start <filename>.timer`
5. Enable the timer so that it automatically starts on boot: `sudo systemctl enable <filename>.timer`

Inspired by [btrfs-snap](https://github.com/jf647/btrfs-snap) by Birger Monsen
                                                                                                                                                                                                      
More at [ownyourbits.com](https://ownyourbits.com)
