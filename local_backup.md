# Local Backup

It is easy to accidentally delete files in commonly used directories (Documents, Pictures)

Therefore we can make a simple local backup area, in a hidden directory that we would not be using day-to-day.

I have tried scheduled jobs like 'cron' or 'anacron' but these are (in my opinion) for always-on machines like servers and not ideal for home users.

The solution here is a backup that runs every time the user logs in.

This guarantees that the changes for the last session are backed up at the start of the next session. 

It is easy to set up and seamless from its enabling.

## Create a .service file in your user space
*this can be done in a root directory too, I just prefer it in the user space

```
vim /home/<path>/.config/systemd/user
```

## Add the following lines

```
[Unit]
Description=Backup Files

[Service]
Type=oneshot
ExecStart=rsync <path/to/file> /home/<user>/.local/share/

[Install]
WantedBy=default.target
```

## Reload the Daemon

Tell systemd to pick up the new file and enable it at login 
`--now` also runs it once immediately so you can confirm it works.

```
systemctl --user daemon-reload
systemctl --user enable --now backup.service
systemctl --user status backup.service
journalctl --user -u backup.service -b
```

*note
A `Type=oneshot` service shows `inactive (dead)` after it finishes. 
This is normal. 
Look for `Finished` in the log and check the file landed in `~/.local/share/`.
