## Backup tool

Linux tool for backing up data to a remote server using [rsync][rsync].

[rsync]: http://linuxcommand.org/man_pages/rsync1.html

### Setup

1. Ensure that [ssh keys] are properly set up between local and remote hosts.
2. Create a backup directory called e.g. `backup` in your home directory.
3. Copy the `backup` script to backup directory.
4. Create directories you want to backup in the backup directory. In case you have already directories in other places, moving them to backup directory and symlinking to original locations might be a convenient solution.
5. Copy sample configuration file `.backup` to backup directory and set correct remote host credentials and backup file list there.

[ssh keys]: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2

### Manual usage

The simplest way to use this tool is to manually execute backup when needed. This is done as follows:

``` 
$ cd ~/backup
$ ./backup
```

Files are restored in the following way:

```
$ cd ~/backup
$ ./backup restore
```

### Using `systemd` timer

Backups can be automated using a [systemd][systemd] timer. For example, configuration in [systemd user mode][systemd-user] is done as follows:


1. Copy systemd service and timer:

        $ cp systemd/* ~/.config/systemd/user/
        $ sudo systemctl --user enable oturpe-backup.timer

2. Edit copied `oturpe-backup.service` file and set parameters `WorkingDirectory` and `Exec` to match the backup directory.

3. Reload systemd daemon and enable timer:

        $ systemctl --user daemon-reload
        $ systemctl --user enable oturpe-backup.timer

4. Check that the timer is running:

        $ systemctl --user status oturpe-backup.timer
        ● oturpe-backup.timer - Backup personal files daily
          Loaded: loaded (/home/otto/.config/systemd/user/oturpe-backup.timer; enabled;
          Active: active (waiting) since su 2017-06-04 12:30:05 CEST; 30min ago
        
        kesä 04 12:30:05 ottovain systemd[1525]: Started Backup personal files daily

[systemd]: https://www.linux.com/learn/understanding-and-using-systemd
[systemd-user]: https://wiki.archlinux.org/index.php/Systemd/User
