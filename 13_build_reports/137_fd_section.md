<!--- @file
  13.6 FD Section

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

## 13.6 FD Section

This section contains platform flash device information and its layout.

### 13.6.1 FD Section Header

Given that a platform may have multi-Firmware device, this section may appear
more than once in the output report. The section header lists the name of FD
and its base address and size. The contents of the section consist of one or
more FD region subsection.

The line format is: "`%-20(key)s: %(value)s`" to ensure vertical alignment.

* FD Name : %FD UI name: FD file base name%
* Base Address: %Base address for the FD image%
* Size : %Size of the FD image%

#### Example

```
>==========================================================================<
Firmware Device (FD)
FD Name:          NT32
Base Address:     0x0
Size:             0x2a0000(2688KB)
============================================================================
... (one or more FD Region Sub-section)
<==========================================================================>
```

### 13.6.2 FD Region Sub-section

This sub-section contains FD region information of platform flash device. If
the region is a firmware volume, it lists the set of modules and its space
information; otherwise, it only lists its region name, base address and size in
its sub-section header.

The line format is: "`%-20(key)s: %(values)s`" to ensure vertical alignment.

* Region Type : %The type of the FD region (FV, Data, File or None)%
* Base Address: %Base address for the FD region%
* Size : %Size of the FD region%
* FV Name*: %FV name and occupation percentage%
* Occupied Size*: %The occupied size of the FV%
* Free Size*: %The free size of the FV%

The contents of FD region sub-section contain the list:

`(Offset, Module)*: %The list offset and module INF file path in the FV%`

The items marked with \* are only available when the region type is FV.

#### Example1:

```
>--------------------------------------------------------------------------<
FD Region
Type:             FV
Base Address:     0x0
Size:             0x280000 (2560K)
FV Name:          FvRecovery (65.9% Full)
Occupied Size:    0x1A6028 (1688K)
Free Size:        0xD9FD8 (872K)
Offset     Module
----------------------------------------------------------------------------
0x00000078 PEI Apriori
0x000000D8 DXE Apriori
0x00000FE8 PeiCore (s:\edk2\MdeModulePkg\Core\Pei\PeiMain.inf)
0x0000EFE8 PcdPeim (s:\edk2\MdeModulePkg\Universal\PCD\Pei\Pcd.inf)
...(More list of offset and modules)
<-------------------------------------------------------------------------->
>--------------------------------------------------------------------------<
```

#### Example2:

```
>--------------------------------------------------------------------------<
FD Region
Type:             DATA
Base Address:     0x280000
Size:             0xc000 (48K)
<-------------------------------------------------------------------------->
>--------------------------------------------------------------------------<
FD Region
Type:             None
Base Address:     0x28C000
Size:             0x2000 (8K)
<-------------------------------------------------------------------------->
>--------------------------------------------------------------------------<
...(More list of FD regions)
```

### 13.6.3 VPD PCD Sub-section

This section lists, in Offset order, every VPD PCD specified in the DSC file.
The line format for this section is PcdName SkuId Offset PcdSize PcdValue.

* Base Address:%Base address from the start of the FD file%
* Size :%Size of the FD region%

For each PCD in this region:

* PcdName : PcdTokenSpaceGuidCname.PcdCname
* SkuId : The string name of the SkuId for this build (or DEFAULT if no SkuId
  name is defined)
* Offset : The number of bytes from the start of the FD file
* PcdSize : Number of bytes reserved for this PCD
* PcdValue : The current value of the PCD, in hex or (for `VOID*`) the byte array

**********
**Note:** There may be gaps in the address map as some PCDs may not be required
for this specific build, but may be required for other builds based on the same
DSC file.
**********

#### Example

```
>----------------------------------------------------------------------<
FD VPD Region
Base Address:     0x3BC000
Size:             0x04000 (16K)
-----------------------------------------------------------------------
gNoSuchTokenSpaceGuid.NoSuchPciSubsystemVendorId | DEFAULT | 0x003BC000 | 2 | 0x8086
gNoSuchTokenSpaceGuid.NoSuchPciSubsystemDeviceId | DEFAULT | 0x003BC002 | 2 | 0x1000
gNoSuchTokenSpaceGuid.NoSuchGigabitEthernetMac | DEFAULT | 0x003BC004 | 8 | {0x80, 0x40, 0x20, 0x10, 0x08, 0x04}
gEfiMdeModulePkgTokenSpaceGuid.PcdRsa2048Sha256PublicKeyBuffer | DEFAULT | 0x003BC01C | 32 | {0x91, 0x29, 0xc4, 0xbd, 0xea, 0x6d, 0xda, 0xb3, 0xaa, 0x6f, 0x50, 0x16, 0xfc, 0xdb, 0x4b, 0x7e, 0x3c, 0xd6, 0xdc, 0xa4, 0x7a, 0x0e, 0xdd, 0xe6, 0x15, 0x8c, 0x73, 0x96, 0xa2, 0xd4, 0xa6, 0x4d}
< ---------------------------------------------------------------------- >
```

**********
**Note:** The whole FD section is present when **FLASH** is specified in **-Y**
option.
**********
