<!--- @file
  8.7 Generated AsBuilt INF Files

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

## 8.7 Generated AsBuilt INF Files

The EDK II build system will generate an INF file for every module that is
built from source files. Comments that would be required in the INF file for
the UEFI Packaging Tool to create a distribution package must be preserved. The
AsBuilt INF file must be an ASCII formatted file with DOS end-of-line (CRLF)
characters. Portions of the AsBuilt INF are generated during pre-build, while
other portions are determined after the images have been created during the
$Make stage. Refer to the _EDK II Module Information (INF) File Specification_
for the exact format for content in these sections. AsBuilt INFs are only
created from building source modules.

### 8.7.1 Header Section

The header of the _AsBuilt_ INF file will use the same content and format as
the INF file except when a comment section that follows the source header
contains the following line:

`## @BinaryHeader`

1. If the above tag is located, then the tool must ignore the source header and
   used the Binary header block instead.

2. If using the Binary header block, the tools must replace `@BinaryHeader` with
   `@file` in the _AsBuilt_ INF.

3. The tool must insert the following four lines between the description and
   copyright line regardless of the header used to create the _AsBuilt_ INF:

```ini
#
# DO NOT EDIT
# FILE auto-generated Binary INF
#
```

**********
**Note:** The copyright date in the source INF should be updated every time a
change is made to the INF file. Since every bug fix or new feature added to the
source code requires that at least one of the `VERSION_STRING` values to be
updated, the binary header should carry the same copyright date as the source
header copyright date it was generated from.
**********
**Note:** When generating the _AsBuilt_ INF, if the source INF file contains
the Doxygen tag, `@BinaryHeader`, the content from this section (which matches
the format of the standard header) will replace the content from the standard
header. The `@BinaryHeader` tag will be replaced with the `@file` tag as the
first line of the _AsBuilt_ INF file.
**********

### 8.7.2 [Defines] Section

The following elements of the source INF will be copied into the `[Defines]`
section of the _AsBuilt_ INF file if and only if they exist in the source INF.
The `INF_VERSION` in the _AsBuilt_ INF File will be updated to match the
version number in the _EDK II INF Specification_ that was used at the time the
tool code to create the AsBuilt file was updated, even if the `INF_VERSION` in
the source INF was a lower version, such as `0x00010005` If the _EDK II INF
Specification_ version in the source INF is greater than the version embedded in
the tool, the tools should replace the version value with the version that is
embedded in the tool, lowering the value.

Macros definitions ("DEFINE" statements) are not listed in the _AsBuilt_ INF
file. Instead, the macro value (where it was used) will be expanded in the path
and value statements.

```
<TS> "[Defines]" <EOL>
<TS> "INF_VERSION" <Eq> <CurrentInfSpecificationVersion> <EOL>
<TS> "BASE_NAME" <Eq> <BaseName> <EOL>
<TS> "FILE_GUID" <Eq> <RegistryFormatGUID> <EOL>
<TS> "MODULE_TYPE" <Eq> <Edk2ModuleType> <EOL>
[<TS> "UEFI_SPECIFICATION_VERSION" <Eq> <VersionVal> <EOL>]
[<TS> "PI_SPECIFICATION_VERSION" <Eq> <VersionVal> <EOL>]
[<TS> "VERSION_STRING" <Eq> <DecimalVersion> <EOL>]
[<TS> "PCD_IS_DRIVER" <Eq> <PcdDriverType> <EOL>]
[<TS> "ENTRY_POINT" <Eq> <CName> <EOL>]*
[<TS> "UNLOAD_IMAGE" <Eq> <CName> <EOL>]*
[<TS> "CONSTRUCTOR" <Eq> <CName> <EOL>]*
[<TS> "DESTRUCTOR" <Eq> <CName> <EOL>]*
[<TS> "SHADOW" <Eq> <BoolType> <EOL>]
[<TS> "PCI_VENDOR_ID" <Eq> <UINT16> <EOL>]
[<TS> "PCI_DEVICE_ID" <Eq> <UNIT16> <EOL>]
[<TS> "PCI_CLASS_CODE" <Eq> <UINT8> <EOL>]
[<TS> "PCI_REVISION" <Eq> <UINT8> <EOL>]
[<TS> "BUILD_NUMBER" <Eq> <UINT16> <EOL>]
[<TS> "MODULE_UNI_FILE" <Eq> <Filename> <EOL>]
[<TS> "SPEC" <MTS> <Identifier> <Eq> <DecimalVersion> <EOL>]*
[<TS> "UEFI_HII_RESOURCE_SECTION" <Eq> <TrueFalse> <EOL>]
```

#### Parameters

**_MODULE_UNI_FILE_**

If the source module contains this entry, the tools must create a USC-2LE
encoded file in the module's `OUTPUT` directory, ensuring that any of the tags
that refer to BINARY content (`@BinaryHeader`) are used in place of tags that
do not contain the word `BINARY`.

**_CurrentInfSpecificationVersion_**

This is the version of the _EDK II INF Specification_ at the time the code (in the
build tools) to generate the AsBuilt INF is updated.

#### Example

```ini
[Defines]
  INF_VERSION     = 0x00010017
  BASE_NAME       = DxeCore
  MODULE_UNI_FILE = DxeCore.uni
  FILE_GUID       = D6A2CB7F-6A18-4e2f-B43B-9920A733700A
  MODULE_TYPE     = DXE_CORE
  VERSION_STRING  = 1.0

  ENTRY_POINT     = DxeMain
```

### 8.7.3 [LibraryClasses] Section

This section must list (in comments) every library instances that gets linked
with the module. A Doxygen tag, `@LIB_INSTANCES` in a comment must precede the
list of library instances.

#### Example

```ini
[LibraryClasses]
  ## @ LIB_INSTANCES
  # MdePkg/Library/BaseDebugLibSerialPort/BaseDebugLibSerialPort.inf
```

### 8.7.4 [Packages] Section

This section is required if there are PCDs listed in the `[PatchPcd]` and
`[PcdEx]`, the packages that declare the PCDs that are list must be listed
here. The format for the PCD entries is defined in the Module Information (INF)
File Specification.

#### Example

```ini
[Packages.IA32]
  MdePkg/MdePkg.dec
  MdeModulePkg/MdeModulePkg.dec
```

### 8.7.5 [Guids] Section

All GUIDs that are listed in the source INF and their usage (if available) must
be include in this section. Usage information may be modified based on feature
flag expressions that are evaluated during the build. For example, the source
INF may have a `SOMETIMES_PRODUCES` usage that may be changed to `PRODUCES` in
the _AsBuilt_ INF file if the build uses a feature flag to include the item.

#### Example

```ini
[Guids.IA32]
  ## PRODUCES ## Event
  gEfiEventMemoryMapChangeGuid

  ## CONSUMES ## UNDEFINED
  gEfiEventVirtualAddressChangeGuid

  ## CONSUMES ## UNDEFINED
  ## PRODUCES ## Event
  gEfiEventExitBootServicesGuid

  ## CONSUMES ## HOB
  gEfiHobMemoryAllocModuleGuid
```

### 8.7.6 [Protocols] Section

All Protocols that are listed in the source INF and their usage (if available)
must be include in this section. The format for the Protocol entries is defined
in the Module Information (INF) File Specification. Usage information may be
modified based on feature flag expressions that are evaluated during the build.
For example, the source INF may have a `SOMETIMES_PRODUCES` usage that may be
changed to `PRODUCES` in the _AsBuilt_ INF file if the build uses a feature flag
to include the item.

```ini
[Protocols.IA32]
  ## PRODUCES
  ## SOMETIMES_CONSUMES
  gEfiDecompressProtocolGuid

  ## SOMETIMES_PRODUCES ## Produces when PcdFrameworkCompatibilitySupport is set
  gEfiLoadPeImageProtocolGuid

  ## SOMETIMES_CONSUMES
  ## SOMETIMES_CONSUMES
  gEfiSimpleFileSystemProtocolGuid
```

### 8.7.7 [PPIs] Section

All Ppis that are listed in the source INF and their usage (if available) must
be include in this section. The format for the PPI entries is defined in the
Module Information (INF) File Specification. Usage information may be modified
based on feature flag expressions that are evaluated during the build. For
example, the source INF may have a SOMETIMES_PRODUCES usage that may be changed
to PRODUCES in the AsBuilt INF file if the build uses a feature flag to include
the item.

```ini
[Ppis.IA32]
  # SOMETIMES_CONSUMES # PeiReportStatusService is not ready if this PPI doesn't exist
  gEfiPeiStatusCodePpiGuid

  # SOMETIMES_CONSUMES # PeiResetService is not ready if this PPI doesn't exist
  gEfiPeiResetPpiGuid

  ## CONSUMES
  gEfiDxeIplPpiGuid

  ## PRODUCES
  gEfiPeiMemoryDiscoveredPpiGuid

  ## SOMETIMES_CONSUMES
  gEfiPeiDecompressPpiGuid

  ## SOMETIMES_PRODUCES
  ## NOTIFY
  # SOMETIMES_PRODUCES # Produce FvInfoPpi if the encapsulated FvImage is found
  gEfiPeiFirmwareVolumeInfoPpiGuid
```

### 8.7.8 [PatchPcd] Section

All PCDs that are listed in the source INF, that are defined as
`PatchableInModule` in the DSC file must be inserted into this section. The
current value and the offset into the `PE32` (.efi) file must be included in the
entry for each PCD listed in this section of the _AsBuilt_ INF file. If the
usage is available, that information must also be included. The format for the
PCD entries is defined in the Module Information (INF) File Specification.

To support override of the Formset class GUID in a binary HII driver, the build
system was enhanced as follows:

* Build tool will collect all VFR file names in one module and output them into
  a temp file, for example, `VfrFileName.txt`.

* After creating the EFI image, the **GenPatchPcdTable** tool will be used to
  create PatchPcd information with input from the MAP, EFI and
  `VfrFileName.txt`.

* **GenPatchPcdTable** will get HII data in the binary EFI image, and locate the
  reserved empty Formset class GUID slot (all zero GUID). If the empty slot is
  found, a Patchable PCD `PcdHiiFormSetClassGuid##VfrFileName` (type `VOID`* for
  GUID) will be auto generated. VfrFileName is obtained from the
  `VfrFileName.txt`.

* Usage information may be modified based on feature flag expressions that are
  evaluated during the build. For example, the source INF may have a
  `SOMETIMES_PRODUCES` usage that may be changed to `PRODUCES` in the _AsBuilt_
  INF file if the build uses a feature flag to include the item.


```ini
[PatchPcd.IA32]
  ## SOMETIMES_CONSUMES
  gEfiMdeModulePkgTokenSpaceGuid.PcdLoadFixAddressBootTimeCodePageNumber|0x00000000|0xC584

  ## SOMETIMES_CONSUMES
  gEfiMdeModulePkgTokenSpaceGuid.PcdLoadFixAddressRuntimeCodePageNumber|0x00000000|0xC588
```

### 8.7.9 [PcdEx] Section

All PCDs that are listed in the source INF, that are defined as `DynamicEx` in
the DSC file must be inserted into this section. In general, values for the
`DynamicEx` PCDs are global to a platform, and must not be inserted into the
_AsBuilt_ INF file. If the usage is available, that information must also be
included. The format for the PCD entries is defined in the Module Information
(INF) File Specification.

If the `DynamicEx` PCD was assigned as subtype HII, then for modules that
produce IFR for setup screens, the following is required. If any of the fields
of an EFI VarStore in the IFR are associated with a PCD, then the _AsBuilt_ INF
must declare that relationship. Since a module that produces IFR may not have C
code that uses the PCDs we need here, the source INF file may not list those
PCDs. Instead, the build tools when building a module that contains IFR must
determine if there is a mapping between PCDs and an EFI VarStore and add those
relationships to the AsBuilt INF. The syntax of the `[PcdEx]` for _AsBuilt_ INF
files is augmented by additional comment information for PCDs that are expected
to be used with HII. The current `<Usage>` comment will be followed by Variable
Name, Variable GUID C Name, and byte offset value which is the same order used
in a DSC file for a `[PcdsDynamixExHii]` section, separated by the "|" field
separation character.

Usage information may be modified based on feature flag expressions that are
evaluated during the build. For example, the source INF may have a
`SOMETIMES_PRODUCES` usage that may be changed to `PRODUCES` in the _AsBuilt_
INF file if the build uses a feature flag to include the item.

```ini
[PcdEx.IA32]
  ## SOMETIMES_PRODUCES
  ## SOMETIMES_CONSUMES
  gEfiMdeModulePkgTokenSpaceGuid.PcdConOutRow

  ## SOMETIMES_PRODUCES
  ## SOMETIMES_CONSUMES
  gEfiMdeModulePkgTokenSpaceGuid.PcdConOutColumn
```

### 8.7.10 [Depex] Section

The complete dependency expression including all dependencies from the
libraries linked with the module must be included in comments in this section.
The format for this dependency expression is defined in the Module Information
(INF) File Specification.

#### Example

```ini
[Depex]
# NOT (gEfiHiiDatabaseProtocolGuid AND gEfiHiiStringProtocolGuid)
# OR gPcdProtocolGuid
```

### 8.7.11 [BuildOptions] Section

The format for the build option entries is defined in the Module Information
(INF) File Specification. All entries in this section appear in comments,
beginning with the following line.

`## @AsBuilt`

#### Example

```ini
[BuildOptions.IA32]
## @AsBuilt
##   MSFT:DEBUG_VS2008x86_IA32_SYMRENAME_FLAGS = Symbol renaming not needed for
##   MSFT:DEBUG_VS2008x86_IA32_ASLDLINK_FLAGS = /NODEFAULTLIB /ENTRY:ReferenceAcpiTable /SUBSYSTEM:CONSOLE
##   MSFT:DEBUG_VS2008x86_IA32_VFR_FLAGS = -l -n
##   MSFT:DEBUG_VS2008x86_IA32_PP_FLAGS = /nologo /E /TC /FIAutoGen.h
##   MSFT:DEBUG_VS2008x86_IA32_GENFW_FLAGS =
##   MSFT:DEBUG_VS2008x86_IA32_OPTROM_FLAGS = -e
##   MSFT:DEBUG_VS2008x86_IA32_SLINK_FLAGS = /NOLOGO /LTCG
##   MSFT:DEBUG_VS2008x86_IA32_ASM_FLAGS = /nologo /c /WX /W3 /Cx /coff /Zd /Zi
##   MSFT:DEBUG_VS2008x86_IA32_ASL_FLAGS =
##   MSFT:DEBUG_VS2008x86_IA32_CC_FLAGS = /nologo /c /WX /GS- /W4 /Gs32768 /D UNICODE /O1ib2 /GL /FIAutoGen.h /EHs-c- /GR- /GF /Gy /Zi /Gm
##   MSFT:DEBUG_VS2008x86_IA32_VFRPP_FLAGS = /nologo /E /TC /DVFRCOMPILE /FI$(MODULE_NAME)StrDefs.h
##   MSFT:DEBUG_VS2008x86_IA32_ASLCC_FLAGS = /nologo /c /FIAutoGen.h /TC /Dmain = ReferenceAcpiTable
##   MSFT:DEBUG_VS2008x86_IA32_APP_FLAGS = /nologo /E /TC
##   MSFT:DEBUG_VS2008x86_IA32_DLINK_FLAGS = /NOLOGO /NODEFAULTLIB /IGNORE:4001 /OPT:REF /OPT:ICF=10 /MAP /ALIGN:32 /SECTION:.xdata,D /SECTION:.pdata,D /MACHINE:X86 /LTCG /DLL /ENTRY:$(IMAGE_ENTRY_POINT) /SUBSYSTEM:EFI_BOOT_SERVICE_DRIVER /SAFESEH:NO /BASE:0 /DRIVER /DEBUG /PDB:$(OUTPUT_PATH)\$(PACKAGE_NAME)_$(PACKAGE_GUID)_$(PACKAGE_VERSION)\$(PACKAGE_RELATIVE_DIR)\$(MODULE_FILE_BASE_NAME)\DEBUG\IA32\$(BASE_NAME).pdb /PDBSTRIPPED:$(OUTPUT_PATH)\$(PACKAGE_NAME)_$(PACKAGE_GUID)_$(PACKAGE_VERSION)\$(PACKAGE_RELATIVE_DIR)\$(MODULE_FILE_BASE_NAME)\DEBUG\IA32\$(BASE_NAME)_Stripped.pdb
##   MSFT:DEBUG_VS2008x86_IA32_ASLPP_FLAGS = /nologo /E /C /FIAutoGen.h
##   MSFT:DEBUG_VS2008x86_IA32_OBJCOPY_FLAGS = objcopy not needed for
##   MSFT:DEBUG_VS2008x86_IA32_MAKE_FLAGS = /nologo
##   MSFT:DEBUG_VS2008x86_IA32_ASMLINK_FLAGS = /nologo /tiny
```

### 8.7.12 [Binaries] Section

The format for the binaries section entries is listed in the Module Information
(INF) File Specification. The a binary `PE32` file, with the .efi extension,
was created by the build, it must be listed in this section. All files listed
in this section must be placed in a section with the corresponding
architectural modifier, such as `[Binaries.IA32]`, where IA32 is the
architectural modifier. The examples below do not cover all of the potential
file types that may appear in a binary INF file; it does show the file types
that must be placed into the auto-generated INF file created during a build.

The generic format for these entries are:

`<TS> BinaryType|[RelativePath]Filename.Extension`

The following is an example of an EFI file format:

`<TS> PE32|Filename.efi`

The following is an example of a DEPEX file format:

`<TS> DXE_DEPEX|Filename.depex`

If the build produces a PDB or SYM file, an entry must be placed in the
`[Binaries.$(ARCH)]` section. The following example shows an entry for a PDB
file.

`<TS> DISPOSABLE|Filename.pdb <EOL>`

If a filename is a fully qualified path and filename, such as a ROM filename,
the build tool must copy that file into the module's OUTPUT directory, then
insert the line as though it were in the directory as part of the build. For a
ROM file, the entry must use the following format:

`<TS> BIN|Filename.rom <EOL>`

For AML files from a platform, the entry must use the following format:

`<TS> ASL|Filename.aml <EOL>`

For ACPI files from a platform, the entry must use the following format:

`<TS> ACPI|Filename.acpi <EOL>`

For a Binary or raw binary file, the entry may use either of the the following
two formats:

```
<TS> RAW|Filename.raw <EOL>
<TS> BIN|Filename.bin <EOL>
```

If the tools cannot determine the content, the binary type, the tools must use
the BIN binary type.

In the above examples, the Filename may be preceded by a module relative path
subdirectory as in the following example:

```
<TS> PE32|Ia32/Filename.efi
<TS> RAW|Vtf0/Bin/ResetVec.ia32.raw
```

#### Example

```ini
[Binaries.IA32]
  PE32|Ia32/DxeCore.efi
  DISPOSABLE|Ia32/DxeCore.pdb
```

### 8.7.13 [Sources] Section

The build tools must never add the `[Sources]` section or the name of the files
from a sources section.

### 8.7.14 [UserExtensions] Section

The build tools must copy all of the source INF's `[UserExtensions]` sections
into the generated INF. The EDK II build tools will ignore these sections,
however other vendors may provide tools that have a priori knowledge of how to
process these sections.
