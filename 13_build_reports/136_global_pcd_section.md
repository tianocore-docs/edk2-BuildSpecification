<!--- @file
  13.5 Global PCD Section

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

## 13.5 Global PCD Section

This section contains the information for all PCDs whose values are the same
for all modules in a platform. The content of global PCD sub-section is grouped
by token space:

```
gEfiNt32PkgTokenSpaceGuid
...
...
gEfiMdeModulePkgTokenSpaceGuid
...
...
...
```

Each global PCD item contains one or more lines:

### 13.5.1 Required line

The first line is required:

`[*P|*F| ] <PcdCName>: <PcdType> (<DatumType>) = <PcdValue>`

* *P means the Pcd's value was obtained from the DSC file
* *F means the PCD's value was obtained from the FDF file.
* If no *P or *F is given, the PCD's value comes from DEC file. If the value
  obtained from either the DSC or FDF is the same as the value in the DEC, then
  neither *P nor *F will be shown in the report.

#### Examples

```
*P PcdWinNtFirmwareVolume               : FIXED   (VOID*) = L"..\\Fv\\Nt32.fd"
*F PcdWinNtFlashNvStorageFtwWorkingBase : FIXED   (UINT32) = 0x0028E000
                                                  DEC DEFAULT = 0x0
```

### 13.5.2 Optional lines

#### 13.5.2.1 Dynamic/DynamicEx

* if `<PcdType>` is DYN-HII

`<VariableGuid>:<VariableName>:<Offset>`

#### Example

```
*P PcdGMchDvmtTotalSize : DYN-HII (UINT8) = 0
                          gSysConfigGuid: L"Setup": 0xAA
```

* if `<PcdType>` is DYN-VPD

`<Offset relative to VPD base address>`

#### Example

```
*F PcdVpdSample : DYN-VPD (UINT32) = 1
                  0x0001FFF
```

#### 13.5.2.2 Default (optional) line

The second optional line is present if the value from the DEC was overridden.
It is formatted as follows:

`DEC DEFAULT = <Value in DEC>`

#### Example

```
*P PcdWinNtFirmwareFdSize   : FIXED (UINT32) = 0x2a0000
                              DEC DEFAULT = 0x0
```

#### 13.5.2.3 Additional optional lines

Additional lines are optional and show if the PCD's value was obtained from the
INF file. This will be listed if the module's final PCD value is not the same
as the first line. The value can be obtained from the INF file only if a single
module uses the PCD.

*M means the PCD's value was obtained from the INF file.

These lines are formatted as:

`*M Inf Filename = <Value>`

#### Example

```
*P PcdDebugPrintErrorLevel : PATCH (UINT32) = 0x80000042
                                DEC DEFAULT = 0x80000000
                                            = 0x80000000
*M Tcp4Dxe.inf                              = 0x0
```

**********
**Note:** Global PCD section is present when **PCD** is specified in **-Y**
option.
**********
