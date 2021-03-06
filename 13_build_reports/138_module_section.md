<!--- @file
  13.8 Module Section

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

## 13.8 Module Section

Module section lists all modules involved in the platform build. If the
**EXECUTION_ORDER** option is specified in **-Y** option, the module sections
are sorted according to their PEI or DXE dispatch order; otherwise the module
sections are listed according to their DSC position.

### 13.8.1 Module Section Summary

This sub-section lists the module basic information: Module name, Module Arch,
INF file path, File GUID, Size, hash value, module build time stamp, module
build time and driver type.

* Module Name : %`BASE_NAME` in INF `[Defines]` section%
* Module Arch : %Architecture of current module%
* Module INF Path : %Path of Module INF file%
* File GUID : %`FILE_GUID` in INF `[Defines]` section%
* Size : %Module EFI image size%
* SHA1 HASH : %SHA1 hash value and efi file name with \* character%
* Build Time Stamp : %The time stamp in module PE32 image% (If the time stamp is
  cleared to be zero, the build time stamp is 1970-01-01 08:00:00 UTC time.)
* Module Build Time : %The time string for this module's build%
* Driver Type : %The driver's file type code[^3] and name in firmware volume%

The following entries are options:

* UEFI Spec Version : %`UEFI_SPECIFICATION_VERSION` in INF `[Defines]` section%
* PI Spec Version : %`PI_SPECIFICATION_VERSION` in INF `[Defines]` section%
* PCI Device ID : %`PCI_DEVICE_ID` in INF `[Defines]` section%
* PCI Vendor ID : %`PCI_VENDOR_ID` in INF `[Defines]` section%
* PCI Class Code : %`PCI_CLASS_CODE` in INF `[Defines]` section%

[^3] The hex value in this field is the Firmware File Type value defined in
Volume 3 of the PI Specification (Table 3 Defined File Types).


#### Example1:

```
>==========================================================================<
Module Summary
Module Name:        SmbiosDxe
Module Arch:        X64
Module INF Path:    MdeModule\Universal\SmbiosDxe\SmbiosDxe.inf
File GUID:          F9D88642-0737-49BC-81B5-6889CD57D9EA
Size:               0x7000 (28.00K)
SHA1 HASH:          d94c3f180f25d6b562f477bc4a16b286cb66a8b6 *SmbiosDxe.efi
Build Time Stamp:   1970-01-01 08:00:00
Module Build Time:  1060ms
Driver Type:        0x7 (DRIVER)
============================================================================
... (Module Section Details for SmbiosDxe)
<==========================================================================>
```

#### Example2:

```
>==========================================================================<
Module Summary
Module Name:        EbcDxe
Module Arch:        X64
Module INF Path:    MdeModule\Universal\EbcDxe\EbcDxe.inf
File GUID:          13AC6DD0-73D0-11D4-B06B-00AA00BD6DE7
Size:               0x9000 (36.00K)
SHA1 HASH:          ff4c019345614afe5c88e7fc37219c30a07f4af4 *EbcDxe.efi
Time Stamp:         1970-01-01 08:00:00
Module Build Time:  1731ms
Driver Type:        0x7 (DRIVER)
============================================================================
... (Module Section Details for EbcDxe)

<==========================================================================>
```

### 13.8.2 Library Sub-section

This sub-section, which follows each Module Summary section, holds the
information for all libraries used in this module. If it is an EDKII style
module, it further lists its correspondent library class, library constructor
and destructor name if they exist. The library instances are sorted by the
order of their constructor calling sequence and the reverse order of their
destructor calling sequence.

* Library INF Path: Path of library instance INF file
* Class\*: The library class name of the library instance
* C\*: The library constructor if it exists
* D\*: The library destructor if it exists
* Time: The build time of this library if it exists

The items marked with \* are only available when the module is an EDKII style
module and they must be listed in the next line immediately after library
instance's INF path.

An example of the module's library instance section is shown below.

Following the subsection header, for each library instance that was linked, the
format is:

1. The first line is the INF file path of the library instance

2. {ClassName} - the name of the library class that the above INF file provides

   * If constructors are provided, for each constructor, the following content
     is inserted in the curly braces after the ClassName:
     ```
     C = ConstructorCname
     ```

   * If destructors are provided, for each destructor, the following is
     inserted in the curly braces before the closing curly brace.
     ```
     D = DestructorCname
     ```

   * Display the build time.
     ```
     Time = TimeString
     ```

#### Example:

```c
>--------------------------------------------------------------------------<
Library
---------------------------------------------------------------------------
s:\edk2\MdePkg\Library\UefiDevicePathLib\UefiDevicePathLib.inf
{DevicePathLib: Time = 643ms}
s:\edk2\MdePkg\Library\BaseLib\BaseLib.inf
{BaseLib: Time = 14702ms}
s:\edk2\MdePkg\Library\BaseMemoryLib\BaseMemoryLib.inf
{BaseMemoryLib: Time = 284ms}
s:\edk2\MdePkg\Library\UefiMemoryAllocationLib\UefiMemoryAllocationLib.inf
{MemoryAllocationLib: Time = 249ms}
s:\edk2\MdePkg\Library\UefiBootServicesTableLib\UefiBootServicesTableLib.inf
{UefiBootServicesTableLib: C = UefiBootServicesTableLibConstructor Time = 219ms}
s:\edk2\MdePkg\Library\DxePcdLib\DxePcdLib.inf
{PcdLib: C = PcdLibConstructor Time = 265ms}
s:\edk2\MdePkg\Library\UefiRuntimeServicesTableLib\UefiRuntimeServicesTableLib.inf
{UefiRuntimeServicesTableLib: C = UefiRuntimeServicesLibConstructor Time = 203ms}
s:\edk2\MdePkg\Library\BaseIoLibIntrinsic\BaseIoLibIntrinsic.inf
{IoLib: Time = 702ms}
s:\edk2\MdePkg\Library\BasePciCf8Lib\BasePciCf8Lib.inf
{PciCf8Lib: Time = 345ms}
s:\edk2\MdePkg\Library\BasePciLibCf8\BasePciLibCf8.inf
{PciLib: Time = 341ms}
s:\edk2\MdePkg\Library\BasePrintLib\BasePrintLib.inf
{PrintLib: Time = 312ms}
s:\edk2\Ich9Pkg\Library\IntelIchAcpiTimerLib\IntelIchAcpiTimerLib.inf
{TimerLib: C = IntelAcpiTimerLibConstructor Time = 282ms}
s:\edk2\MdePkg\Library\UefiLib\UefiLib.inf
{UefiLib: Time = 733ms}
s:\edk2\MdePkg\Library\BaseSynchronizationLib\BaseSynchronizationLib.inf
{SynchronizationLib: Time = 920ms}
s:\edk2\MdePkg\Library\DxeHobLib\DxeHobLib.inf
{HobLib: C = DxeHobLibConstructor Time = 218ms}
s:\edk2\MdePkg\Library\UefiDriverEntryPoint\UefiDriverEntryPoint.inf
{UefiDriverEntryPoint Time = 234ms}
s:\edk2\MdePkg\Library\UefiRuntimeLib\UefiRuntimeLib.inf
{UefiRuntimeLib: C = UefiRuntimeLibConstructor D = UefiRuntimeLibDestructor Time = 265ms}
<-------------------------------------------------------------------------->
```

**********
**Note:** This sub-section is present when **LIBRARY** is specified in **-Y**
option.
**********

### 13.8.3 PCD Sub-section

This sub-section (following the Module Summary information) holds the
information for all PCDs used in this module. The content of module PCD
sub-section is divided by token space such as:

```
gEfiMdeModulePkgTokenSpaceGuid
...
...
gEfiNt32PkgTokenSpaceGuid
...
...
...
```

Each PCD may contain up to following lines:

1. The first line is a mandatory line with the following format:

   `[*B|*F|*P|*M] <PcdCName> : <PcdType> (<DatumType>) [(<SKUID>)][(<DefaultStore>)] = <PcdValue>`

   **Note: ** If the Pcd is a Structure PCD, `<DatumType>` is the Struct Name.

   For example:
     ```
     *P PcdWinNtFirmwareVolume : FIXED (VOID*) = L"..\\Fv\\Nt32.fd"
     ```

2. The second line is the optional line
   - if `<PcdType>` is DYN-HII
     ```
     <VariableGuid>:<VariableName>:<Offset>
     ```
     For example:
     ```
     *P PcdGMchDvmtTotalSize : DYN-HII (UINT8) = 0x0 (0)
                               gSysConfigGuid: L"Setup": 0xAA
     ```
   - if `<PcdType>` is DYN-VPD
     ```
     <Offset relative to VPD base address>
     ```
     For example:
     ```
     *F PcdVpdSample : DYN-VPD (UINT32) = 0x1 (1)
                       0x0001FFF
     ```

3. The `DSC DEFAULT` `INF DEFAULT` and `DEC DEFAULT` are option if the module's
   final `<PcdValue>` is not equal to the PCD value in the PCD common section in
   DSC file, the PCD value in the module INF file and the PCD value in the DEC
   file respectively.
   ```
   DSC DEFAULT = <Value in PCD Common Section in DSC>
   INF DEFAULT = <Value in module INF>
   DEC DEFAULT = <Value in DEC>
   ```
   For example:
   ```
   *M PcdDebugPrintErrorLevel : FIXED   (UINT32) = 0x80000042 (2147483714)
                                DSC DEFAULT = 0x80000040 (2147483712)
                                DEC DEFAULT = 0x80000000 (2147483648)
   *P PcdPlatformBootTimeOut : DYNHII (UINT16) = 0xA (10)
                     gEfiGlobalVariableGuid: L"Timeout": 0x0
                                DEC DEFAULT = 0xffff (65535)
   ```

4. Additional lines may exist if the PCD is Structure PCD. Please refer to 
13.6.2.4 Rules for Structure PCD for details.

**********
**Note:** This sub-section is present when **PCD** is specified in **-Y**
option.
**********

### 13.8.4 DEPEX Sub-section

This sub-section (following the Module Summary information) holds module
dependency expression (DEPEX) information. The sub-section header holds the
module dependency expression instructions and final dependency expression. If
the module is an EDK II style module and it inherits dependency from one of its
library instance, it lists the inherited library dependency expression in the
sub-section contents.

**********
**Note:** For `UEFI_DRIVER` module types, the tools may optimize the depex
to none, and therefore, a **DEPEX** report may not be output. However, some
`UEFI_DRIVER` modules may produce a DEPEX section if libraries that they have
been linked with have DEPEX sections.
**********

#### Example1:
```
>--------------------------------------------------------------------------<
Final Dependency Expression (DEPEX) Instructions
  PUSH gEfiFirmwareVolumeBlock2ProtocolGuid
  PUSH gEfiRuntimeArchProtocolGuid
  PUSH gEfiPcdProtocolGuid
  PUSH gEfiDevicePathUtilitiesProtocolGuid
  AND
  AND
  AND
  END
----------------------------------------------------------------------------
Dependency Expression (DEPEX) from INF
(gEfiFirmwareVolumeBlockProtocolGuid AND gEfiRuntimeArchProtocolGuid) AND
(gEfiPcdProtocolGuid) AND
(gEfiDevicePathUtilitiesProtocolGuid)
---------------------------------------------------------------------------
From Module INF:  gEfiFirmwareVolumeBlockProtocolGuid AND
gEfiRuntimeArchProtocolGuid
From Library INF: (gEfiPcdProtocolGuid) AND
(gEfiDevicePathUtilitiesProtocolGuid)
<-------------------------------------------------------------------------->
```

#### Example2:

```
>--------------------------------------------------------------------------<
Dependency Expression (DEPEX) Instructions
  PUSH gEfiPciRootBridgeIoProtocolGuid
  PUSH gEfiVariableArchProtocolGuid
  PUSH gEfiVariableWriteArchProtocolGuid
  PUSH gEfiMetronomeArchProtocolGuid
  PUSH gEfiRuntimeArchProtocolGuid
  PUSH gEfiHiiDatabaseProtocolGuid
  AND
  AND
  AND
  AND
  AND
  END
-----------------------------------------------------------------------
Dependency Expression (DEPEX) from DXS
EFI_PCI_ROOT_BRIDGE_IO_PROTOCOL_GUID AND EFI_VARIABLE_ARCH_PROTOCOL_GUID AND
EFI_VARIABLE_WRITE_ARCH_PROTOCOL_GUID AND EFI_METRONOME_ARCH_PROTOCOL_GUID AND
EFI_RUNTIME_ARCH_PROTOCOL_GUID AND EFI_PCI_ROOT_BRIDGE_IO_PROTOCOL_GUID AND
EFI_HII_DATABASE_PROTOCOL_GUID
<-------------------------------------------------------------------------->
```

**********
**Note:** This sub-section is present when **DEPEX** is specified in **-Y**
option.
**********

### 13.8.5 Build Flags Sub-section

This sub-section (following the Module Summary information) holds module build
flags information. The sub-section header holds the module tool chain tag and
the subsection contents list all related build flags, arranged using the tool
code and flag attributes defined in the `Conf/tools_def.txt` file.

#### Example

```
>--------------------------------------------------------------------------<
Build Flags
Tool Chain Tag: VS2008x86
----------------------------------------------------------------------------
SLINK_FLAGS =  /NOLOGO /LTCG
----------------------------------------------------------------------------
DLINK_FLAGS =  /NOLOGO /NODEFAULTLIB /IGNORE:4001 /OPT:REF /OPT:ICF=10 /MAP /
ALIGN:32 /SECTION:.xdata,D
/SECTION:.pdata,D /MACHINE:X86 /LTCG /DLL /ENTRY:$(IMAGE_ENTRY_POINT) /
SUBSYSTEM:EFI_BOOT_SERVICE_DRIVER /SAFESEH:NO
/BASE:0 /DRIVER /DEBUG /EXPORT:InitializeDriver=$(IMAGE_ENTRY_POINT) /
BASE:0x10000 /ALIGN:4096 /FILEALIGN:4096
/SUBSYSTEM:CONSOLE
----------------------------------------------------------------------------
CC_FLAGS = /nologo /c /WX /GS- /W4 /Gs32768 /D UNICODE /O1ib2 /GL /FIAutoGen.h /
EHs-c- /GR- /GF /Gy /Zi /Gm
<--------------------------------------------------------------------------->
```

**********
**Note:** This sub-section is present when `BUILDFLAGS` is specified in
`-Y` option.
**********

### 13.8.6 Fixed Address Prediction Sub-section

This sub-section (following the Module Summary information) contains module
notification function information. All the notification functions are listed
with the following triplet line by line:

`(Type, Address, Name)`

%The address type, predicted address, and function name%

The second character of the Type indicates whether the address is in Flash or
Memory.

#### Example1:

```
>----------------------------------------------------------------------<
Fixed Address Prediction
*I   Image Loading Address
*E   Entry Point Address
*N   Notification Function Address
*F   Flash Address
*M   Memory Address
*S   SMM RAM Offset
TOM  Top of Memory
Type Address        Name
-----------------------------------------------------------------------
*IF  0x00fffe6dac   (Image Base)
*EF  0x00fffe6e74   _ModuleEntryPoint
*NF  0x00fffe70b5   EndOfPeiCallback
*NF  0x00fffe83f0   MemoryDiscoveredPpiNotifyCallback
*IM  0x003ef48000   (Image Base)
*EM  0x003ef480c8   _ModuleEntryPoint
*NM  0x003ef48309   EndOfPeiSignalPpiNotifyCallback
*NM  0x003ef49644   EndOfPeiCallback
<---------------------------------------------------------------------->
```

#### Example2:

```
>----------------------------------------------------------------------<
Fixed Address Prediction
*I   Image Loading Address
*E   Entry Point Address
*N   Notification Function Address
*F   Flash Address
*M   Memory Address
*S   SMM RAM address
TOM  Top of Memory
Type Address           Name
-----------------------------------------------------------------------
*IM  TOM-0x00014000   (Image Base)
*EM  TOM-0x00013d60   _ModuleEntryPoint
*IS  TOM-0x00034000   (Image Base)
*ES  TOM-0x00033d60   _ModuleEntryPoint
<---------------------------------------------------------------------->
```

**********
**Note:** This sub-section is present when `FIXEDADDRESS` is specified in
`-Y` option.
**********
