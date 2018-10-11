<!--- @file
  13.6 Global PCD Section

  Copyright (c) 2008-2018, Intel Corporation. All rights reserved.<BR>

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

## 13.6 Global PCD Section

This section contains the information for all PCDs that used for all modules in
a platform. The content of global PCD section is grouped by token space:

```
gEfiMdeModulePkgTokenSpaceGuid
...
...
gEfiNt32PkgTokenSpaceGuid
...
...
...
```

PCD values derived from expressions or other PCDs are not differentiated in the
report. Only the final value is displayed.

The section header is:
```
>===============================================================================<
Platform Configuration Database Report
  *B  - PCD override in the build option
  *P  - Platform scoped PCD override in DSC file
  *F  - Platform scoped PCD override in FDF file
  *M  - Module scoped PCD override
=================================================================================
.. (List of PCDs)
<===============================================================================>
```

Each global PCD item contains one or more lines:

### 13.6.1 Required line

The first line is required:

`[*P|*F|*B] <PcdCName>: <PcdType> (<DatumType>) [(<SKUID>)][(<DefaultStore>)] = <PcdValue>`

**Note: ** If the Pcd is a Structure PCD, `<DatumType>` is the Struct Name.

#### Examples

```
*P PcdWinNtFirmwareVolume               : FIXED   (VOID*) = L"..\\Fv\\Nt32.fd"
*F PcdWinNtFlashNvStorageFtwWorkingBase : FIXED   (UINT32) = 0x0028E000 (2678784)
                                                  DEC DEFAULT = 0x0 (0)

gTokenSpaceGuid
*B LogEnable                            : FIXED   (UNIT32) = 0x1 (1)
                                                  DEC DEFAULT = 0x0 (0)
*P TestDynamic                          :  DYN    (VOID*) (DEFAULT) = L"COM1!COM2"
                                        :  DYN    (VOID*) (SKU1)    = L"COM3!COM4"
                                        :  DYN    (VOID*) (SKU2)    = L"COM5!COM6"
                                                  DEC DEFAULT = L"COM1!COM0"
```

### 13.6.2 Optional lines

#### 13.6.2.1 Dynamic/DynamicEx

* if `<PcdType>` is DYN-HII

`<VariableGuid>:<VariableName>:<Offset>`

#### Example

```
*P PcdGMchDvmtTotalSize : DYN-HII (UINT8) = 0x0 (0)
                          gSysConfigGuid: L"Setup": 0xAA
```

* if `<PcdType>` is DYN-VPD

`<Offset relative to VPD base address>`

#### Example

```
*F PcdVpdSample : DYN-VPD (UINT32) = 0x1 (1)
                  0x0001FFF
```

#### 13.6.2.2 Default (optional) line

The second optional line is present if the value from the DSC was overridden
by build option. It is formatted as follows:

`DSC DEFAULT = <Value in PCD Section in DSC>`

The third optional line is present if the value from the DEC was overridden.
It is formatted as follows:

`DEC DEFAULT = <Value in DEC>`

#### Example

```
*P PcdWinNtFirmwareFdSize   : FIXED (UINT32) = 0x2a0000 (2752512)
                              DEC DEFAULT = 0x0 (0)
```

#### 13.6.2.3 Additional optional lines

Additional lines are optional and show if the PCD's value was obtained from the
INF file or DSC file components module scoped PCD section. This will be listed
if the module's final PCD value is not the same as the first line.

*M means the PCD's value was obtained from the INF file or DSC file components
module scoped PCD section.

These lines are formatted as:

`*M Inf Filename = <Value>`

#### Example

```
*P PcdDebugPrintErrorLevel : PATCH (UINT32) = 0x80000042 (2147483714)
                                DEC DEFAULT = 0x80000000 (2147483648)
*M     Tcp4Dxe.inf                          = 0x0 (0)
```

**********
**Note:** Global PCD section is present when **PCD** is specified in **-Y**
option.
**********

#### 13.6.2.4 Rules for Structure PCD
If the Pcd is a Structure Pcd, it display in the report file would have two parts.
One is the final Pcd value and its field value, the field value is from FDF file,
DSC file PCD Section or build option, not from the final structure byte array,
and the field order is sorted by the field name. When the field value from DSC file
PCD Section, it will not have additional `*P` Flag, when the field value from FDF file,
it will have additional `*F` Flag before the field name, when the field value from
build option, it will have additional `*B` Flag before the field name. The other part
is the DEC default value and its field value, this part is optional. The field value
in this part is from DEC file, and the field order is sorted by the field name.

**********
**Note:** When the structure Pcd have some value from build option, no matter whether
the value is same with FDF or DSC file or DEC file, there will display a `*B` Flag before
the PCD name. When the structure Pcd have some value from FDF file, no matter whether
the value is same with DSC file or DEC file, there will display a `*F` Flag before the
PCD name. When the structure Pcd value have some value from DSC file PCD section, no
matter whether the value is same with DEC file, there will display a `*P` Flag before
the PCD name.
**********

#### Example

```
gEfiMdePkgTokenSpaceGuid
*B TestDynamicExHii               : DEXHII    (TEST) (SKU1) (STANDARD) = {
    0xff,0x01,0x00,0x2e,0xf6,0x08,0x6f,0x19,0x5c,0x8e,0x49,0x91,0x57,0x00,0x00,0x00,
    0x00,0x64,0x00,0x00,0x00}
           .A             = 0x1
           .Array         = {0x2e,0xf6,0x08,0x6f,0x19,0x5c,0x8e,0x49,0x91,0x57}
       *B  .C             = 0x0
           .D             = 0x64
                                  : DEXHII    (TEST) (SKU1) (Manufacturing) = {
    0xff,0x02,0x00,0x2e,0xf6,0x08,0x6f,0x20,0x5c,0x8e,0x49,0x91,0x57,0x00,0x00,0x00,
    0x00,0x68,0x00,0x00,0x00}
           .A             = 0x2
           .Array         = {0x2e,0xf6,0x08,0x6f,0x20,0x5c,0x8e,0x49,0x91,0x57}
       *B  .C             = 0x0
           .D             = 0x68
                                        DEC DEFAULT = {0xFF,0xFF}
           .A             = 0xF
           .C             = 0xF
*P TestFix                        :  FIXED   (TEST) = {
    0xff,0x02,0x00,0x2e,0xf6,0x08,0x6f,0x19,0x5c,0x8e,0x49,0x91,0x57,0x00,0x00,0x00,
    0x00,0x64,0x00,0x00,0x00}
           .A             = 0x2
           .Array         = {0x2e,0xf6,0x08,0x6f,0x19,0x5c,0x8e,0x49,0x91,0x57}
           .C             = 0x0
           .D             = 0x64
                                        DEC DEFAULT = {0xFF,0xFF}
           .A             = 0xF
           .C             = 0xF
```
