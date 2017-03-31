<!--- @file
  13.4 Platform Summary

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

## 13.4 Platform Summary

Platform summary displays at the beginning of the output report, including the
following items:

* Platform Name : %Platform UI name: '`PLATFORM_NAME`' in DSC `[Defines]`
  section%
* Platform DSC Path: %Path of platform DSC file%
* Architectures : %List string of all architectures used in build%
* Tool Chain : %Tool chain string%
* Target : %Target String"
* Output Path : %Path to platform build directory%
* Build Environment : %Environment string reported by Python%
* Build Duration : %Build duration time string%
* Report Content : %List of flags the control the report content%

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
Report Contents:    PCD, LIBRARY, BUILD_FLAGS, DEPEX, FLASH, FIXED_ADDRESS
```

**********
**Note:** Platform Summary is always present and appears at the beginning of
report.
**********
