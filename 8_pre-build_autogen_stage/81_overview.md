<!--- @file
  8.1 Overview

  Copyright (c) 2008-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

## 8.1 Overview

This chapter describes in detail the steps that are accomplished by the AutoGen
stage, which is the first step of building a platform or a module.

![](../media/image19.png)

###### Figure 19 EDK II AutoGen Process

The first file the build tool is looking for in AutoGen stage is `target.txt`
in directory `$(WORKSPACE)/Conf`. All the configurations in `target.txt` can be
overridden by command line options of build tool. If no platform description
file is specified in either `target.txt` and command line, the build tool will
try to find one in current directory. And if build tool finds a description
file of a module (INF file) in current directory, it will try to build just
that module only rather than building a whole platform.

Once the build tool gets what to build and how to build, it starts to parse the
platform description file (DSC). From the DSC file, the build tools will locate
the INF files for all modules and libraries, as well as other settings of the
platform (including DEC specified default values for PCDs used by modules and
libraries that do not have values specified in the DSC file).

From module description files, the build tool will find out what package
description files the module depends on. In this way, the build tool will find
out and parse all modules and packages that make up a platform.

The next thing to do in the AutoGen stage is to generate files required to
build a module. The files include: `AutoGen.h`, `AutoGen.c`, `$(BASENAME).depex`
and `Makefile`.

`AutoGen.c` and `$(BASENAME).depex` files will not be generated for library
modules, and `$(BASENAME).depex` file is generated only if there's `[Depex]`
section found in the module's INF file.

Each module found in DSC file will have a makefile generated for it. Once all
of the makefiles have been generated, the build tool will call **nmake** (or
**make**) for each module's `Makefile`.

**********
**Note:** When building a module, only the module's makefile will be called.
**********
