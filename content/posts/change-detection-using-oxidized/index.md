---
title: "Change Detection Using Oxidized"
date: "2020-06-29"
lastmod: "2024-07-19T15:22:00.000Z"
draft: false
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
    "last_edited_time": "2024-07-19T15:22:00.000Z",
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
        "url": "https://prod-files-secure.s3.us-west-2.amazonaws.com/8bc3c4f0-c291-4309-a955-a5876c66b3de/d11cde59-6d00-4ef9-9821-c54fa2fa52b0/banner-oxidized.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QZT7DB5G%2F20250514%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250514T120923Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQD0fuWIrCqgak16VjFqzUDaGGPmEd6%2Bi4ug3is%2ByhZipQIgCD38fsERZMn38iecjD26PGlgJnAUmuCUgw8APF%2Fn5asq%2FwMIFRAAGgw2Mzc0MjMxODM4MDUiDFTPkIC1JSu6Rasj6ircAwEx2UfxFUzMeBTIjBHwVXoMG158vEghuXUHJdzBcdR2NCMoeAM3pn9HUKhnZEdFBi936aCFpZKkjTQlv%2BZett9gDoYawifQvGxoix6pkUiE0A5JsFzQABZRz%2FZnj4d86dpcU9p2UaiY%2B0Y3ppZsd2U0mQWTDwGlhMgIZxbpF78D9TlP86pRpYj6sROeVGfmBuIGGZX1aH2MyYnQVo%2BgeSZ6vGdDcydmNATAc%2Fsl7gqxmosUCMfEv7mGopdsyiyLHDVqBxYqrXG%2BtPVJizkob5uWnI3BavB5DPXd6WHOF3z3%2FRdi8H%2BDcoUpyqhB2U%2Fe34xub7kPCdvU8r6eZ1fnb2r2B94A56IYT%2FIMV0n%2B17jnObkksNRK5pk88Hv4YMYS1dhnPltnBBeXRZLPKsrHwf%2BGHSINqbIzQ7gYYYlLBa3s2ILaE0zgzqNzQTGRD%2BYMWPqBoK%2FptrAWWWmldIGgJT0kFVW40LNEx9GShvg8zwiX4r4wg9sdnbK7SMEcdV0GgaeA%2Fgd4%2B75oIMxSjXfIHsnDy56pVc0gmdIbYHQZ%2FYCA52IvydehKAZ6fVujGLeU6hxrz5vIithgAXxU%2Fy4O13%2F7lHqTjbrKcaziG7MpCxFHMfAceUEVA5eip94PMImGksEGOqUBA2GBoYj9u39%2FFYYpNpSzN92Q6eWWbanFnVSperWQe7xuE8GyXpPhKgJgvPxZn8sOw4jaPjIRKZf0mXMN3viNflA3myYzpdHdOIGn3g%2FlvXsMhjQ1zbyzyy0AjU42aBAj2dDDLoaHywWvQ5ov1SI4S3bUZiIi%2BJJ0AqHxz8mkysrShyhZHEh4H4MgEzDwOfxi1o0PPFx5Mmqzx6kmzCRMyghu99t0&X-Amz-Signature=3598761a4fd24aee554756fbcc495aa8647bd3fae8ebd38a2fc2647858290cde&X-Amz-SignedHeaders=host&x-id=GetObject",
        "expiry_time": "2025-05-14T13:09:23.039Z"
      }
    },
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Change-Detection-Using-Oxidized-9fdafc82e3e648928dd7852b07fda49c",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:12:21.330Z"
last_edited_time: "2024-07-19T15:22:00.000Z"
EXPIRY_TIME: "2025-05-14T13:12:19.674Z"
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

