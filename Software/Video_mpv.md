# mpv

<!-- TOC -->

- [1. Install](#1-install)
- [2. Terminal](#2-terminal)
- [3. GUI](#3-gui)
  - [3.1. backward/forward](#31-backwardforward)
  - [3.2. Speed](#32-speed)
  - [3.3. fullscreen](#33-fullscreen)
  - [3.4. playlist](#34-playlist)
  - [3.5. volume](#35-volume)
  - [3.6. loop](#36-loop)

<!-- /TOC -->

## 1. Install

```bash
sudo apt install mpv
```

## 2. Terminal

```bash
mpv [options] [file|URL|PLAYLIST|-]
```

示例： 以随机顺序播放MP3音乐

```bash
mpv --shuffle ~/Music/*.mp3

mpv --shuffle /folder-path/*
```

## 3. GUI

### 3.1. backward/forward

较小的快进和后退

- `LEFT` and `RIGHT`: Seek backward/forward 5 seconds.
- `Sift` + `LEFT and RIGHT`: Seek backward/forward 1 seconds. 适合于较细节的镜头切换。

较大的快进和后退

- `UP` and `DOWN`: Seek forward/backward 1 minute.
- `Sift` + `UP and DOWN`: Seek backward/forward 5 seconds.

最微小的快进和后退

- `.`: Step forward. Pressing once will pause, every consecutive press will play one frame and then go into pause mode again.
- `,`: Step backward. Pressing once will pause, every consecutive press will play one frame in reverse and then go into pause mode again.

### 3.2. Speed

- `[` and `]`: Decrease/increase current playback speed by 10%.
- `{` and `}`: Halve/double current playback speed.
- `BACKSPACE`: Reset playback speed to normal.

### 3.3. fullscreen

- `f`: Toggle fullscreen

### 3.4. playlist

- `<` and `>`: Go backward/forward in the playlist.
- `ENTER`:  Go forward in the playlist.

### 3.5. volume

- `/` and `*`: Decrease/increase volume.
- `9` and `0`: Decrease/increase volume.
- `m`: Mute sound.

### 3.6. loop

- `l`: Set/clear **A-B loop points**. 用于练习听力非常好
- `L`: Toggle infinite looping.
