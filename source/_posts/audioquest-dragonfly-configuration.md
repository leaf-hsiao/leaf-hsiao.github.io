---
title: 吹一波AudioQuest
date: 2017-09-03 12:01:57
tags:
categories:
---

被种草AudioQuest很久了，再加上等了一整年七彩虹也没放出USB-DAC的驱动，估摸着可能永久性鸽了，还是买了个DragonFly  Black试了试，在iPhone上尝试了一下效果提升了一耳朵，然后又被安利了JitterBug，感觉可以购买AudioQuest全家桶了。

![](/images/audioquest/aq1.jpg)

<!--more-->

# JitterBug

>JitterBug is designed to remove unwanted noise currents and parasitic resonances from both the data (communication) and Vbus (power) lines of USB ports. 

使用方式就是在你的DAC和电脑中间加入一个JitterBug，如果你的设备有多个USB口的话可以再接入一个JitterBug，音质能有进一步改善（当然也请不要再接入第三个JitterBug了，并不是越多越好。

如果有其他设备连接电脑可以通过空余的JitterBug接入

![](/images/audioquest/jitterbug.png)

如果将将移动设备接入到支持USB输入口的播放器上时也可以在中间接入JitterBug，如果使用的是储存卡或者U盘之类的，也建议接入JitterBug。

PS：JitterBug不会耗电

# DragonFly Black 

从2012年诞生开始，斩获了无数HI-FI大奖，低廉的价格但是有着不俗的音质。

> Beautiful music from computers and mobile devices.

Black是DragonFly Series中的平民款，相比Red，Black的output为1.2v，而Red为2.1v，volume control不同，以及分别是ESS 9010和ESS9016。虽然看完德仪退休老员工[Bruce Trump](https://www.ece.iastate.edu/the-department/external-advisory-council/bruce-trump/)的[文章](http://www.edn.com/electronics-blogs/the-signal/4415482/Slew-Rate-the-op-amp-speed-limit)之后还是十分懵（谁推荐我看的(′д｀ )…彡…彡,唔看得懂的当我没说。再贴一篇[相关文章](http://e2e.ti.com/blogs_/b/analogwire/archive/2013/08/26/dac-essentials-understanding-your-dac-39-s-speed-limit)。纠结清楚（其实并不清楚）了之后还是选择价格便宜一半的Black。可以看一下[DragonFly Model Comparison ](http://www.audioquest.com/wp-content/uploads/2016/04/dragonfly-spec-sheet-darktheme.pdf)详细比对。

## Features

U盘大小，便宜的价格，直推耳机，支持大部分主流桌面和移动设备（天地良心没被抛弃的WinRT），可以直接再生任何类型的音频文件而不用考虑解析度之类的（反正就是能提升一耳朵听感，最重要的是~~价格低廉~~，一百刀能有这么好的音质和推力还有什么不满意的

## Configuration

手头只有IOS和Win10，其实大都在官网上有[说明](http://www.audioquest.com/dragonfly-series/#downloads)了（当个搬运工把XD

在桌面系统下安装[AQ Desktop App](http://www.audioquest.com/digitalupdates)更新到最新固件，然后开始使用

### Android&IOS

Android需要Jelly Bean以上，硬件也需要兼容，[USB Host Check](https://play.google.com/store/apps/details?id=org.tauruslabs.usbhostcheck)可以检测是否支持，更多信息参考[该文章](http://www.extreamsd.com/index.php/technology/usb-audio-driver)，同时也推荐使用他家的[UAPP](https://play.google.com/store/apps/details?id=com.extreamsd.usbaudioplayerpro)在Android上播放。

IOS使用的话官方建议使用[Lightning to USB 3 Camera Adapter](https://www.apple.com/shop/product/MK0W2AM/A/lightning-to-usb-3-camera-adapter)转换，比起[2.0](https://www.apple.com/shop/product/MD821AM/A/lightning-to-usb-camera-adapter)听感要好很多，稳定性也更好。

### Windows下的使用

将DragonFly设为默认播放设备

![](/images/audioquest/capture1.png)

关闭所有音效

![](/images/audioquest/capture2.png)

选择24bit/44100.0 Hz采样率，并勾选两个独占模式

![](/images/audioquest/capture3.png)

关于采样率直接的复制一下官方说明

>CDs操作系統的採樣率是 44100.0 Hz。壓縮的 MP3和AAC 音樂檔案和音頻流通常有三種不同層次的音質來編碼 - 128kbps, 256kbps (iTunes Plus), 和 320kbps — 然後大都再生為44100.0 Hz的文件。與此相似的，許多轉檔的 Apple Lossless 和 FLAC的下載音樂和CD音樂檔案也都再生為 44100.0 Hz的檔案。如果您有更高分辨率的檔案，選擇正確的較高採樣率是很重要的，這樣才能充分體現此音樂檔案的優點。讓高於 DragonFly Black的 96kHz 上限的採樣率檔案展現更好的音質，需要使用與其本身分辨率倍數相同的採樣率 。例如，一個192kHz 的檔案應使用 96kHz 來播放。 (i.e., 2 x 96000.0 = 192000.0).
>
>有些程序 (如 NPR) 使用 48000.0 Hz。 這些 24-bit/48000.0 Hz 檔案的音質表現也能十分接近比其更高採樣率的音樂檔案 。有些 “高分辨率”檔案使用 88200.0 Hz 因為這是 CD標準採樣率的整數倍。(i.e., 2 x 44100.0 = 88200.0). 還有些 “高分辨率”檔案使用 96000.0 Hz 因為這是 DVD、藍光播放器和電腦所用採樣率的整數倍。(i.e., 2 x 48000.0 = 96000.0).

[进阶参考](http://www.audioquest.com/pdfs/Computer-Audio-Demystified-WhitePaper-EN-R2.pdf)（还是很重要的）

机身上的LED灯代表了当前状况。

- 红色: 待机
- 绿色: 44100.0 Hz
- 蓝色: 48000.0 Hz
- 琥珀色: 88200.0 Hz
- 紫红色: 96000.0 Hz

针对iTunes, Roon, JRiver, foobar2000, Pure Music, Decibel,和Bit Perfect等可以通过进一步设置提升~~玄学~~听感，[Computer Audio Setup Guide](http://www.audioquest.com/pdfs/CA-Setup-Guide.pdf)中给出了ITunes和JRiver的详细设置方案。

#### ITunes

在导入设置里导入使用选择AIFF或者Apple Lossless，设置为自动，勾选开启纠错功能。

![](/images/audioquest/capture4.png)

不要开启均衡器

![](/images/audioquest/capture5.png)

Enjoy ITunes.

#### Foobar2000

比起体验极其糟糕的ITunes，个人还是偏爱Foobar2000

需要[WASAPI output](https://www.foobar2000.org/components/view/foo_out_wasapi)支持

Output Device选择WASAPI，format使用24bit

![](/images/audioquest/capture6.png)

然后再根据个人喜好调♂教DSP

最后

![](/images/audioquest/aq2.jpg)