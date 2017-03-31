<!--- @file
  2 Design Discussion

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

# 2 Design Discussion

This section of the document provides an overview to the build process for UEFI
and PI compliant modules. This includes existing EDK components and EDK II
modules. EDK II build tools process the following meta-data files:

* EDK II build configuration files

* EDK Component and EDK II Module (INF) Files

* EDK Library (only used by EDK Components) INF Files

* EDK II Package Declaration (DEC) Files

* EDK II Platform Description (DSC) Files

* EDK II Flash Description (FDF) Files

The meta-data file content is used to generate:

* Module specific C files, both .c and .h files

* PI compliant dependency files

* Makefiles used by third party compiler utilities

* PCI Option ROM images

* UEFI compliant image files

* Platform firmware images

* Platform update capsules

**********
**Note:** Path and Filename elements within the EDK II Meta-Data files and
command line arguments are case-sensitive in order to support building on UNIX
style operating systems.
**********
**Note:** The total path and file name length is limited by the operating
system and third party tools. It is recommended that for EDK II builds that the
project directories under a subst drive in Windows (s:/ build as an example) or
be located in either the /opt directory or in the user's /home/username
directory for Linux and OS/X.This will minimize the path lengths of filenames
for the command-line tools.
**********

#### Reference Implementation

The EDK II build system is a reference implementation. Its description starts
with chapter EDK II Build Process, after discussing the design and
architectural elements of UEFI/PI compliant files.
