<!--- @file
  13.1 Build Report Generation Options

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

## 13.1 Build Report Generation Options

The Build Report Generation (BRG) tool is part of build process to report the
following platform information after platform build ends successfully.

**_PCD Information_**

Complete platform configuration database information

**_LIBRARY Information_**

Library class and instance mapping, constructor/destructor information

**_DEPEX Information_**

Module dependency information

**_BUILD Information_**

Module build tool chain tag, specific compiler or linker options

**_FLASH Information_**

Module firmware device and firmware volume information

**_PREDICTION Information_**

The predicted dispatch order of modules (PEIMs / drivers) and their
notification invoking sequence; Also the predicted addresses of module image
loading, entry point and notification functions. Generating this report does
take a significant amount of time, more than 2x the standard build time.

**********
**Note:** The execution order prediction report output is an html file,
separate from the rest of the reports. All remaining reports are generated in a
single text file. The reports are generated in the current working directory.
**********

The information in the reports listed above is useful for platform integrators
to diagnose the platform issues in an efficient way. Integrators must specify
which reports to include in the report file.
