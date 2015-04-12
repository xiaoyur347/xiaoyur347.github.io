---
title: gperftools small contribution
layout: post
---

```shell
./configure CC=clang CXX="clang++ -Qunused-arguments" CXXFLAGS="-fno-builtin"
make
make check
```

DONE:

1. [link](https://code.google.com/p/gperftools/source/detail?r=43809080931127037ce6e748f37a28ce7489387d)"lowered autoconf requirement" to support ubuntu 10.04. Just one line for configure.ac.
2. [link](https://code.google.com/p/gperftools/source/detail?r=28dd85e2825af71138621a4417e6ab004631924d)"implement pc from ucontext access for mips" for compiling cpu profiler. Also one line.
3. [link](https://code.google.com/p/gperftools/source/detail?r=326990b5c30d249c3cf4688a88fc415b05494aca)"added support for dumping heap profile via signal". The idea comed from cpu profiler signal support.
4. [link](https://code.google.com/p/gperftools/source/detail?r=7c4888515ed93347d4793fc066cd6048e519a197)"add uclibc support" with some suggestion by Aliaksey.
5. [link](https://code.google.com/p/gperftools/source/detail?r=a15115271cc475509b17bf7fecbe1ac4966baf2e)"add "-finstrument-functions" support for MIPS uclibc".
6. [link](https://code.google.com/p/gperftools/source/detail?r=60b12171bc73117c0108b847bb310af095cd2778)"fix GCC version detect for platforms other than X86/X64" to support MIPS GCC 3.X.
7. [link](https://code.google.com/p/gperftools/source/detail?r=48a0d131c1aa088c6075e9c4676ee430f81d8600)"pass -fno-builtin to compiler for unittests" to support clang unittest. Since clang doesn't support -fno-builtin-{malloc/free/}, but support -fno-builtin.

TODO:

1. Eliminate all warnings for clang if possible. I have done this by adding #pragma GCC ... which I dislike.
2. Fix heap checker not detecting all leaks bug.
3. Add support to cpu profiler to auto detecting all threads.

Hope you can also make it better and better.