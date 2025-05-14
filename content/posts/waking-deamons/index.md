---
title: "Waking Deamons"
date: "2019-01-14"
lastmod: "2024-07-19T15:23:00.000Z"
draft: false
featuredImage: "./cover-73e34910.jpg"
Status: "Published"
Tags:
  [
    "Linux"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "73e34910-76b7-4e57-b1bc-78f6c50ed206",
    "created_time": "2024-07-11T14:00:00.000Z",
    "last_edited_time": "2024-07-19T15:23:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": {
      "type": "file",
      "file": {
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/86ccfbc2-0c47-4877-af82-0f585fc089f2/banner.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXSKFPNB%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T143510Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQDx5XfXUW7Cs54HNZZkEHFpFbZlv24weKEXpFXomO4%2F8AIhAJ1L9WyLf46%2B4agk%2Ff%2B%2BvlDNpJTeI3DgUEOYanjUG6JdKv8DCBcQABoMNjM3NDIzMTgzODA1Igwts1QFaUsqFGq7R5sq3AMJ%2F62AugFt4k5ocFo9dMlQi6Hv0Y2uJXwMCNJ%2BwaeGvwbstlU%2BtFJvpFneH8tjP57a72js2PzDYXoYG9v02g2DbE7hlR8QHcTeQq5rfRcFFSeyHRKdEF9fbFD%2Fr8IELMcvvevXSO5HsWERoZw64nvmyGyUEiI1KYx8i06x%2BV86VQLYNaFlKLFcsW%2BjAXY6ktLt6X7lNVc4nLvEsVAAE5zY2Xik3Ub9uJ2MGQ6ydm8KrDFtbCUEdgVhMjl9JedN1Vukt255BOWfSEVqQ9nVNvE2wzcVFG%2BPqwaSoW0POEspt%2BfT1jMQg5dWDqwOFo742%2BcqnUhHzgwVEgsPdwWqC%2BXy%2Fu4Gtx2qUYtkR5Ka0K58v%2FWv7P%2BblUPJixOeXhOznnR3288HAGgpoWnYV15WplQiI0eQ5sjNMC%2BcywDpvNpLhKbo5GxkIr0ZxUeKNQJULDr9i3zv52iX1QpqVq98ecmKiCjQ4K5qHyssAGDYkYyHlQr7mTTaBx74ArkXqt%2BxJWX7DObei%2BXkPKcTCCGpY94Pylrxet44oz62ycW1ZlkSWksXEIMFxV31TdsC%2FfyNe5B%2FA8ez4DM0oRgWdpfS9EiMHW2P%2Bt3vwHpwR5P7SPVunHFmxoRhUY97HJHrLzD%2FupLBBjqkAUoMNn2Db060301O%2F2Hgh6tlKDn7i5%2BCbqRfTbG7Ouxh%2F3BWJAfqPNPyl%2Bhr4TLDmRPihpYBc2OC6pzStkvROyGPcRjeFylhxqd7KJrWVd3iDo6b9TpLqv1MgDGaZMfwOsI%2FvVjvn%2FI0Dxo6xVpg6yWKLgU7F5R9vLtSy%2Bd9yI2qO0Ws6l2m7OF01EDBDnGWJrwSj1XGrsjk3m5ga9xgd7hX5u0X&X-Amz-Signature=9302b41c16c841eaef03dcbe6c819aa0e239543abc7446ee30648dae9d140ae6&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T15:35:10.689Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Waking-Deamons-73e3491076b74e57b1bc78f6c50ed206",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T14:36:50.535Z"
last_edited_time: "2024-07-19T15:23:00.000Z"
---

With a multitude of Raspberry PI’s deployed around the house, each taking a dedicated duty in ensuring that services run transparently; It is not uncommon for me to discover the initialization scripts designed to have these services auto start at boot is not working.

The content of this post is a reference for different methods which can be employed to resolve these stubborn daemons; which always are to fond of reappearing after an unplanned outage; or what is more commonly referred to as a Power Failure!

# rc.local

To start a program on your Linux distribution *(I am focusing on Raspbian running on a Raspberry Pi)* at start-up, before other services are started, we will use the file `rc.local`.

## Editing rc.local

On your Pi, using *nano* or *vi* which are installed by default, using elevated permissions trough *sudo*, we will edit the file `/etc/rc.local`:

```shell
sudo nano /etc/rc.local
```

Add commands to execute the program, using absolute path references of the file location.

The final command in the file should be `exit 0` to indicate to the OS that we are terminating without error, then save the file and exit.

```shell
## Start our Node Application
sudo node /usr/local/bin/cgateweb/index.js
exit
```

Program which are not expected to terminate, *(runs continuously in an infinite loop)* should be stated as a forked process by adding an ampersand `&` to the end of the command. Failure to address this scenario will prevent the OS from completing its boot process.

The ampersand allows the command to run in a separate process and continue booting with the main process running.

```shell
sudo node /usr/local/bin/cgateweb/index.js &
```

> Note: A script added to /etc/rc.local is added to the OS boot sequence. A bug here will prevent the OS boot sequence progressing. Recommend that the script’s output and error messages are directed to a text file for debugging.

```shell
sudo node /usr/local/bin/cgateweb/index.js & > /var/log/myservice.log 2>&1
```

# .bashrc

The `.bashrc` file executes on boot and *also* every time when a new terminal is opened, or when a new SSH connection is made.

Normally, we would spawn our program, by placing the command at the bottom of `/home/pi/.bashrc` file. The program can be aborted with ‘ctrl-c’ while it is running!

```shell
sudo nano /home/pi/.bashrc
```

Add commands to execute the program, using absolute path references of the file location.

```shell
echo Running at boot
sudo node /usr/local/bin/cgateweb/index.js &
```

The echo statement above is used to show that the commands in `.bashrc` file are executed on bootup as well as connecting to bash console.

# init.d directory

The `/etc/init.d` directory contains the scripts which are started during the boot process, and also during the shutdown or reboot process.

Create a new file in the `/etc/init.d` directory

```shell
cd /etc/init.d
sudo nano sample.py
```

With the following sample content, we define a *Linux Standard Base (LSB)* (A standard for software system structure, including the filesystem hierarchy used in the Linux operating system) *init* script.

```shell
# /etc/init.d/sample.py

### BEGIN INIT INFO
# Provides:          sample.py
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO
```

*init.d* scripts require the above runtime dependencies to be documented so that it is possible to verify the current boot order, the order the boot using these dependencies, and run boot scripts in parallel to speed up the boot process.

> LSB Init Scripts guide.

Finally, the script in the `/etc/init.d` directory should be executable, and added to the init database with the following commands:

```shell
sudo chmod +x sample.py
sudo update-rc.d sample.py defaults
```

# SystemD

`systemd` provides a standard process for controlling what programs run when a Linux system boots up.

A sample *unit* file is provided by default in the OS, located at `/lib/systemd/system/sample.service`

```shell
sudo cp /lib/systemd/system/sample.service /lib/systemd/system/my.service
sudo nano /lib/systemd/system/my.service
```

With a copy of the sample file, we define a new service called *Sample Service* and we are requesting that it is launched once the multi-user environment is available.

* ExecStart parameter specifies the command we want to run. 
* Type set to idle to ensure that the ExecStart command is run only when everything else has loaded.
> Note: Paths are absolute and define the complete location of the runtime and any input files

```shell
[Unit]
Description=My Sample Service
After=multi-user.target

[Service]
Type=idle
ExecStart=/usr/bin/node /usr/local/bin/cgateweb/index.js

[Install]
WantedBy=multi-user.target
```

In order to store the output in a log file you can change the *ExecStart* as follows:

```shell
ExecStart=/usr/bin/node /usr/local/bin/cgateweb/index.js > /var/log/myservice.log 2>&1
```

The permission on the unit file needs to be set to 644, and then we can tell *systemd* to start it during the boot sequence.

```shell
sudo chmod 644 /lib/systemd/system/my.service
sudo systemctl daemon-reload
sudo systemctl enable my.service
```

# crontab

Crontab is a table used by `cron` which is a daemon used to run specific commands at a particular time. Crontab is very flexible and can also run a program at boot or to repeat a task or program at specific times.

Create a script to bootstrap our program

```shell
sudo nano /home/pi/.scripts/myProgram.sh
```

and then add the command you wish to execute

```shell
/usr/bin/node /usr/local/bin/cgateweb/index.js > /var/log/myservice.log 2>&1
```

Now open crontab. You most likely will be required to open crontab with elevated permissions.

```shell
sudo crontab -e
```

Add a new entry at the very bottom with `@reboot` to specify that you want to run the command at boot, followed by the command. Here we want to run our bootstrap script

```plain text
@reboot sudo /home/pi/.scripts/myProgram.sh
```

Now save the file and exit.

When you restart the pi, the command will be run and we will get the output log file.

Be a bit careful with the permissions and making sure that your program runs properly before you put it on boot: you can waste a lot of time trying to figure out what went wrong!

```shell
grep cron /var/log/syslog
```

