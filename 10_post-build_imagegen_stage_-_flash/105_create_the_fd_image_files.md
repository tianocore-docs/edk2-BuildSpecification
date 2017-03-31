<!--- @file
  10.5 Create the FD image file(s)

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

## 10.5 Create the FD image file(s)

The whole FD image is described by a list of `Regions` which correspond to the
locations of different areas within the hardware flash device. Currently most
flash devices have a variable number of blocks, all of identical size. When
"burning" an image into one of these devices, only whole blocks can be burned
into the device at any one time. This puts a constraint that all layout regions
of the FD image must start on a block boundary. To accommodate future flash
parts that have variable block sizes, the layout is described by the offset
from the `BaseAddress` and the size of the section that is being described.
Since completely filling a block is not probable, part of the last block of a
region can be left empty. To ensure that no extraneous information is left in a
partial block, the block must be erased prior to burning it into the device.
Multiple devices with non-volatile memory are treated as a single device with
contiguous memory space.

Regions must be defined in ascending order and may not overlap.

Each layout region starts with a eight digit hex offset (leading "0x" required)
followed by the pipe "|" character, followed by the size of the region, also in
hex with the leading "0x" characters.

The format for an FD Layout Region is:

```ini
Offset|Size
[TokenSpaceGuidCName.PcdOffsetCName|TokenSpaceGuidCName.PcdSizeCName]
[RegionType]
```

Setting the optional PCD names in this fashion is shortcut. The two regions
listed below are identical, with the first example using the shortcut, and the
second using the long method:

```
0x000000|0x0C0000
gEfiMyTokenSpaceGuid.PcdFlashFvMainBaseAddress|gEfiMyTokenSpaceGuid.PcdFlashFvMa inSize
FV = FvMain

0x000000|0x0C0000
SET gEfiMyTokenSpaceGuid.PcdFlashFvMainBaseAddress = 0x000000
SET gEfiMyTokenSpaceGuid.PcdFlashFvMainSize = 0x0C0000
FV = FvMain
```

The shortcut method is preferred, as the user does not need to maintain the
values in two different locations.

The EDK II BaseTools support the use of expressions in the offset field and
size fields. When a PCD is used in either of these fields, the PCD must have
been set in a statement above where it is used in an expression (tools process
the file top to bottom). During the processing of the FDF file, the value of an
'offset' PCD is the offset from 0x00000000 After the processing has been
completed, the tools will adjust these 'offset' PCDs to be the absolute
address. For example:

```ini
[FD.Main]
BaseAddress = 0xFFE00000
Size        = 0x00800000
#DEFINE REGION1_SIZE = 0x1000
#DEFINE REGION2_SIZE = 0x2000

0x00000000|$(REGION1_SIZE)
gMyPlatformTSGuid.PcdRegion1Base|gMyPlatformTSGuid.PcdRegion1Size
FILE = MyPlatform/Region1Bin/Region1.bin

gMyPlatformTSGuid.PcdRegion1Base + $(REGION1_SIZE)|$(REGION2_SIZE)
gMyPlatformTSGuid.PcdRegion2Base|gMyPlatformTSGuid.PcdRegion2Size
```

In the above example, during FDF processing, the `PcdRegion1Base` is
`0x00000000`, while after the FDF file processing has been completed, the value
of the PCD, `PcdRegion1Base`, will be `0xFFE00000`.

The optional `RegionType`, if specified, must be one of the following `FV`,
`DATA`, `FILE`, `CAPSULE` or no `RegionType` at all. Not specifying the
`RegionType` implies that the region starting at the `Offset`, of length
`Size` must not be touched. This unspecified region type is typically used
for event logs that are persistent between system resets, and modified via some
other mechanism (and SMM Event Log module, for example).

EDK II FDF does not use the concept of sub-regions, which existed in EDK FDF
files.

### 10.5.1 FV Region Type

The `FV` RegionType is used as a pointer to either one of the unique FV names
that are defined in the `[FV]` section. These are files that contains a binary
FV as defined by the PI specification. The format for the `FV` RegionType is
one of the following:

`FV = $(UiFvName)`

The following is an example of `FV` region type.

```
0x000000|0x0C0000
gEfiMyTokenSpaceGuid.PcdFlashFvMainBaseAddress|gEfiMyTokenSpaceGuid.PcdFlashFvMa inSize
FV = FvMain
```

### 10.5.2 DATA Region Type

The `DATA` RegionType is a region that contains is a hex value or an array of
hex values. This data that will be loaded into the flash device, starting at
the first location pointed to by the `Offset` value. The format of the
`DATA` RegionType is:

`DATA = { <Hex Byte Data Structure> }`

The following is an example of a `DATA` region type.

```c
0x0CA000|0x002000
gEfiMyTokenSpaceGuid.PcdFlashNvStorageBase|gEfiMyTokenSpaceGuid.PcdFlashNvStorageSize
DATA = {
  0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  0x8D, 0x2B, 0xF1, 0xFF, 0x96, 0x76, 0x8B, 0x4C
}
```

This data may need to be modified based on content of the region. In order for
EFI modules to access these regions, a customized region header may be
required. Tools for creating custom header information is beyond the scope of
the standard build.

### 10.5.3 FILE Region Type

The `FILE` RegionType is a pointer to a binary file that will be loaded into
the flash device, starting at the first location pointed to by the `Offset`
value. The format of the `FILE` RegionType is:

`FILE = $(FILE_DIR)/Filename.bin`

The following is an example of the `FILE` RegionType.

```
0x0CC000|0x002000
gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchAddress|gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchSize
FILE = FV/Microcode.bin
```

### 10.5.4 INF Region Type

The `INF` RegionType is a pointer to a binary INF file that will be loaded into
the flash device, starting at the first location pointed to by the Offset
value. The format of the `INF` RegionType is:

`INF [Options] Package/BinModule.inf`

The following is an example of the `INF` RegionType.

```
0x0CC000|0x002000
gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchAddress|gEfiCpuTokenSpaceGuid.PcdCpuMicrocodePatchSize
INF MyPackage/MyMicrocode.inf
```
