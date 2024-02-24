<!--- @file
  8.3 Auto-generated code

  Copyright (c) 2008-2019, Intel Corporation. All rights reserved.<BR>

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

## 8.3 Auto-generated code

The section covers, in sequence, the processes used to generate code files that
will be used during the build.

### 8.3.1 AutoGen Stage File Extensions

The following table provides the extension and a description of files processed
during the AutoGen stage of the build. The `build_rule.txt` file describes the
processing rules for generating the Makefiles for the $(MAKE) stage.

###### Table 14 AutoGen Stage Input File Extensions

| Extension | Description                                                                                | File Format         |
|:---------:| ------------------------------------------------------------------------------------------ | ------------------- |
| .c, .cpp  | C code files                                                                               | ASCII Text, DOS EOL |
| .h        | C header files                                                                             | ASCII Text, DOS EOL |
| .asm      | 32 and 64-bit Windows assembly files                                                       | ASCII Text, DOS EOL |
| .s        | 32 and 64-bit GCC assembly files                                                           | ASCII Text, DOS EOL |
| .S        | IPF GCC and Windows assembly files                                                         | ASCII Text, DOS EOL |
| .nasm     | 32 and 64-bit NASM assembly files                                                          | ASCII Text, DOS EOL |
| .i        | IPF Assembly include files                                                                 | ASCII Text, DOS EOL |
| .vfr      | Visual Forms Representation files                                                          | ASCII Text, DOS EOL |
| .uni      | HII Unicode string files                                                                   | UCS-2 Characters    |
| .idf      | HII Image Definition files                                                                 | ASCII Text, DOS EOL |
| .asl      | C formatted ACPI code files - these files are processed independent from the C code files  | ASCII Text, DOS EOL |
| .asi      | ACPI Header Files                                                                          | ASCII Text, DOS EOL |
| .aslc     | C formatted ACPI table files - these files are processed independent from the C code files | ASCII Text, DOS EOL |
| .txt      | Microcode text files                                                                       | ASCII Text, DOS EOL |
| .map      | VPD tool intermediate file                                                                 | ASCII Text, DOS EOL |
| .bin      | Binary files                                                                               | Binary              |
| .bmp      | Logo files used in the ImageGen stage                                                      | Binary              |
| .ui       | Unicode User Interface files                                                               | UCS-2 Characters    |
| .ver      | Unicode Version files                                                                      | UCS-2 Characters    |

### 8.3.2 Dependency expression file

The dependency expression file (`.depex`) is generated from the `[Depex]`
section in module's INF file, if the section presents, or `.dxs` file if
`DPX_SOURCE` definition is found in INF file. If both the DPX_SOURCE definition
and [Depex] section content is present, the content in the file specified in
the DPX_SOURCE definition is used and the [Depex] section content will be
ignored. The GUID used in `[Depex]` section must be the GUID C name.

First, the GUID C name in the dependency expression string will be converted
into its value in C structure format. Then the expression string will be
converted into postfix notation. Before saving to a file, the operator and GUID
value in the postfix notation will be converted to their binary value.

Dependency expression sections listed in an INF file may be scoped via feature
flag expressions (logical expressions which typically utilize PCDs using
FeatureFlag or FixedAtBuild access methods). It is the module writer's
responsibility to ensure the different sections are mutually exclusive. It is
the platform integrator's responsibility to ensure that they do not override
this exclusivity.

For example, the following dependency expression

`NOT (gEfiHiiDatabaseProtocolGuid AND gEfiHiiStringProtocolGuid) OR gPcdProtocolGuid`

will be converted to

```
include_statement($(MODULE_BUILD_DIR)/OUTPUT/$(BASE_NAME).dxs, "
  // PUSH
  02
  // gEfiHiiDatabaseProtocolGuid
  72 c1 9f ef b2 a1 93 46 b3 27 6d 32 fc 41 60 42
  // PUSH
  02
  // gEfiHiiStringProtocolGuid
  74 69 d9 0f aa 23 dc 4c b9 cb 98 d1 77 50 32 2a
  // AND
  03
  // NOT
  05
  // PUSH
  02
  // gPcdProtocolGuid
  06 40 b3 11 5b d8 0a 4d a2 90 d5 a5 71 31 0e f7
  // OR
  04
  // END
  08
");
```

The binary dependency expression file will be generated in
`$(MODULE_BUILD_DIR)/OUTPUT` with `.depex` file extension.

#### 8.3.2.1 Guidelines

Use of a separate file for describing the dependencies is discouraged. Grammar
of the INF, DSC and FDF files permit specifying the dependency expressions.
Libraries may also have a dependency, `[Depex]`, section. These dependencies
must be appended to the module's `DEPEX` sections unless the module includes a
depex (_.dxs_) file - even if the module does not contain a `[Depex]` section.
When a developer chooses to write the .dxs file, the developer is responsible
for specifying all dependencies in the .dxs file.

Libraries that are linked to a UEFI DRIVER may have `DEPEX` sections. There are
three 'rules' for the tools.

* Tools are coded so that for a given module the `[Depex]` sections of all
  linked-in library instances are logically `AND`'d with the `DEPEX` section of
  the module

  - If no `DEPEX` section is specified in the module, then only the library
    instances DEPEX sections are logically `AND`'d to create the `DEPEX` section
    for the module

* Tools are also coded to ignore the depex sections of libraries that are
  linked to `UEFI_DRIVER` or PCI Option ROM code

* The tools will break the build if one module, using one of the noted module
  types, contains a depex section in the INF file.

### 8.3.3 VFR

The EDK II build system provides tools for processing formatted Unicode files
and Visual Forms Representation (VFR) files in order to create the IFR files.
Refer to the EDK II User's Manual for more information regarding the use of the
Unicode and VFR files. Refer to the VfrCompiler description and the VFR
Programming Language document for more detailed information on the provided
implementation. Additionally, the EDK II build AutoGen tools are used to
process Unicode files listed in a module's INF file. Also note that the IFR
code is not compatible - UFI compliant IFR code is different from the IFR code
defined by early Intel Framework documents.

#### 8.3.3.1 Reference Implementation: Compatibility

The EDK II Vfr compiler tools can process EDK II VFR and Unicode files
and to generate UEFI/PI compliant IFR files. EDK II Unicode files can use the
UEFI defined Unicode extended grammar. Table 15 shows the compatibility matrix.

###### Table 15 VFR Compatibility Matrix

| Code                        | non-UEFI Compliant VFR Tools | UEFI Compliant VFR Tools |
| --------------------------- |:----------------------------:|:------------------------:|
| pre-UEFI 2.1 Unicode        | Yes                          | Yes                      |
| pre-UEFI 2.1 VFR Source     | Yes                          | Yes                      |
| pre-UEFI 2.1 IFR - binaries | Yes                          | No                       |
| UEFI 2.1 Unicode            | No                           | Yes                      |
| UEFI 2.1 Vfr                | No                           | Yes                      |
| UEFI 2.1 IFR - binaries     | No                           | Yes                      |

### 8.3.4 HII String Pack

The human-readable HII string pack data consists of UCS-2 characters in .uni
files. The build tools will do following steps to convert the strings
information into HII string pack data structure.

* The build tools will get all the string IDs, the associated string and
  language code from the `.uni` files. Note that the DSC file or options on the
  command-line may be used to filter the languages used for generating the
  AutoGen code. The `RFC_LANGUAGES` is a semi-colon separated, doubled quoted
  string of RFC 4646 language codes, while the `ISO_LANGUAGES` (for EDK
  components only) is a nonseparated double quoted string of three character
  ISO 639-2 language codes.

* For EDK II modules, their Unicode files must use RFC 4646 language codes. If
  an EDK II module's Unicode file contains a three character ISO 639-2 language
  code, the build will break with an appropriate warning message.

**********
**Note:** Tools must not refactor the EDK component ISO 639-2 language codes to
RFC 4646 language codes, as the DXE drivers are responsible for handling the
different language code formats.
**********

* Search all source files in the include path of the module to find out which
  string IDs are used.Macros will be generated in `AutoGen.h` for the string
  IDs used. Those string IDs not used will be generated but commented out. They
  is just for debug purposes. For example:

```c
include_statement (AutoGen.h, "
  //
  //Unicode String ID
  //

  // #define $LANGUAGE_NAME 0x0000  // not referenced
  // #define $PRINTABLE_LANGUAGE_NAME 0x0001  //not referenced
  #define STR_BOOT_FAILED 0x0002
  #define STR_BOOT_SUCCEEDED 0x0003
  #define STR_PERFORM_MEM_TEST 0x0004
  // #define STR_INTERNAL_EFI_SHELL 0x009E  // not referenced
  // #define STR_LEGACY_BOOT_A 0x009F  // not referenced
  // #define STR_PROCESSED_ALL_BOOT_OPTIONS 0x00A0  // not referenced
");
```

* The font attribute specifies the default font that will be used for the
  characters in string. If `#font` is not specified, then the default font
  identifier will be used.

* If the `#font` attribute appears before the first `#language` identifier,
  then it applies to all characters for all languages. If the `#font` attribute
  appears after a #language identifier, it applies only to the string
  characters in that language. It is permissible for `#font` to appear in more
  than one place, in which case the language-specific font identifier will have
  priority.

* The HII string package data will be generated in `AutoGen.c` in the form of a
  data array, with array name `<ModuleBaseName>`Strings. For example:

```c
include_statement (AutoGen.c, "
  //
  //Unicode String Pack Definition
  //
  unsigned char PlatformBdsDxeStrings[] = {
  // Start of string definitions for fra
    0x20, 0x1A, 0x00, 0x00, 0x02, 0x00, 0x8E, 0x02,
    0x00, 0x00, 0x96, 0x02, 0x00, 0x00, 0x9E, 0x00,
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
    // offset 0x16
    0x8E, 0x02, 0x00, 0x00, // offset to string $LANGUAGE_NAME (0x0000)
    0x96, 0x02, 0x00, 0x00, // offset to string
                            //$PRINTABLE_LANGUAGE_NAME (0x0001)
    ...
    ...
    // string $LANGUAGE_NAME offset 0x0000028E
    0x66, 0x00, 0x72, 0x00, 0x61, 0x00, 0x00, 0x00,
    // string $PRINTABLE_LANGUAGE_NAME offset 0x00000296
    0x46, 0x00, 0x72, 0x00, 0x61, 0x00, 0x6E, 0x00,
    0xE7, 0x00, 0x61, 0x00, 0x69, 0x00, 0x73, 0x00,
    0x00, 0x00,
    ...
    ...
    // strings terminator pack
    0x00, 0x00, 0x00, 0x00, 0x02, 0x00, 0x00, 0x00,
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
  };
");
```

#### 8.3.4.1 More than One Unicode File

If more than one Unicode file is required by a module, the rules for including
these files are as follows. If one Unicode file uses a `#include` statement to
include other Unicode files, these secondary Unicode files must also be listed
in the INF file's [Sources] section.

### 8.3.5 HII Image Pack

The HII Image package data is stored in `.idf` files. The build tools perform
the following steps to convert the image information into an HII Image package
data structure.

* The build tools retrieve all the image IDs, the optional `TRANSPARENT` setting
  and the associated image file name from the `.idf` files. The `TRANSPARENT`
  setting is optional. If it is specified, build tools apply the `TRANS` image
  block type to the input image file. The _UEFI Specification_ does not define
  the `TRANS` block type for JPG or PNG images. The `TRANSPARENT` setting is
  ignored for JPG and PNG images. The image file name should be listed in the
  `[Sources]` section of the INF file, and the extension of the image file must
  be one of `.bmp`, `.jpg`, or `.png`. The extension is case insensitive.

* Search all source files in the include path of the module to find out which
  image IDs are used. Macros are generated in `AutoGen.h` for the image IDs
  used. For example:
  ```c
  include_statement(AutoGen.h, "
    //
    //Image ID
    //
    #define IMG_FULL_LOGO  0x0001
    #define IMG_OEM_LOGO   0x0002
  ");
  ```

* The HII Image package data is generated in `<ModuleBaseName>Idf.hpk` or in
  `AutoGen.c` in the form of a data array, with array name
  `<ModuleBaseName>Images`. For example:
  ```c
  include_statement(AutoGen.c, "
    //
    //Image Pack Definition
    //
    unsigned char HelloWorldImages[] = {
      // STRGATHER_OUTPUT_HEADER
      0xD9, 0xCA, 0x01, 0x00,
      // Image PACKAGE HEADER
      0xD5, 0xCA, 0x01, 0x06, 0x0C, 0x00, 0x00, 0x00, 0x97, 0xC7, 0x01, 0x00,
      // Image DATA
      // 0x0001: IMG_FULL_LOGO: 0x0001
      0x12, 0x01, 0x90, 0x01, 0xDC, 0x00, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF,
      ...
      ...
      // 0x0002: IMG_OEM_LOGO: 0x0002
      0x14, 0x02, 0x25, 0x01, 0xDC, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
      ...
      ...
      // End of the Image Info
      0x00,
      // Palette Header
      0x03, 0x00,
      // Palette Data
      // 0x0001: IMG_FULL_LOGO: 0x0001
      0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0x00, 0x80, 0x00, 0x00, 0x80, 0x80, 0x80, 0x00, 0x00, 0x80,
      0x00, 0x80, 0x80, 0x80, 0x00, 0x80, 0x80, 0x80, 0xC0, 0xC0, 0xC0, 0x00, 0x00, 0xFF, 0x00, 0xFF,
      ...
      ...
      // 0x0002: IMG_OEM_LOGO: 0x0002
       0x00, 0x00, 0x00, 0x00, 0x00, 0x80, 0x00, 0x80, 0x00, 0x00, 0x80, 0x80,
      ...
      ...
    };
  ");
  ```

* If more than one image definition file is required by a module, the build tools
  combine the images from the multiple `.idf` files into a single HII Image Pack.

### 8.3.6 AutoGen.h file

The code generated in AutoGen.h includes:

* Prototypes of constructor and destructor from the library instances the
  module will link against
* Prototypes of entry and unload image entry points
* Global variable definitions for GUID/Protocol/PPI values used as well as
  extern definitions
* Global variable definitions and the database of PCDs used
* Unicode string database definitions.
* Image package database definitions.

The file will contain:

#### 8.3.6.1 Header prologue

The macro name is composed with GUID value of INF file.

```c
include_statement (AutoGen.h, "
  #ifndef _AUTOGENH_6987936E_ED34_44db_AE97_1FA5E4ED2116
  #define _AUTOGENH_6987936E_ED34_44db_AE97_1FA5E4ED2116

  #ifdef __cplusplus
  extern "C" {
  #endif
");
```

#### 8.3.6.2 Header file inclusion.

Only one header file is included.

```c
include_statement (AutoGen.h, "
  #include <Base.h>
");
```

#### 8.3.6.3 Caller ID GUID definition.

The GUID value is the same as INF file GUID. The macro, `EFI_CALLER_ID_GUID`,
is generated only for non - library module.

```c
include_statement (AutoGen.h, "
  extern GUID gEfiCallerIdGuid;
  // following definition is not needed for library module
  #define EFI_CALLER_ID_GUID \
    { 0x6987936E, 0xED34, 0x44db, { 0xAE, 0x97, 0x1F, 0xA5, 0xE4, 0xED,
      0x21, 0x16 } }
");
```

#### 8.3.6.4 PCD definitions

There are differences in the generated code for library and non-library
modules, which are illustrated in pseudo-code below.

##### 8.3.6.4.1 Non-library Module

```c
include_statement(AutoGen.h, "
  #define _PCD_TOKEN_<TokenCName> <TokenNumber>
");

If ((PCD_type == FIXED_AT_BUILD) || (PCD_type == FEATURE_FLAG)) {
  If ((DatumType == 'VOID*') &&
      (
        (PcdValue == array) ||
        (PcdValue == C_FormatGuid) ||
        (PcdValue == C_String)
      )
     ) {
    include_statement (AutoGen.h, "
      "#define _PCD_PATCHABLE_<TokenCName>_SIZE <MaxDatumSize>"
    );
  }
  include_statement (AutoGen.h, "
  #define _PCD_VALUE_<TokenCName> <PcdValue>
  extern const <DatumType> _gPcd_FixedAtBuild_<TokenCName>;
  #define _PCD_GET_MODE_<DatumSize>_<TokenCName> _gPcd_FixedAtBuild_<TokenCName>
  ");
}
If (PCD_type == PATCHABLE_IN_MODULE) {

  If ((DatumType == 'VOID*') &&
      (
        (PcdValue == array) ||
        (PcdValue == C_FormatGuid) ||
        (PcdValue == C_String)
      )
     ) {
    include_statement (AutoGen.h, "
      #define _PCD_PATCHABLE_<TokenCName>_SIZE <MaxDatumSize>
    ");

  }
  include_statement (AutoGen.h, "
    #define _PCD_VALUE_<TokenCName> <PcdValue>
    extern <DatumType> _gPcd_BinaryPatch_<TokenCName>;
    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> _gPcd_BinaryPatch_<TokenCName>
  ");

  If ((DatumType == 'VOID*') &&
      (
        (PcdValue == array) ||
        (PcdValue == C_FormatGuid) ||
        (PcdValue == C_String)
      )
     ) {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(SizeOfBuffer,Buffer) \
        LibPatchPcdSetPtr (_gPcd_BinaryPatch_<TokenCName>, \
         (UINTN)_PCD_PATCHABLE_<TokenCName>_SIZE, \
         (SizeOfBuffer), \
         (Buffer) \
        )
    ");
  } Else {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(Value) \
        (_gPcd_BinaryPatch_<TokenCName> = (Value))
    ");
  }
}
If (PCD_type == DYNAMIC) {
  include_statement (AutoGen.h, "
    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> \
      LibPcdGet<DatumSize>(_PCD_TOKEN_<PcdTokenCName>)
  ");
  If (DatumType == 'VOID*') {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(SizeOfBuffer, Buffer) \
        LibPcdSet<DatumSize> ( \
          _gPcd_BinaryPatch_<TokenCName>, \
          (SizeOfBuffer), \
          (Buffer) \
        )
    ");
  } Else {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(Value) \
        LibPcdSet<DatumSize>(_gPcd_BinaryPatch_<TokenCName>, (Value))
    ");
  }
}
If (PCD_type == DYNAMIC_EX) {
  include_statement (AutoGen.h, "
    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> \
      LibPcdGetEx<DatumSize>(&<TokenSpaceGuidCName>, \
         _PCD_TOKEN_<PcdTokenCName> \
       )
  ");
  If (DatumType == 'VOID*') {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>( \
        SizeOfBuffer, Buffer) \
        LibPcdSetEx<DatumSize>(&<TokenSpaceGuidCName>, \
          _gPcd_BinaryPatch_<TokenCName>, \
          (SizeOfBuffer), \
          (Buffer) \
          )
    ");
  } Else {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(Value) \
        LibPcdSetEx<DatumSize>(&<TokenSpaceGuidCName>, \
          _gPcd_BinaryPatch_<TokenCName>, \
          (Value) \
          )
    ");
  }
}
```

##### 8.3.6.4.2 Library Module

```c
nclude_statement(AutoGen.h, "
  #define _PCD_TOKEN_<TokenCName> <TokenNumber>
");

If ((PCD_TYPE == FIXED_AT_BUILD) &&
    (ALL_MODULES_LINKED_W_THIS_LIB_USE_PCD_TYPE == FIXED_AT_BUILD) &&
    (ALL_MODULES_LINKED_W_THIS_LIB_USE_PCD_VALUES == <ThisPcdValue>)) {
  #define _PCD_VALUE_<TokenCName> <PcdValue>
}

If ((PCD_type == FIXED_AT_BUILD) || (PCD_type == FEATURE_FLAG)) {
  include_statement (AutoGen.h, "
    extern const <DatumType> _gPcd_FixedAtBuild_<TokenCName>;

    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> \
      _gPcd_FixedAtBuild_<TokenCName>
  ");
}

If (PCD_type == PATCHABLE_IN_MODULE) {
  include_statement (AutoGen.h, "
    extern <DatumType> _gPcd_BinaryPatch_<TokenCName>;

    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> \
      _gPcd_BinaryPatch_<TokenCName>
    #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(Value) \
      (_gPcd_BinaryPatch_<TokenCName> = (Value))

  ");
}

If (PCD_type == DYNAMIC) {
  include_statement (AutoGen.h, "
    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> \
      LibPcdGet<DatumSize>(_PCD_TOKEN_<PcdTokenCName>)
  ");

  If (DatumType == 'VOID*') {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>( \
        SizeOfBuffer, Buffer) \
        LibPcdSet<DatumSize>(_gPcd_BinaryPatch_<TokenCName>, \
          (SizeOfBuffer), \
          (Buffer) \
          )
    ");
  } Else {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(Value) \
        LibPcdSet<DatumSize>(_gPcd_BinaryPatch_<TokenCName>, (Value))
    ");
  }
}

If (PCD_type == DYNAMIC_EX) {
  include_statement (AutoGen.h, "
    #define _PCD_GET_MODE_<DatumSize>_<TokenCName> \
      LibPcdGetEx<DatumSize>( &<TokenSpaceGuidCName>, \
        _PCD_TOKEN_<PcdTokenCName> )
  ");
  If (DatumType == 'VOID*') {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>( \
        SizeOfBuffer, Buffer ) \
        LibPcdSetEx<DatumSize>(&<TokenSpaceGuidCName>, \
          _gPcd_BinaryPatch_<TokenCName>, \
          (SizeOfBuffer), \
          (Buffer) \
          )
    ");
  } Else {
    include_statement (AutoGen.h, "
      #define _PCD_SET_MODE_<DatumSize>_<TokenCName>(Value) \
        LibPcdSetEx<DatumSize>(&<TokenSpaceGuidCName>, \
          _gPcd_BinaryPatch_<TokenCName>, \
          (Value) \
          )
    ");
  }
}
```

##### 8.3.6.4.3 HII string pack definitions,

These are generated only if `.uni` files are found. For details, please refer
to section 7.3.2.

```c
include_statement (AutoGen.h, "
  //
  //Unicode String ID
  //
  // #define $LANGUAGE_NAME 0x0000  // not referenced
  // #define $PRINTABLE_LANGUAGE_NAME 0x0001  // not referenced
  #define STR_MISC_BASE_BOARD_MANUFACTURER 0x0002
  #define STR_MISC_BASE_BOARD_PRODUCT_NAME 0x0003
  #define STR_MISC_BASE_BOARD_VERSION 0x0004
  // ...
  // ...
  // ...
  extern unsigned char MiscSubclassStrings[];

  #define STRING_ARRAY_NAME MiscSubclassStrings

");
```

##### 8.3.6.4.4 HII image pack definitions

These are generated only if `.idf` files are found.
```
include_statement(AutoGen.h, "
  //
  //Image ID
  //
  #define IMG_FULL_LOGO  0x0001
  #define IMG_OEM_LOGO   0x0002

  extern unsigned char  HelloWorldImages[];

  #define IMAGE_ARRAY_NAME  HelloWorldImages
");
```

#### 8.3.6.5 AutoGen Epilogue

```c
#ifdef __cplusplus
}
#endif

#endif
```

### 8.3.7 AutoGen.c file

The code generated in AutoGen.c includes:

* Calling of constructor and destructor of library instances against which the
  module will link
* The module load and unload entry points
* Global variables for GUID/Protocol/PPIs value used, global variables and
  database for PCDs used
* Unicode string pack definition.
* Image pack definition.

`AutoGen.c` file is only generated for EDK II non-library modules. The following
sections identify what lines of information are included in the file as well as
pseudo-code to references on to how a variable (<var_name>) might be generated.

The file will contain:

#### 8.3.7.1 Header files inclusion.

Which files are included is determined by module type.

```c
Switch MODULE_TYPE {
  case "BASE":
  case "HOST_APPLICATION":
  case "USER_DEFINED":
    include_statement (AutoGen.c, "
      #include <Base.h>

    );
    break;
  case "SEC":
  case "PEI_CORE":
  case "PEIM":
    include_statement (AutoGen.c, "
      #include <PiPei.h>
      #include <Library/DebugLib.h>

    ");
    break;
  case "DXE_CORE":
    include_statement (AutoGen.c, "
      #include <PiDxe.h>
      #include <Library/DebugLib.h>

    ");
    break;
  case "DXE_DRIVER":
  case "DXE_SMM_DRIVER":
  case "DXE_RUNTIME_DRIVER":
  case "DXE_SAL_DRIVER":
  case "UEFI_DRIVER":
  case "UEFI_APPLICATION"
    include_statement (AutoGen.c, "
      #include <PiDxe.h>
      #include <Library/BaseLib.h>
      #include <Library/DebugLib.h>
      #include <Library/UefiBootServicesTableLib.h>

    ");
    break;
  default:
    PrintError ( "%s\n", message);
    BreakTheBuild();
}
```

The following will be inserted in AutoGen.c after the header files have been
included.

`GLOBAL_REMOVE_IF_UNREFERENCED CHAR8 *gEfiCallerBaseName = "<ModuleName>";`

Where the `<ModuleName>` is the value of the `BASE_NAME` from the module INF
file's `[Defines]` section.

#### 8.3.7.2 Caller ID GUID variable definition.

Because not all GUID variables are required, a link-time optimization removes
items that are not referenced by other parts of the code to save on space in
the image.

```c
include_statement (AutoGen.c, "
  GLOBAL_REMOVE_IF_UNREFERENCED GUID gEfiCallerIdGuid = {0x4A9B9DB8,
    0xEC62, 0x4A92, {0x81, 0x8F, 0x8A, 0xA0, 0x24, 0x6D, 0x24, 0x6E}};
");
```

#### 8.3.7.3 Library Constructor Statements

If there are `CONSTRUCTOR`s defined in `[Defines]` section in INF file of the
library instances that are being linked to.

```c
If (CONSTRUCTOR defined in INF) {

  If ((MODULE_TYPE == "BASE") || (MODULE_TYPE == "SEC")) {
    include_statement (AutoGen.c, "
      EFI_STATUS
      EFIAPI
      <CONSTRUCTOR> (
        VOID );

      ");

  }

  If ((MODULE_TYPE == "PEI_CORE") || (MODULE_TYPE == "PEIM")) {
    include_statement (AutoGen.c, "
      EFI_STATUS
      EFIAPI
      <CONSTRUCTOR> (
        IN EFI_PEI_FILE_HANDLE  FileHandle,
        IN EFI_PEI_SERVICES     **PeiServices
      );

    ");

  If ((MODULE_TYPE == 'DXE_CORE') || (MODULE_TYPE == 'DXE_DRIVER') ||
    (MODULE_TYPE == 'DXE_SMM_DRIVER') ||
    (MODULE_TYPE == 'DXE_RUNTIME_DRIVER' ||
    (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
    (MOODULE_TYPE == 'UEFI_DRIVER') ||
    (MODULE_TYPE == 'UEFI_APPLICATION')) {

    include_statement (AutoGen.c, "
      EFI_STATUS
      EFIAPI
      <CONSTRUCTOR> (
        IN EFI_HANDLE        ImageHandle,
        IN EFI_SYSTEM_TABLE  *SystemTable
      );

    ");
  }

} // End CONSTRUCTOR defined in INF

If ((MODULE_TYPE == "BASE") || (MODULE_TYPE == "SEC")) {
  include_statement (AutoGen.c, "
    VOID
    EFIAPI
    ProcessLibraryConstructorList (
      VOID
    )
  ");
}

If ((MODULE_TYPE == "PEI_CORE") || (MODULE_TYPE == "PEIM")) {
  include_statement (AutoGen.c, "
    VOID
    EFIAPI
    ProcessLibraryConstructorList (
      IN EFI_PEI_FILE_HANDLE  FileHandle,
      IN EFI_PEI_SERVICES     **PeiServices
    )

  ");
}

If ((MODULE_TYPE == 'DXE_CORE') || (MODULE_TYPE == 'DXE_DRIVER') ||
    (MODULE_TYPE == 'DXE_SMM_DRIVER') ||
    (MODULE_TYPE == 'DXE_RUNTIME_DRIVER' ||
    (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
    (MOODULE_TYPE == 'UEFI_DRIVER') ||
    (MODULE_TYPE == 'UEFI_APPLICATION')) {
  include_statement (AutoGen.c, "
    VOID
    EFIAPI
    ProcessLibraryConstructorList (
      IN EFI_PEI_FILE_HANDLE  ImageHandle,
      IN EFI_PEI_SERVICES     **SystemTable
    )
  ");
}

include_statement (AutoGen.c, "
  {
");

If (CONSTRUCTOR defined in INF) {
  If ((MODULE_TYPE == "BASE") || (MODULE_TYPE == "SEC")) {
    include_statement (AutoGen.c, "
       EFI_STATUS Status;

       Status = <CONSTRUCTOR> ();
       ASSERT_EFI_ERROR (Status);

    ");
  }

  If ((MODULE_TYPE == "PEI_CORE") || (MODULE_TYPE == "PEIM")) {
    include_statement (AutoGen.c, "
      EFI_STATUS Status;

      Status = <CONSTRUCTOR> (FileHandle, PeiServices);
      ASSERT_EFI_ERROR (Status);

    ");
  }

  If ((MODULE_TYPE == 'DXE_CORE') || (MODULE_TYPE == 'DXE_DRIVER') ||
      (MODULE_TYPE == 'DXE_SMM_DRIVER') ||
      (MODULE_TYPE == 'DXE_RUNTIME_DRIVER' ||
      (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
      (MOODULE_TYPE == 'UEFI_DRIVER') ||
      (MODULE_TYPE == 'UEFI_APPLICATION')) {
    include_statement (AutoGen.c, "
      EFI_STATUS Status;

      Status = <CONSTRUCTOR> (ImageHandle, SystemTable);
      ASSERT_EFI_ERROR (Status);

    ");
  }
}

include_statement (AutoGen.c, "
  }
");
```

#### 8.3.7.4 Library Destructor Statements

Contained if there are `DESTRUCTOR`s defined in `[Defines]` section in INF file
of the library instances that are being linked to.

```c
If (DESTRUCTOR defined in INF) {
  If ((MODULE_TYPE == "BASE") || (MODULE_TYPE == "SEC")) {
    include_statement (AutoGen.c, "
      EFI_STATUS
      EFIAPI
      <DESTRUCTOR> (
        VOID
      );
    ");
  }
  If ((MODULE_TYPE == "PEI_CORE") || (MODULE_TYPE == "PEIM")) {
    include_statement (AutoGen.c, "
      EFI_STATUS
      EFIAPI
      <DESTRUCTOR> (
        IN EFI_PEI_FILE_HANDLE  FileHandle,
        IN EFI_PEI_SERVICES     **PeiServices
     );
    ");
  }
  If ((MODULE_TYPE == 'DXE_CORE') || (MODULE_TYPE == 'DXE_DRIVER') ||
      (MODULE_TYPE == 'DXE_SMM_DRIVER') ||
      (MODULE_TYPE == 'DXE_RUNTIME_DRIVER' ||
      (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
      (MOODULE_TYPE == 'UEFI_DRIVER') ||
      (MODULE_TYPE == 'UEFI_APPLICATION')) {
    include_statement (AutoGen.c, "
      EFI_STATUS
      EFIAPI
      <DESTRUCTOR> (
        IN EFI_HANDLE        ImageHandle,
        IN EFI_SYSTEM_TABLE  *SystemTable
      );
    ");
  }
} // End DESTRUCTOR defined in INF

If ((MODULE_TYPE == "BASE") || (MODULE_TYPE == "SEC")) {
  include_statement (AutoGen.c, "
    VOID
    EFIAPI
    ProcessLibraryDestructorList (
      VOID
    )

  ");
}

If ((MODULE_TYPE == "PEI_CORE") || (MODULE_TYPE == "PEIM")) {
  include_statement (AutoGen.c, "
    VOID
    EFIAPI
    ProcessLibraryDestructorList (
      IN EFI_PEI_FILE_HANDLE  FileHandle,
      IN EFI_PEI_SERVICES     **PeiServices
    )

  ");
}

If ((MODULE_TYPE == 'DXE_CORE') || (MODULE_TYPE == 'DXE_DRIVER') ||
    (MODULE_TYPE == 'DXE_SMM_DRIVER') ||
    (MODULE_TYPE == 'DXE_RUNTIME_DRIVER' ||
    (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
    (MOODULE_TYPE == 'UEFI_DRIVER') ||
    (MODULE_TYPE == 'UEFI_APPLICATION')) {
  include_statement (AutoGen.c, "
    VOID
    EFIAPI
    ProcessLibraryDestructorList (
     IN EFI_PEI_FILE_HANDLE  ImageHandle,
     IN EFI_PEI_SERVICES     *SystemTable
    )

  ");
}

include_statement (AutoGen.c, "
  {

");

If (DESTRUCTOR defined in INF) {
  If ((MODULE_TYPE == "BASE") || (MODULE_TYPE == "SEC")) {
    include_statement (AutoGen.c, "
      EFI_STATUS Status;

      Status = <DESTRUCTOR> ();
      ASSERT_EFI_ERROR (Status);

    ");
  }

  If ((MODULE_TYPE == "PEI_CORE") || (MODULE_TYPE == "PEIM")) {
    include_statement (AutoGen.c, "
      EFI_STATUS Status;

      Status = <DESTRUCTOR> (FileHandle, PeiServices);
      ASSERT_EFI_ERROR (Status);
    ");
  }

  If ((MODULE_TYPE == 'DXE_CORE') || (MODULE_TYPE == 'DXE_DRIVER') ||
      (MODULE_TYPE == 'DXE_SMM_DRIVER') ||
      (MODULE_TYPE == 'DXE_RUNTIME_DRIVER' ||
      (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
      (MOODULE_TYPE == 'UEFI_DRIVER') ||
      (MODULE_TYPE == 'UEFI_APPLICATION')) {
    include_statement (AutoGen.c, "
      EFI_STATUS Status;

      Status = <DESTRUCTOR> (ImageHandle, SystemTable);
      ASSERT_EFI_ERROR (Status);
    ");
  }
}

include_statement (AutoGen.c, "
  }
");
```

#### 8.3.7.5 Module Entry Point Statements

Contained if there are `ENTRY_POINT`s defined `[Defines]` section in INF file.

```c
If (ENTRY_POINT defined in INF) {
  If (MODULE_TYPE == 'PEI_CORE') {
    include_statement (AutoGen.c, "
      EFI_STATUS
      <ENTRY_POINT> (
        IN CONST EFI_SEC_PEI_HAND_OFF    *SecCoreData,
        IN CONST EFI_PEI_PPI_DESCRIPTOR  *PpiList,
        IN VOID                          *OldCoreData
      );

      EFI_STATUS
      EFIAPI
      ProcessModuleEntryPointList (
        IN CONST EFI_SEC_PEI_HAND_OFF    *SecCoreData,
        IN CONST EFI_PEI_PPI_DESCRIPTOR  *PpiList,
        IN VOID                          *OldCoreData
      )

      {
        return <ENTRY_POINT> (SecCoreData, PpiList, OldCoreData);
      }

    ");
  }

  If (MODULE_TYPE == 'DXE_CORE') {
    include_statement (AutoGen.c, "
      const UINT32 _gUefiDriverRevision = 0;

      VOID
      <ENTRY_POINT> (
        IN VOID  *HobStart
        );

      VOID
      EFIAPI
      ProcessModuleEntryPointList (
        IN VOID  *HobStart
        )

      {
        <ENTRY_POINT> (HobStart);
      }

    ");
  }

  If (MODULE_TYPE == 'PEIM') {
    include_statement (AutoGen.c, "
      GLOBAL_REMOVE_IF_UNREFERENCED const UINT32 _gPeimRevision = 0;
    ");
    If (Number of ENTRY_POINT == 0) {
      include_statement (AutoGen.c, "
        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_PEI_FILE_HANDLE  FileHandle,
          IN EFI_PEI_SERVICES     **PeiServices
          )
        {
          return EFI_SUCCESS;
        }

      ");
    }

    If (Number of ENTRY_POINT == 1) {
      include_statement (AutoGen.c, "
        EFI_STATUS
        <ENTRY_POINT> (
          IN EFI_PEI_FILE_HANDLE  FileHandle,
          IN EFI_PEI_SERVICES     **PeiServices
          );

        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_PEI_FILE_HANDLE  FileHandle,
          IN EFI_PEI_SERVICES     **PeiServices
          )
        {
          return <ENTRY_POINT> (FileHandle, PeiServices);
        }

      ");
    }

    If (Number  of   ENTRY_POINT > 1) {
      include_statement (AutoGen.c, "
        <ENTRY_POINT1> (
          IN EFI_PEI_FILE_HANDLE  FileHandle,
          IN EFI_PEI_SERVICES     **PeiServices
          );

        <ENTRY_POINT2> (
          IN EFI_PEI_FILE_HANDLE  FileHandle,
          IN EFI_PEI_SERVICES     **PeiServices
          );

        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_PEI_FILE_HANDLE FileHandle,
          IN EFI_PEI_SERVICES **PeiServices
          )

        {
          EFI_STATUS Status;
          EFI_STATUS CombinedStatus;

          CombinedStatus = EFI_LOAD_ERROR;

          Status = <ENTRY_POINT1> (FileHandle, PeiServices);
          if (!EFI_ERROR (Status) || EFI_ERROR (CombinedStatus)) {
            CombinedStatus = Status;
          }

          Status = <ENTRY_POINT2> (FileHandle, PeiServices);
          if (!EFI_ERROR (Status) || EFI_ERROR (CombinedStatus)) {
            CombinedStatus = Status;
          }

          return CombinedStatus;
        }

      ");
    }
  }

  If (MODULE_TYPE == 'DXE_SMM_DRIVER') {
    If (Number of ENTRY_POINT == 0) {
      include_statement (AutoGen.c, "
        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          )

        {
          return EFI_SUCCESS;
        }

      ");
    }

    If (Number of ENTRY_POINT == 1) {
      include_statement (AutoGen.c, "
        EFI_STATUS
        <ENTRY_POINT> (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          );

        static BASE_LIBRARY_JUMP_BUFFER  mJumpContext;
        static EFI_STATUS  mDriverEntryPointStatus = EFI_LOAD_ERROR;

        VOID
        EFIAPI
        ExitDriver (
          IN EFI_STATUS  Status
          )

        {
          if (!EFI_ERROR (Status) || EFI_ERROR (mDriverEntryPointStatus)) {
            mDriverEntryPointStatus = Status;
          }
          LongJump (&mJumpContext, (UINTN) - 1);
          ASSERT (FALSE);
        }

        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          )

        {
          if (SetJump (&mJumpContext) == 0) {
            ExitDriver (<ENTRY_POINT> (ImageHandle, SystemTable));
            ASSERT (FALSE);
          }

          return mDriverEntryPointStatus;
        }

      ");
    }
  }

  If ((MODULE_TYPE == 'DXE_RUNTIME_DRIVER') ||
      (MODULE_TYPE == 'DXE_DRIVER') ||
      (MODULE_TYPE == 'DXE_SAL_DRIVER') ||
      (MODULE_TYPE == 'UEFI_DRIVER') ||
      (MODULE_TYPE == 'UEFI_APPLICATION')) {
    include_statement (AutoGen.c, "
      const UINT32 _gUefiDriverRevision = 0;

    ");

    If (Number of ENTRY_POINT == 0) {
      include_statement (AutoGen.c, "
        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          )
        {
          return EFI_SUCCESS;
        }

      ");
    }

    If (Number of ENTRY_POINT == 1) {
      include_statement (AutoGen.c, "
        EFI_STATUS
        ${Function} (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          );

        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          )
        {
          return <ENTRY_POINT> (ImageHandle, SystemTable);
        }

        VOID
        EFIAPI
        ExitDriver (
          IN EFI_STATUS  Status
          )
        {
          if (EFI_ERROR (Status)) {
            ProcessLibraryDestructorList (gImageHandle, gST);
          }
          gBS->Exit (gImageHandle, Status, 0, NULL);
        }

      ");
    }

    If (Number of ENTRY_POINT > 1) {
      include_statement (AutoGen.c, "
        <ENTRY_POINT1> (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          );

        <ENTRY_POINT2> (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          );

        EFI_STATUS
        EFIAPI
        ProcessModuleEntryPointList (
          IN EFI_HANDLE        ImageHandle,
          IN EFI_SYSTEM_TABLE  *SystemTable
          )
        {
          if (SetJump (&mJumpContext) == 0) {
            ExitDriver (<ENTRY_POINT1> (ImageHandle, SystemTable));
            ASSERT (FALSE);
          }

          if (SetJump (&mJumpContext) == 0) {
            ExitDriver (<ENTRY_POINT2> (ImageHandle, SystemTable));
            ASSERT (FALSE);
          }

          return mDriverEntryPointStatus;
        }

        static BASE_LIBRARY_JUMP_BUFFER mJumpContext;
        static EFI_STATUS mDriverEntryPointStatus = EFI_LOAD_ERROR;

        VOID
        EFIAPI
        ExitDriver (
          IN EFI_STATUS Status
          )
        {
          if (!EFI_ERROR (Status) || EFI_ERROR (mDriverEntryPointStatus)) {
            mDriverEntryPointStatus = Status;
          }
          LongJump (&mJumpContext, (UINTN) - 1);
          ASSERT (FALSE);
        }

      ");
    }
  }
}
```

#### 8.3.7.6 Module Unload Image Statements

The following algorithm is used to process potential `UNLOAD_IMAGE` statements
that might be defined in the `[Defines]` section in the INF file.

```c
If (Number of UNLOAD_IMAGE in INF == 0) {
  include_statement (AutoGen.c, "
    GLOBAL_REMOVE_IF_UNREFERENCED const UINT8 _gDriverUnloadImageCount = 0;

    EFI_STATUS
    EFIAPI
    ProcessModuleUnloadList (
      IN EFI_HANDLE  ImageHandle
      )
    {
      return EFI_SUCCESS;
    }

  ");
}

If (Number of UNLOAD_IMAGE in INF == 1) {
  include_statement (AutoGen.c, "
    GLOBAL_REMOVE_IF_UNREFERENCED const UINT8 _gDriverUnloadImageCount = 1;
    EFI_STATUS
    <UNLOAD_IMAGE> (
      IN EFI_HANDLE  ImageHandle
    );

    EFI_STATUS
    EFIAPI
    ProcessModuleUnloadList (
      IN EFI_HANDLE ImageHandle
      )
    {
      return <UNLOAD_IMAGE> (ImageHandle);
    }

  ");
}

If (Number of UNLOAD_IMAGE in INF > 1) {
  include_statement (AutoGen.c, "
    GLOBAL_REMOVE_IF_UNREFERENCED const UINT8 _gDriverUnloadImageCount = <NumberOfUnloadImage>;

    EFI_STATUS
    <UNLOAD_IMAGE1> (
      IN EFI_HANDLE  ImageHandle
      );

    EFI_STATUS
    <UNLOAD_IMAGE2> (
      IN EFI_HANDLE  ImageHandle
      );

    EFI_STATUS
    EFIAPI
    ProcessModuleUnloadList (
      IN EFI_HANDLE  ImageHandle
      )
    {
      EFI_STATUS Status;

      Status = EFI_SUCCESS;

      if (EFI_ERROR (Status)) {
        <UNLOAD_IMAGE1> (ImageHandle);
      } else {
        Status = <UNLOAD_IMAGE1> (ImageHandle);
      }

      if (EFI_ERROR (Status)) {
        <UNLOAD_IMAGE2> (ImageHandle);
      } else {
        Status = <UNLOAD_IMAGE2> (ImageHandle);
      }

      return Status;
    }

  ");
}
```

#### 8.3.7.7 Global variables

These are generated from "Guids", "Protocols", "Ppis", "xxxPcd" sections of the
`.inf` file and `.uni` and `.idf` files.

```c
InfList = [];

add (ModuleInf, InfList);

foreach LibraryInstance {
  add (LibraryInf, InfList);
  foreach DependentLibraryInstance {
    add (LibraryInf, InfList);
  }
}

foreach INF in InfList {
  If ("[Guids]" defined in INF) {
    foreach GuidCName {
      include_statement (AutoGen.c, "
        GLOBAL_REMOVE_IF_UNREFERENCED EFI_GUID <GuidCName> = <GuidValue>;
      ");
    }
  }

  If ("[Protocols]" defined in INF) {
    foreach ProtocolGuidCName {
      include_statement (AutoGen.c, "
        GLOBAL_REMOVE_IF_UNREFERENCED EFI_GUID <ProtocolGuidCName> = <GuidValue>;
      ");
    }
  }

  If (("[Ppis]" defined in INF) {
    foreach PpiGuidCName {
      include_statement (AutoGen.c, "
         LOBAL_REMOVE_IF_UNREFERENCED EFI_GUID <PpiGuidCName> = <GuidValue>;
      ");
    }
  }

  If ("[Pcd]" defined in INF) {
    foreach PcdCName {
      If ((PcdDatumType == 'VOID*') &&
          (
           (PcdValue == array) ||
           (PcdValue == C_FormatGuid) ||
           (PcdValue == C_String)
          )
         ) {
        include_statement (AutoGen.c, "
          GLOBAL_REMOVE_IF_UNREFERENCED UINT8 <PcdCName> = <PcdValueMacro>;
        ");
      } Else {
        include_statement (AutoGen.c, "
          GLOBAL_REMOVE_IF_UNREFERENCED <PcdDatumType> <PcdCName> = <PcdValueMacro>;
        ");
      }
    }
  }

  If (.UNI file found in INF SourcesSection) {
    include_statement (AutoGen.c, "
      unsigned char MiscSubclassStrings[] = {
        ......
      }
    ");
  }

  If (.IDF file found in INF SourcesSection) {
    include_statement (AutoGen.c, "
      unsigned char  HelloWorldImages[] = {
        ......
      }
    ");
  }
}
```
