---
layout: single-column-cover
lang: en
title: "How to make the Ergo Jr robot work on a Raspberry pi 3"
description: "Changes between Rpi2 and Rpi3 to the serial port management prevents the current ergo-jr code from working on a Rpi3. This post will show you how to fix this"
cover_title: "How to make the Ergo Jr robot work on a Raspberry pi 3"
cover_image: "/assets/img/covers/rpi3-lego.jpg"
cover_size: small
cover_gradient: true
published: true
categories:
  - en
---

We've received feedback from users about [Raspberry Pi 3 issues](https://forum.poppy-project.org/t/factory-reset-problem/2651/3).

Basically, some changes between Raspberry pi 2 and 3 to the serial interface prevent the current Ergo Jr code from working on a Raspberry pi 3.
Good news, solving this is rather easy, and we prepared a working image for the Raspberry pi 3 available for [download][rpi3-image].

If you want to upgrade your existing robot or want to switch from a Raspberry pi 2 to a Raspberry pi 3, the provided instructions below show how to do this.
You should note that depending on how one has customized their configuration, the following instructions may not work.

1.  Make sure the official `raspi-config` is installed:

    ```bash
    sudo apt-get update && sudo apt-get install -y raspi-config
    ```

2.  Launch the utility `sudo raspi-config`, then enable the camera and disable the `Advanced Options > Disable shell and kernel messages on the serial connection` options.

3.  Make sure the `/boot/config.txt` file contains the `enable_uart=1` line, or add it otherwise.

4.  Replace *poppy_ergo_jr* python dependency so it looks for `/dev/ttyS0` instead of `/dev/ttyAMA0` (this is what makes communication stall):

    ```bash
    sed -i -- 's/ttyAMA0/ttyS0/g' /home/poppy/miniconda/lib/python2.7/site-packages/poppy_ergo_jr/configuration/poppy_ergo_jr.json
    ```

5. Restart the Raspberry pi, and enjoy robotics!

[rpi3-image]: https://github.com/poppy-project/poppy-ergo-jr/releases/download/1.0.0-gm/poppy-ergo-jr-2016-09-08-rpi3.img.zip