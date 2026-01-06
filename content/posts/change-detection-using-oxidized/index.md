---
title: "Change Detection Using Oxidized"
date: "2020-06-29"
lastmod: "2025-05-15T09:51:00.000Z"
draft: false
featuredImage: "./cover-9fdafc82.jpg"
Status: "Published"
Tags:
  [
    "Linux"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "9fdafc82-e3e6-4892-8dd7-852b07fda49c",
    "created_time": "2024-07-11T13:00:00.000Z",
    "last_edited_time": "2025-05-15T09:51:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/d11cde59-6d00-4ef9-9821-c54fa2fa52b0/banner-oxidized.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDG47UF5%2F20260106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260106T124612Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCTIWphcBTs9IRswhcgZml7bj2vwlKuHEm6NFHqgVVHPQIgHK222GAO%2F2UPRBlee%2BhkScaQunTZfdDK5SK8ucfkwOYq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDHQlksWoGtse8jNfPSrcAztpJ6UBjzriB%2FV0RZzza2Zr5mgPzJM%2Bsg%2FCcjVzoq0aEIKV6iB7a%2F8RXKzSpEOpSJVwm9wRe5IefnDdr6k1gDBQZwZus84R2COtkqc4U1CTzeQ0XA88NK5hQRQQAI%2F6ZH%2F5dk3V9SRd3uWbmgnef993DRdb%2BxUCTBZRRztuKgpRX6l%2FaUnuVZDTd9lIG9MTTJ7og6jhMzaygHNbaDiCZ%2BBG%2FXz7i8GFzoZ%2FS%2BoGvb%2BI5DMgj9%2BWZ85ir0ecTr4ciIvHLQqa1OM0BjTmvLbhqlE6Z6ADF0jsHmyP0PNDcq6247utxeQntYXkGOi2lOn0nEndrD%2BtI1WkfalKHktonm1nsveeuQjtgn8QSOTU8rX%2BTuDscBBU%2FwT4tIl%2B3f3IRYY9AweiOcINHiQdZfObt%2BnWiruO7f0mjIG0dz25fz6h6nwDAbSPPVfr3M0e91XFseeduRxme%2BYdk6eBikTOFR52c8EJqBfQvOezhBq0x58Q50lNmvkgSSdrthc5owwa0b%2BABxVsm2jNhuQ1lWSz2NtUG74WGe5aySUzi61kNS3b1MUQX816Oqd9RB8%2FXxqy5wnFRiDsdR2qimxafHhDyf5eQmlAPFgfC66HGMz8YRme%2F1OTNQce7Czgw%2B%2FPMJH988oGOqUBfLczVDsaY%2FQD7R7C6aPX40nmcfJtpsRWOfPbKA8kzJ3oFnB1XkbzwXRE2MsYqecJUxnyXqx3spA7%2FO0JDMW0%2BLIzzv49PYpKrGc0s5elhD1r%2BVsBuHRlt4sq6yVJg1hDrBcUsqI7EP8LSvCe7m3fUNgFrRk04skRjsKMMD0iTJghGVJOZeo%2FrjZYKqXQLyxeTrS0enpdidSbs%2BAwM7Hgs3iiGCCj&X-Amz-Signature=9fa7d70ab66f5c2faa096b628bfb993ab079de8e6b2ee201d0c14afcb0899b93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject",
        "expiry_time": "2026-01-06T13:46:12.469Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "data_source_id",
      "data_source_id": "235a5f88-c313-46d9-84b5-9f168a1633b7",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Change-Detection-Using-Oxidized-9fdafc82e3e648928dd7852b07fda49c",
    "public_url": null
  }
UPDATE_TIME: "2026-01-06T12:47:15.318Z"
last_edited_time: "2025-05-15T09:51:00.000Z"
EXPIRY_TIME: "2026-01-06T13:47:14.901Z"
---

Oxidized is a Linux based service which has the ability to monitor a device’s configuration, including software and hardware. Current configuration is backed up from each device and stored to a GIT repository to maintain history of changes.

The process is very simple:

1. Login to each device in the router list router.db,
1. Run Commands to get the information that will be saved
1. Clean the output
1. Commit the Changes to GIT Repository
The tool is coded in *Ruby*, and implements a Domain Specific Language (DSL) for interaction.

Finally, there is a Web based User experience included in the solution so we can get a fast overview of the world.

# Docker Container

All of the configuration for my container is hosted at the file system location `/opt/appdata/oxidized`

I will also select to execute the Web Interface for Oxidized using its default port with is `tcp:8888`

Using the follow command, we will grab the latest container version from Docker Hub, and call the container *oxidized* locally. Additionally, if the container should stop, I am providing the flag to instruct docker to always restart the service again.

```bash
sudo docker run --restart always -v /opt/appdata/oxidized:/root/.config/oxidized -p 8888:8888/tcp -t oxidized/oxidized:latest oxidized
```

## Configuration

We need a configuration file to guide Oxidized running process

```bash
vi config
```

The following is the configuration sample that I am running with

```yaml
---
username: admin
password: P@ssw0rd!
model: junos
resolve_dns: true
interval: 3600
use_syslog: false
debug: false
threads: 30
timeout: 20
retries: 3
prompt: !ruby/regexp /^([\w.@-]+[#>]\s?)$/
rest: 0.0.0.0:8888
next_adds_job: false
vars: {}
groups: {}
models: {}
pid: "/root/.config/oxidized/pid"
crash:
  directory: "/root/.config/oxidized/crashes"
  hostnames: false
stats:
  history_size: 10
input:
  default: ssh, telnet
  debug: false
  ssh:
    secure: false
  ftp:
    passive: true
  utf8_encoded: true
output:
  default: git
  file:
    directory: "/root/.config/oxidized/configs"
  git:
    single_repo: true
    user: Oxidized
    email: oxidized@email.target
    repo: "~/.config/oxidized/oxidized.git"
source:
  default: csv
  csv:
    file: ~/.config/oxidized/router.db
    delimiter: !ruby/regexp /:/
    map:
      name: 0
      ip: 1
      model: 2
      username: 3
      password: 4
model_map:
  cisco: ios
  juniper: junos
  unifiap: airos
  edgeos: edgeos
```

## Device list

The table based on the configuration we just defined, will be formatted as follows

  To populate the table, we can open the editor `vi router.db`, and then inset the following sample entries

```yaml
Bedroom1_ap:172.16.1.114:unifiap:sysadmin:P@ssw0rd!
Kitchen_ap:172.16.1.121:unifiap:sysadmin:P@ssw0rd!
Cinema_ap:172.16.1.160:unifiap:sysadmin:P@ssw0rd!
ServerRoom_ap:172.16.1.115:unifiap:sysadmin:P@ssw0rd!
Firewall:172.16.1.1:edgeos:ubnt:Sc0rp10n!
```

Now, we are ready, we have the configuration all set for this installation

## Web Interface

Launching our browser to the oxidized site hosted on `TCP 8888` renders the current status

![Image](img-9fdafc82-2020-06-30-oxid-01.png)

From here we can see all the version changes for the devices configuration

![Image](img-9fdafc82-2020-06-30-oxid-02.png)

And even select any one of these change sets, and view the changes which were applied to the configuration

![Image](img-9fdafc82-2020-06-30-oxid-03.png)

# Closing Thoughts

Now, How do you think this might work with Azure?…

