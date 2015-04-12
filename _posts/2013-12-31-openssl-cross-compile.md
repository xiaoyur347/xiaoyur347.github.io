---
title: openssl cross compile
layout: post
---

arm:

```shell
perl ./Configure linux-armv4 -march=armv7-a --cross-compile-prefix=arm-linux-
```

mips:

```shell
perl ./Configure linux-elf --cross-compile-prefix=mipsel-linux-
```

x86:

```shell
perl ./Configure linux-elf -march=pentium -m32
```

x64:

```shell
perl ./Configure linux-x86_64 -m64
```

Don't tell me to mofidy Makefile to support cross compile.