<!--- @file
  12.5 Platform Specific ASL Tools

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

## 12.5 Platform Specific ASL Tools

The platform ACPI compilers are not all backward compatible. Typically, an ASL
compiler is selected based on the ACPI version and features that are required
by the platform. Different flags may also be required for different releases of
the ASL compilers. One method for using different versions of the ASL compilers
on Windows* systems is presented here.

The EDK II build tools expect the Microsoft ASL compilers (asl.exe) and the
Intel/ACPI compiler (iasl.exe) to be located in the C:\ASL directory of the
developer's workstation. This path and the compiler names are also coded in the
`tools_def.txt` file. Name the compiler binaries using the ACPI Spec compliance
value, for example, `C:\ASL\iasl3a.exe` and `C:\ASL\iasl5.exe`.

Use the `[BuildOptions]` section of the platform's DSC file to override the
default values in the `tools_def.txt` file as shown below.

#### Platform1 DSC requiring ACPI 3a compliance

```
[BuildOptions]
*_*_*_ASL_PATH == C:\ASL\iasl3a.exe
```

#### Platform2 DSC requiring ACPI 5 compliance

```
[BuildOptions]
*_*_*_ASL_PATH == C:\ASL\iasl5.exe
*_*_*_ASL_FLAGS = -cr
```

The "==" means replace the ASL compiler specified by the `PATH` attribute in
the `tools_def.txt` file with this ASL compiler.

The "=" means to append the flags to the flags specified in the `FLAGS`
attribute in the `tools_def.txt` file.

No changes are required to any other files or tools in order for this method to
work. Other tools may also benefit from this build system flexibility.
