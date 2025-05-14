---
title: "Optimizing my GL-MT3000 for UPnP Streaming with Audio Transcoding"
date: "2024-08-21"
lastmod: "2024-08-20T23:26:00.000Z"
draft: true
Categories:
  [
    "IoT"
  ]
Status: "Draft"
Tags:
  [
    "DNLA",
    "UPnP"
  ]
NOTION_METADATA:
  {
    "object": "page",
    "id": "2383ca79-0304-40dc-92aa-ed0d0112a7e9",
    "created_time": "2024-08-20T22:11:00.000Z",
    "last_edited_time": "2024-08-20T23:26:00.000Z",
    "created_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "last_edited_by": {
      "object": "user",
      "id": "550f3f90-071d-4a6c-a8de-29d1f5804ee4"
    },
    "cover": null,
    "icon": null,
    "parent": {
      "type": "database_id",
      "database_id": "4bb8f075-358d-4efe-b575-192baa1d62b9"
    },
    "archived": false,
    "properties": {},
    "url": "https://www.notion.so/Optimizing-my-GL-MT3000-for-UPnP-Streaming-with-Audio-Transcoding-2383ca79030440dc92aaed0d0112a7e9",
    "public_url": null
  }
UPDATE_TIME: "2025-05-14T12:10:52.367Z"
last_edited_time: "2024-08-20T23:26:00.000Z"
---

Recently, I tackled the challenge of configuring my GL-MT3000 router to serve as a UPnP media server with audio transcoding capabilities. Here's how I did it without overloading this nifty little device.

### Setting Up the UPnP/DLNA Server

First things first, I needed a lightweight DLNA server. After some research, I settled on `minidlna`. Here's how I got it up and running:

1. I SSH'd into my router and ran:
  ```bash
  opkg update
  opkg install minidlna
  ```
  
  1. Then, I edited /etc/config/minidlna to point to my media storage (I use an external USB drive) and enabled audio transcoding:
  ```plain text
  list media_dir '/tmp/mountd/disk1_part1'
  option enable_transcode_audio '1'
  ```
  
  1. I also made sure to enable UPnP sharing in the GL-MT3000's web interface under "Applications" > "File Sharing".
### Tackling Audio Transcoding

My Roku and iPads sometimes struggle with AC3 audio, so I needed a way to transcode it to stereo. FFmpeg to the rescue!

1. Currently, a lightweight version of FFmpeg for audio transcoding is a good candidate to install:
  ```bash
  # opkg install ffmpeg
  opkg install libffmpeg-audio-dec
  ```
  
  1. Then, I added this line to my minidlna config:
  ```plain text
  option audio_transcoding_cmd '/usr/lib/libavcodec.so.58 -i %s -ac 2 -f wav -'
  ```
  
  This setup allows `minidlna` to use `FFmpeg` for on-the-fly audio transcoding when needed.

### Optimizing for Performance

To keep my GL-MT3000 from breaking a sweat, I made a few tweaks:

1. I limited concurrent connections:
  ```plain text
  option max_connections '5'
  
  ```
  
  1. I optimized the scanning process:
  ```plain text
  option inotify '1'
  option notify_interval '600'
  ```
  
  1. I disabled thumbnail generation to save resources:
  ```plain text
  option enable_thumbnail '0'
  ```
  
  ### The Results

After applying these configurations, I'm happy to report that my GL-MT3000 is humming along nicely. I can stream to my Roku and iPads without a hitch, and when I throw an AC3 audio track at it, it transcodes to stereo without missing a beat.

Of course, I'm always looking for ways to squeeze more performance out of my setup. I've found that using wired connections where possible and storing media on a fast USB 3.0 drive makes a noticeable difference.

```bash
config minidlna 'config'
	option user 'minidlna'
	option port '8200'
	option interface 'br-lan'
	option db_dir '/var/run/minidlna'
	option inotify '1'
	option enable_tivo '0'
	option wide_links '0'
	option strict_dlna '0'
	option notify_interval '600'
	option serial '12345678'
	option model_number '1'
	option root_container '.'
	option album_art_names 'Cover.jpg/cover.jpg/AlbumArtSmall.jpg/albumartsmall.jpg/AlbumArt.jpg/albumart.jpg/Album.jpg/album.jpg/Folder.jpg/folder.jpg/Thumb.jpg/thumb.jpg'
	option friendly_name 'GL-mt3000 DLNA Server'
	option enabled '1'
  option enable_transcode_audio '1'
  option audio_transcoding_cmd '/usr/lib/libavcodec.so.58 -i %s -ac 2 -f wav -'
  option max_connections '5'
  option enable_thumbnail '0'
	list media_dir '/tmp/mountd/disk1_part1'
	option uuid '09ae49d3-b836-480c-883c-73fcb2310fec'
```

If you're  trying to set up a similar system, I hope my experience helps. Happy streaming!

