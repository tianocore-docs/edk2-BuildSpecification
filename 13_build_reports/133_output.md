<!--- @file
  13.3 Output

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

## 13.3 Output

The output is in raw text file encoded in ASCII character set so that it can be
portable to all OS environments. The text file is supposed to be organized in a
logical way for human readability and QA team's validation.

**********
**Note:** If the EXECUTION_ORDER flag is provided as the only report type and
the -y option is not provided, the tool will generate an HTML document,
Report.html in the current working directory.
**********

If any other report type is also requested, the report will be a flat text
file. If the **-y** option is provided, the report type will also be a flat text
file (even if you name the file, using **-y**, as "Report.html").

### 13.3.1 Layout

The layout of the text report file:

```
|---- Platform summary
    |----- Mixed PCD section
    |----- Global PCD section
    |----- FD section*
        |---- FD Region sub-section*
        |---- VPD PCD Data sub-section*
    |---- Module section*
        |---- Basic Information summary
        |---- PCD sub-section
        |---- Library sub-section
        |---- DEPEX sub-section
        |---- Build_flags sub-section
        |---- Notification sub-section
```

**********
**Note:** Items marked with * can occur more than once in one parent instance.
**********

### 13.3.2 Section and Sub-section Format

The output report of BRG is divided into platform and module part. Each part
may further consist of sections and sub-sections with the following rules:

1.  Each section starts with marker `>==============================<`
2.  Each section ends with marker `<==============================>`
3.  There must be a section header after each section start marker.
4.  There must a separator `==========================` to separate the section
    header and contents if the section has non-empty contents.
5.  The section contents can further be divided into one-level sub-sections.
6.  Each sub-section starts with marker
    `>-------------------------------------------<`
7.  Each sub-section ends with marker
    `<-------------------------------------------->`
8.  There must be a sub-section header after each section start marker.
9.  There must a separator `--------------------------------------------` to
    separate the section header and contents if the section has non-empty
    contents.
10. In general, each line in section will not exceed 120 characters.

#### Example

```
Platform Name:      NT32
Platform DSC Path:  s:\edk2\Nt32Pkg\Nt32Pkg.dsc
Architectures:      IA32
Tool Chain:         VS2008x86
Target:             DEBUG
Output Path:        s:\edk2\Build\NT32IA32
Build Environment:  Windows-7-6.1.7601-SP1
Build Duration:     00:01:53
Report Contents:    PCD, LIBRARY, BUILD_FLAGS, DEPEX, HASH, FLASH, FIXED_ADDRESS
>==========================================================================<
Firmware Device (FD)
FD Name:            NT32
Base Address:       0x0
Size:               0x2A0000(2688KB)
============================================================================
>--------------------------------------------------------------------------<
FD Region
Type:               FV
Base Address:       0x0
Size:               0x280000 (2560K)
FV Name:            FvRecovery (65.9% Full)
Occupied Size:      0x1A6028 (1688K)
Free Size:          0xD9FD8 (872K)
Offset         Module
---------------------------------------------------------------------------
..(List of Module in FvRecovery)
<-------------------------------------------------------------------------->
>--------------------------------------------------------------------------<
..(List of other FD region sub-section)
>==========================================================================<
```

The following sections describe these reports and sub-sections in detail.
