---
title: "Gentoo 声卡错误：hdaudio hdaudioC0D0: Unable to bind the codec"
date: 2018-07-30T01:33:55+08:00
comment: true
tags: ["gentoo", "audio"]
categories: ["development"]
---

笔者在安装 Gentoo 后，发现声卡没有正常加载，用 `dmesg | grep audio` 看到有以下错误：

```
[    3.121698] hdaudio hdaudioC0D0: Unable to bind the codec
[    3.122375] hdaudio hdaudioC1D0: Unable to bind the codec
```
<!--more-->

## 内核配置

经过搜索一番，说是要开启内核的 `CONFIG_SND_HDA_CODEC_REALTEK` 设置。

```
$ cd /usr/src/linux
$ make menuconfig
```

然后输入 `/` 和 `CONFIG_SND_HDA_CODEC_REALTEK` 进行搜索，然后按序号即可找到相应的设置位置。


## 重新编译内核

修改内核配置之后，我们需要重新编译内核：

```
$ make && make modules_install
$ make install
```


## 更新 GRUB

> 笔者使用的引导加载程序是 GRUB，这一步骤请根据你实际使用的引导加载程序进行更新。


```
$ grub-mkconfig -o /boot/grub/grub.cfg
```

## 重启

紧接着我们重启，正常情况下，`dmesg` 会显示以下类似信息：

```
$ dmesg | grep audio

[    3.069111] snd_hda_codec_realtek hdaudioC1D0: autoconfig for ALC283: line_outs=1 (0x14/0x0/0x0/0x0/0x0) type:speaker
[    3.069114] snd_hda_codec_realtek hdaudioC1D0:    speaker_outs=0 (0x0/0x0/0x0/0x0/0x0)
[    3.069116] snd_hda_codec_realtek hdaudioC1D0:    hp_outs=1 (0x21/0x0/0x0/0x0/0x0)
[    3.069117] snd_hda_codec_realtek hdaudioC1D0:    mono: mono_out=0x0
[    3.069118] snd_hda_codec_realtek hdaudioC1D0:    inputs:
[    3.069120] snd_hda_codec_realtek hdaudioC1D0:      Mic=0x12
[    3.071039] snd_hda_codec_generic hdaudioC0D0: autoconfig for Generic: line_outs=0 (0x0/0x0/0x0/0x0/0x0) type:line
[    3.071041] snd_hda_codec_generic hdaudioC0D0:    speaker_outs=0 (0x0/0x0/0x0/0x0/0x0)
[    3.071043] snd_hda_codec_generic hdaudioC0D0:    hp_outs=0 (0x0/0x0/0x0/0x0/0x0)
[    3.071044] snd_hda_codec_generic hdaudioC0D0:    mono: mono_out=0x0
[    3.071045] snd_hda_codec_generic hdaudioC0D0:    dig-out=0x3/0x0
[    3.071046] snd_hda_codec_generic hdaudioC0D0:    inputs:
```
