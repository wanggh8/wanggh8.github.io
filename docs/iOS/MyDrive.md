---
title: MyDrive
date: 2020-10-25
updated: 2020-10-25 
tags: 
  - iOS
categories: iOS
description: MyDrive 
cover: /images/swift.jpg
top_img: /images/swift.jpg
typora-root-url: ../../../source
---

## 框架选型

[网易云开发者音频开发博客](http://msching.github.io)

### 音乐播放框架

#### Jukebox

> Jukebox is an iOS audio player written in Swift.
>
> [Jukebox](https://github.com/teodorpatras/Jukebox)

#### DOUAudioStreamer

> DOUAudioStreamer is a Core Audio based streaming audio player for iOS/Mac.
>
> [DOUAudioStreamer](https://github.com/douban/DOUAudioStreamer)

#### FreeStreamer

> A streaming audio player for iOS and OS X.
>
> - **CPU-friendly** design (uses 1% of CPU on average when streaming)
> - **Multiple protocols supported**: ShoutCast, standard HTTP, local files
> - **Prepared for tough network conditions**: adjustable buffer sizes, stream pre-buffering and restart on failures
> - **Metadata support**: ShoutCast metadata, IDv2 tags
> - **Local disk caching**: user only needs to stream a file once and after that it can be played from a local cache
> - **Preloading**: playback can start immediately without needing to wait for buffering
> - **Record**: support recording the stream contents to a file
> - **Access the PCM audio samples**: as an example, a visualizer is included
>
> [FreeStreamer](https://github.com/muhku/FreeStreamer)

## StreamingKit

> StreamingKit (formally Audjustable) is an audio playback and streaming library for iOS and Mac OSX. StreamingKit uses CoreAudio to decompress and playback audio (using hardware or software codecs) whilst providing a clean and simple object-oriented API.
>
> - Free OSS.
> - Simple API.
> - Easy to read source.
> - Carefully multi-threaded to provide a responsive API that won't block your UI thread nor starve the audio buffers.
> - Buffered and gapless playback between all format types.
> - Easy to implement audio data sources (Local, HTTP, AutoRecoveringHTTP DataSources are provided).
> - Easy to extend DataSource to support adaptive buffering, encryption, etc.
> - Optimised for low CPU/battery usage (0% - 1% CPU usage when streaming).
> - Optimised for linear data sources. Random access sources are required only for seeking.
> - StreamingKit 0.2.0 uses the AudioUnit API rather than the slower AudioQueues API which allows real-time interception of the raw PCM data for features such as level metering, EQ, etc.
> - Power metering
> - Inbuilt equalizer/EQ (iOS 5.0 and above, OSX 10.9 Mavericks and above) with support for dynamically changing/enabling/disabling EQ while playing.
> - Example apps for iOS and Mac OSX provided.
>
> [StreamingKit](https://github.com/tumtumtum/StreamingKit)
>
> https://zhuanlan.zhihu.com/p/28514566