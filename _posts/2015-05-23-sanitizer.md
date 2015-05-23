---
title: sanitizer
layout: post
category: 检测
tags: [检测]
---
{% include JB/setup %}
# sanitizer
---

可以从gcc/gcc/doc/invoke.texi中查看sanitizer的编译参数支持。

*功能* | *版本* | *含义*
:--|:--|:--
asan | GCC 4.8 | 越界检测
tsan | GCC 4.8 | 线程安全
lsan | GCC 4.9 | 泄漏检测
ubsan | GCC 4.9 | 未定义检测
msan | ? | 未初始化检测

gcc自身的sanitizer flag定义在gcc/gcc/flag-types.h中

```C
/* Different instrumentation modes.  */
enum sanitize_code {
  /* AddressSanitizer.  */
  SANITIZE_ADDRESS = 1 << 0,
  SANITIZE_USER_ADDRESS = 1 << 1,
  SANITIZE_KERNEL_ADDRESS = 1 << 2,
  /* ThreadSanitizer.  */
  SANITIZE_THREAD = 1 << 3,
  /* LeakSanitizer.  */
  SANITIZE_LEAK = 1 << 4,
  /* UndefinedBehaviorSanitizer.  */
  SANITIZE_SHIFT = 1 << 5,
  SANITIZE_DIVIDE = 1 << 6,
  SANITIZE_UNREACHABLE = 1 << 7,
  SANITIZE_VLA = 1 << 8,
  SANITIZE_NULL = 1 << 9,
  SANITIZE_RETURN = 1 << 10,
  SANITIZE_SI_OVERFLOW = 1 << 11,
  SANITIZE_BOOL = 1 << 12,
  SANITIZE_ENUM = 1 << 13,
  SANITIZE_FLOAT_DIVIDE = 1 << 14,
  SANITIZE_FLOAT_CAST = 1 << 15,
  SANITIZE_BOUNDS = 1UL << 16,
  SANITIZE_ALIGNMENT = 1UL << 17,
  SANITIZE_NONNULL_ATTRIBUTE = 1UL << 18,
  SANITIZE_RETURNS_NONNULL_ATTRIBUTE = 1UL << 19,
  SANITIZE_OBJECT_SIZE = 1UL << 20,
  SANITIZE_VPTR = 1UL << 21,
  SANITIZE_UNDEFINED = SANITIZE_SHIFT | SANITIZE_DIVIDE | SANITIZE_UNREACHABLE
		       | SANITIZE_VLA | SANITIZE_NULL | SANITIZE_RETURN
		       | SANITIZE_SI_OVERFLOW | SANITIZE_BOOL | SANITIZE_ENUM
		       | SANITIZE_BOUNDS | SANITIZE_ALIGNMENT
		       | SANITIZE_NONNULL_ATTRIBUTE
		       | SANITIZE_RETURNS_NONNULL_ATTRIBUTE
		       | SANITIZE_OBJECT_SIZE | SANITIZE_VPTR,
  SANITIZE_NONDEFAULT = SANITIZE_FLOAT_DIVIDE | SANITIZE_FLOAT_CAST
};
```

<!--break-->

# ubsan

*功能* | *版本* | *含义*
:--|:--|:--
-fsanitize=shift | GCC 4.9 | This option enables checking that the result of a shift operation is not undefined. Note that what exactly is considered undefined differs slightly between C and C++, as well as between ISO C90 and C99, etc. 
-fsanitize=integer-divide-by-zero | GCC 4.9 | Detect integer division by zero as well as INT_MIN / -1 division. 
-fsanitize=unreachable | GCC 4.9 | With this option, the compiler turns the __builtin_unreachable call into a diagnostics message call instead. When reaching the __builtin_unreachable call, the behavior is undefined. 
-fsanitize=vla-bound | GCC 4.9 | This option instructs the compiler to check that the size of a variable length array is positive. 
-fsanitize=null | GCC 4.9 | This option enables pointer checking. Particularly, the application built with this option turned on will issue an error message when it tries to dereference a NULL pointer, or if a reference (possibly an rvalue reference) is bound to a NULL pointer, or if a method is invoked on an object pointed by a NULL pointer. 
-fsanitize=return | GCC 4.9 | This option enables return statement checking. Programs built with this option turned on will issue an error message when the end of a non-void function is reached without actually returning a value. This option works in C++ only. 
-fsanitize=signed-integer-overflow | GCC 4.9 | This option enables signed integer overflow checking. We check that the result of +, *, and both unary and binary - does not overflow in the signed arithmetics. Note, integer promotion rules must be taken into account. 
-fsanitize=bounds | GCC 5.1 | This option enables instrumentation of array bounds. Various out of bounds accesses are detected. Flexible array members, flexible array member-like arrays, and initializers of variables with static storage are not instrumented. 
-fsanitize=bounds-strict | GCC 5.1+ | This option enables strict instrumentation of array bounds. Most out of bounds accesses are detected, including flexible array members and flexible array member-like arrays. Initializers of variables with static storage are not instrumented. 
-fsanitize=alignment | GCC 5.1 | This option enables checking of alignment of pointers when they are dereferenced, or when a reference is bound to insufficiently aligned target, or when a method or constructor is invoked on insufficiently aligned object. 
-fsanitize=object-size | GCC 5.1 | This option enables instrumentation of memory references using the __builtin_object_size function. Various out of bounds pointer accesses are detected. 
-fsanitize=float-divide-by-zero | GCC 5.1 | Detect floating-point division by zero. Unlike other similar options, -fsanitize=float-divide-by-zero is not enabled by -fsanitize=undefined, since floating-point division by zero can be a legitimate way of obtaining infinities and NaNs. 
-fsanitize=float-cast-overflow | GCC 5.1 | This option enables floating-point type to integer conversion checking. We check that the result of the conversion does not overflow. Unlike other similar options, -fsanitize=float-cast-overflow is not enabled by -fsanitize=undefined. This option does not work well with FE_INVALID exceptions enabled. 
-fsanitize=nonnull-attribute | GCC 5.1 | This option enables instrumentation of calls, checking whether null values are not passed to arguments marked as requiring a non-null value by the nonnull function attribute. 
-fsanitize=returns-nonnull-attribute | GCC 5.1 | This option enables instrumentation of return statements in functions marked with returns_nonnull function attribute, to detect returning of null values from such functions. 
-fsanitize=bool | GCC 5.1 | This option enables instrumentation of loads from bool. If a value other than 0/1 is loaded, a run-time error is issued. 
-fsanitize=enum | GCC 5.1 | This option enables instrumentation of loads from an enum type. If a value outside the range of values for the enum type is loaded, a run-time error is issued. 
-fsanitize=vptr | GCC 5.1 | This option enables instrumentation of C++ member function calls, member accesses and some conversions between pointers to base and derived classes, to verify the referenced object has the correct dynamic type.

-fsanitize=bounds-strict
https://github.com/gcc-mirror/gcc/commit/8cafe2834ebdcac360743afe5baea3ee00b1773e

# 4.9.2相比4.9.0

sanitizer_common/sanitizer_platform_limits_linux.cc

```
#define time_t __kernel_time_t
```

sanitizer_common/sanitizer_platform_limits_linux.cc
增加了一点__sparc__和__arch64__

# 5.1.0相比4.9
* 增加asan/asan_activation.cc/h
* 增加asan/debugging.cc
* asan/asan_interceptors.cc中memcpy实现加速，支持fork检测。

# 5.1.0+
5.1.0使用的是15年3月23日clyon提交的libsanitizer。
https://github.com/gcc-mirror/gcc/commit/2a236b316bfbf15330475fe08ea109e0313fd862

截止5月23日没有实质的修改。
只有两处修改：
* 增加RPC头文件检测 https://github.com/gcc-mirror/gcc/commit/e0965b31a3eb3e96403e46cb797d27e9ae141310
* 修改编译脚本 https://github.com/gcc-mirror/gcc/commit/f05994ca2c08ec782a08309c4d9902e30c728ac6


