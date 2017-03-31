<!--- @file
  6 Quick Start

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

# 6 Quick Start

This chapter describes the build environment. Additional chapters describe how
the build system parse files, creates C files and assembles binary images into
PI compliant firmware images. The EDK II build system uses multiple threads
during the build process. The maximum number of threads that will be spawned is
controlled in the `Conf/target.txt` file. Typically, this value will be one
more than the number of cores that are on the development workstation.
Increasing the number beyond the N+1 value will not offer any performance
benefit.

**********
**Note:** Path and Filename elements within the Build Meta-Data files and
command-line arguments are case-sensitive in order to support building on UNIX
style operating systems.
**********

A build is always performed within the context of a "platform" defined in a
single workspace. Multiple platforms can be defined in any one workspace. While
some developers will not be building actual platform firmware; the platform
definition file (DSC) format is suitable for Option ROM and stand-alone
application development, as well as flexible enough to create binary
distribution code for individual modules as well as a full platform firmware
file.

One set of EDK II build tools is required on a development system. The source
code for these tools is written in either generic C or Python.

Refer to the TianoCore.org _Getting Started with EDK II_ web page for
additional information on setting up and using the EDK II build system.
