---
title: 如何用 foobar2000 播放 dsd
date: 2021-11-26 20:48:23
tags:
categories:
---

本文不讨论 PCM 和 DSD 的优劣，也不讨论 ASIO,  WASAPI, Direct Sound 哪个更好，仅对尝试用 fb2k 播放 dsd 做个配置记录。

<!--more-->

双十一叠上淘宝的折扣以及 Amex 的满减入了[拓品 DX3 Pro+](https://www.topping.audio/productinfo/711796.html) 。相中了这款甜品级的台机倒只是因为偶然间看到了 [Audio Science Review ](https://www.audiosciencereview.com/forum/index.php?threads/topping-dx3-pro-review-dac-headphone-amp.27148/) 给出的推荐。正好苦于 iPhone 的蓝牙不支持 ALAC，正好把以前的 AQ DragonFly 拿去听 Apple Music。光纤和同轴输入支持顺道也解决了 Xbox 的音频输出方案，不用再依赖显示器那孱弱的数模转换能力。

![挺香的](/images/dx3pro+/dx3pro+.jpg)

扯远了，之前用 DragonFly 时 fb2k 的 output 用的 WASAPI，DX3 Pro + 支持 DSD，便想尝试一下，同时官方给 Win10 使用了定制的 [ASIO 驱动](https://thesycon.de/eng/usb_audiodriver.shtml)，尝试配置一下，坑还挺多的。

![](/images/dx3pro+/driver.jpg)

官网随着驱动打包的是 ASIOProxy 0.7.2以及 fb2k 的两个组件 foo_output_asio 2.1.2 和 foo_input_sacd 0.7.8，但是 [ASIOProxy](https://sourceforge.net/projects/sacddecoder/files/foo_dsd_asio/) 最新的一个版本是 0.9.4，更新时间是在 16 年，而 [foo_output_asio](https://www.foobar2000.org/components/view/foo_out_asio) 2.1.2 则停留在 12 年。


直至 21 年年末，目前 [Super Audio CD Decoder](https://sourceforge.net/projects/sacddecoder/) 已发布过 [dsd_transcoder](https://sourceforge.net/projects/sacddecoder/files/dsd_transcoder/)，[foo_dsd_converter](https://sourceforge.net/projects/sacddecoder/files/foo_dsd_converter/)，[foo_dsd_asio](https://sourceforge.net/projects/sacddecoder/files/foo_dsd_asio/)，[foo_input_sacd](https://sourceforge.net/projects/sacddecoder/files/foo_input_sacd/)，[foo_out_asio+dsd](https://sourceforge.net/projects/sacddecoder/files/foo_out_asio%2Bdsd/)等插件，功能的交叉，命名的迷惑性，加上其中一些项目早已停止开发，网络上的配置说明也由于发布时间差异很大。

靠着翻发布日志+开发者的留言，一路踩坑终于配好。按照说明书的配置方法也可以，虽然版本有些老，但用起来也没什么问题。简单来说则是 **foo_input_sacd 、 foo_dsd_asio(ASIOProxy) 、 foo_output_asio(ASIO support)**，其中 ASIO support 不是由 SACD 发布。

foo_input_sacd 目前的最新版是 21 年 11 月 12 日 更新的 1.4.2 版，16 年 4 月 29日 的 0.9.7 版的 foo_input_sacd 已经移除了 direct DSD for ASIO，所以如果仅使用新版的是无法按驱动说明选择 ASIO native ，只能使用 DoP。

> Version 0.9.7 - DoP for ASIO/WASAPI/DS, direct DSD for ASIO removed.

开发者在谈及为什么 foo_input_sacd 要去除掉 native DSD 说到

> I'll see what can be done about track cutting. Does it depend on ASIO buffer length?
> Other output plugins (WASAPI, KernelStreaming, ...)  can't do Native DSD. So having DoP is natural.

>If foobar had support for DSD then "natural" should mean Native DSD. But it doesn't and DoP is the only option.

这样在依旧能使用 ASIOProxy + ASIO support 输出的情况下，也能更方便的换至 WASAPI 等。

> If your dac/soundcard supports DSD playback through ASIO driver you can set up ASIO proxy

ASIO proxy 在 16 年11月 0.9.4 修复了 DoP to native DSD 后，也停止更新， 17 年 4月发布了 dsd_transcoder(DSD Transcoder)，从某种程度上是替代了 ASIO proxy，并能在 DoP 与 DSD native 转码。

> If your dac/soundcard supports native DSD playback through ASIO driver you can set up DSD Transcoder

> foo_input_sacd plugin 1.0.x outputs only in DoP mode.  So, dsd_transcoder converts that DoP back to native DSD (when it is set  to do so) 

因此也有许多教程给出了  **foo_input_sacd 、 dsd_transcoder 、 foo_output_asio** 的配置，可供支持 native DSD 的 DAC 使用，而 transcoder 一名颇具误导性，实际上它的作用只是对 native DSD 进行 DoP 封装/解封，并不改变内容。

> "transcode" means add or remove DoP packaging. Content doesn't change.

在 21 年 2月 25 日 foo_out_asio+dsd 发布，用于替代 foo_output_asio，并且 "is capable of Native DSD playback for compatible DACs." 。对于使用上相当于是取代了 dsd_transcoder + foo_output_asio ，即只需要 **foo_input_sacd 、foo_out_asio+dsd** 两款插件即可。

而两者的差异在于 dsd_transcoder 也能将 Native DSD 封装成 DoP。

> Both transcode DoP to Native DSD. But dsd_transcoder can do Native DSD to DoP as well and works with other players.

这两者的选择的话则可以按照自己 DAC 的兼容性来选择，个人而言 foo_input_sacd 和 foo_out_asio+dsd 的方式更简单易用，况且看起来后续的维护升级更有保障。缺憾的话~~~不支持 VU meter~~~

```sh
autoproxy_output::check_dsd_stream(true) => DSD stream contains 1 chunks and 4704 samples
autoproxy_output::process_samples() => Switch to DSD playback
autoproxy_output::process_samples() => Input stream [channels: 2, samplerate: 2822400, channel_config: 0x00000003]
autoproxy_output::process_samples() => Output stream [channels: 2, samplerate: 2822400, channel_config: 0x00000003]
autoproxy_output::process_samples(channels = 2, sample_rate = 2822400, channel_config = 0x00000003)
autoproxy_output::volume_adjust(samplerate = 2822400, latency = 0.000, volume = 0.000)
autoproxy_output::check_dsd_stream(false) => DSD stream contains 4 chunks and 18816 samples
```

姑且就这么先听着🙄
