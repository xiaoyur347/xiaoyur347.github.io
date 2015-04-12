---
title: Makefile自动依赖
layout: post
---

makefile文件

# 1. 增加-MD

$(CC) $(INCDIRS) $(CFLAGS) $(RMCFLAGS) -c $<

为

$(CC) $(INCDIRS) $(CFLAGS) $(RMCFLAGS) -MD -c $<

修改define COMPILE_CPP下面的

$(CPP) $(INCDIRS) $(CPPFLAGS) $(RMCFLAGS) -c $<

为

$(CPP) $(INCDIRS) $(CPPFLAGS) $(RMCFLAGS) -MD -c $< 说明：用于生成依赖文件*.d

# 2. 依赖 .d 文件
增加DEPENDFILES = $(patsubst %.cpp,%.d,$(SOURCEFILES))说明：将.cpp的文件替换为.d

# 3. 包含.d 文件
在文件末尾增加-include $(DEPENDFILES)说明：makefile文件中包含这些.d文件，从而实现自动依赖。前面“-”必不可少，因为第一次编译时没有.d文件

# 4. 增加.d文件清理
修改clean命令为clean:
     rm -f $(TARGET) $(OBJECTFILES) $(DEPENDFILES)

# 附录
```shell
-MF FILE
    When used with -M or -MM, specifies a file to write the dependencies to.  
    If no -MF switch is given the preprocessor sends the rules to the same place it would have sent preprocessed output.

    When used with the driver options -MD or -MMD, `-MF overrides the default dependency output file.

-MD
    -MD is equivalent to -M -MF FILE, except that -E is not implied.  
    The driver determines FILE based on whether an `-o option is given.  
    If it is, the driver uses its argument but with a suffix of .d, 
    otherwise it take the basename of the input file and applies a .d suffix.

    If -MD is used in conjunction with -E, any -o switch is understood to specify the dependency output file (but *note -MF:
    dashMF.), but if used without -E, each -o is understood to specify a target object file.

    Since -E is not implied, -MD can be used to generate a dependency output file as a side-effect of the compilation process.

-MMD
    Like -MD except mention only user header files, not system header files.


-MP
    This option instructs CPP to add a phony target for each dependency other than the main file, causing each to depend on nothing.  
    These dummy rules work around errors make gives if you remove header files without updating the Makefile to match.

    This is typical output:

         test.o: test.c test.h

         test.h:

-MT TARGET
    Change the target of the rule emitted by dependency generation.  By
    default CPP takes the name of the main input file, including any
    path, deletes any file suffix such as .c, and appends the
    platforms usual object suffix.  The result is the target.

    An -MT option will set the target to be exactly the string you
    specify.  If you want multiple targets, you can specify them as a
    single argument to -MT, or use multiple -MT options.

    For example, -MT $(objpfx)foo.o might give

         $(objpfx)foo.o: foo.c 
```

