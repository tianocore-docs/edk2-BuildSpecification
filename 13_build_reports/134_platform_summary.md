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
* Target : %Target String%
* SKUID: %Platform SKUID String%
* DefaultStore: %Platform DefaultStore String%
* Output Path : %Path to platform build directory%
* Build Environment : %Environment string reported by Python%
* Build Duration : %Build duration time string%
* AutoGen Duration : %AutoGen duration time string if it exists%
* Make Duration : %Make duration time string if it exists%
* GenFds Duration : %GenFds duration time string if it exists%
* Report Content : %List of flags the control the report content%

#### Example

```
Platform Name:      NT32
Platform DSC Path:  s:\edk2\Nt32Pkg\Nt32Pkg.dsc
Architectures:      IA32
Tool Chain:         VS2008x86
Target:             DEBUG
SKUID:              DEFAULT
DefaultStore:       STANDARD
Output Path:        s:\edk2\Build\NT32IA32
Build Environment:  Windows-7-6.1.7601-SP1
Build Duration:     00:01:29
AutoGen Duration:   00:00:10
Make Duration:      00:01:02
GenFds Duration:    00:00:15
Report Contents:    PCD, LIBRARY, BUILD_FLAGS, DEPEX, FLASH, FIXED_ADDRESS
```

**********
**Note:** Platform Summary is always present and appears at the beginning of
report.
**********

If the DSC or FDF file contains conditional directive statements (`!if`,
`!elseif`, `!ifdef` or `!ifndef`) or the value of PCD is not used by a module is
set in the DSC file (PCD Sections) or the FDF file (`SET` statements for
example), the following sub-section is generated.

```
==========================================================================<
Conditional Directives used by the build system
============================================================================
```

If the DSC or FDF file define values for PCDs that are not used by any module
and are not used in conditional directive statements, the following sub-section
is generated.

```
==========================================================================<
PCDs not used by modules or in conditional directives
============================================================================
```

### 13.4.1 PCDs in Conditional Directives

If a PCD is used in a conditional directive statement, the PCD section is
generated.

PCD values derived from expressions or other PCDs are not differentiated in the
report. Only the final value is displayed.

The first line is required:

`[*P|*F|*B] <PcdCName>: <PcdType> (<DatumType>) = <PcdValue>`

* `*P` means the Pcd's value was obtained from the DSC file
* `*F` means the PCD's value was obtained from the FDF file.
* `*B` means the PCD's value set by a build option.
**Note:** If the Pcd is a Structure PCD, <DatumType> is the Struct Name.

Additional lines may be displayed showing default values when the value is not a
default value.

### Example

```
>==========================================================================<
Conditional Directives used by the build system
============================================================================
PCD statements
>--------------------------------------------------------------------------<
gMyTokenSpaceGuid
*P SmmEnable                   : FEATURE (BOOLEAN) = 0x0
                                         DEC DEFAULT = 0x1
*B LogEnable                   : FIXED   (UNIT32) = 0x1
                                         DEC DEFAULT = 0x0
<-------------------------------------------------------------------------->
>==========================================================================<
```

### 13.4.2 PCDs not used

If a PCD is not used in a conditional directive statement or by a module, the
not used PCD section is generated.

PCD values derived from expressions or other PCDs are not differentiated in the
report. Only the final value is displayed.

The first line is required:

`[*P|*F|*B] <PcdCName>: <PcdType> (<DatumType>) [(<SKUID>)][(<DefaultStore>)] = <PcdValue>`

* `*P` means the Pcd's value was obtained from the DSC file
* `*F` means the PCD's value was obtained from the FDF file.
* `*B` means the PCD's value set by a build option.
**Note:** If the Pcd is a Structure PCD, <DatumType> is the Struct Name.

Additional lines may be displayed showing default values when the value is not a
default value.

Since the PCDs in this section are not used by any module, the PCD value is not
evaluated to determine if it is a valid value or in a value in a valid range.
Instead, the PCD value from the DSC file, FDF file, or build option are
displayed exactly as they were entered.

### Example

```
>==========================================================================<
PCDs not used by modules or in conditional directives
============================================================================
PCD statements
>--------------------------------------------------------------------------<
gMyTokenSpaceGuid
*P SmmEnable                   : FEATURE (BOOLEAN) = 0x0
                                         DEC DEFAULT = 0x1
*B UsbEnable                   : FIXED   (UNIT32) = 0x1
                                         DEC DEFAULT = 0x0
<-------------------------------------------------------------------------->
>==========================================================================<
```
