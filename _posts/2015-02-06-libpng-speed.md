---
title: libpng speed
layout: post
category: 库
tags: [图形库]
---
{% include JB/setup %}
# libpng speed
---

For png (24 bit) photo, I tested the speed of libpng, lodepng and yspng, and got the following result.

The environment:

CPU:   MIPS STB Dual Core 1.2G

Photo: 1280x720 24bit, no transparency

Test:  Decode the photo once, and record the cost time.

RAM Test:

libpng    118ms

lodepng   246ms

yspng     452ms
