<!--- @file
  10.3 Build Intermediate Images

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

## 10.3 Build Intermediate Images

### 10.3.1 Binary modules

Binary modules can be inserted into flash image in one of three ways. The first
way is to use the FILE statement mentioned in Section 10.2.1. The second way
uses an INF file listed in an FV section that describes binary files, like the
one below:

```ini
[Defines]
  INF_VERSION               = 0x00010017
  BASE_NAME                 = Logo
  FILE_GUID                 = 7BB28B99-61BB-11D5-9A5D-0090273FC14D
  MODULE_TYPE               = USER_DEFINED
  VERSION_STRING            = 1.0
  EFI_SPECIFICATION_VERSION = 0x00020000

[Binaries.common]
  BIN|Logo.bmp|*
```

This INF file shows that binary file `Logo.bmp` will be wrapped into the Logo
FFS file. This kind of INF file is specified using standard `INF` statement in
an FV section of the FDF file.

The third method is to list a binary INF file containing the binary data in an
FD section. If the binary specified in the INF file in this section is a BIN
type (BIN|Filename.bin) the tools will not process the file and will be
inserted at the offset specified for the region. This is equivalent of
specifying a filename using the FILE statement, but with the binary file
included from a binary module. If the file is another binary file type, such as
an FSP binary containing PatchableInModule PCDs, the tools will be able to
patch the binary file prior to adding it to the region.

### 10.3.2 Creating EFI Sections

Sections are produced by `GenSec` tool using information in FDF file of what
type and content the section must contain. Section information in FDF file
belongs to two categories: either it is a leaf section, or it is an encapsulate
section. Encapsulation sections may contain one or more leaf sections or other
encapsulate section. The leaf section information appears in the FILE statement
in Section 10.2.1, the PE32 section type for the Fat.efi file. Normally this
information is enough for `GenSec` tool, however, more information can be
specified by specifying a `[Rule]` section in the FDF file. Rules in an FDF
file, look like:

```ini
[Rule.Common.SEC]
  FILE SEC = $(NAMED_GUID) {
    TE  TE   Align = 8   |.efi
    RAW BIN  Align = 16  |.com
  }
```

The above rule stipulates that for file type `SEC` (Security) in all build
architectures, the generated FFS must contain one TE section with 8-byte
alignment and one `RAW` section with 16-byte alignment.

Different information can be specified for different section types:

```ini
[Rule.Common.PEIM]
  FILE PEIM = $(NAMED_GUID) {
    PEI_DEPEX PEI_DEPEX Optional  |.depex
    TE        TE                  |.efi
    UI        STRING = "$(MODULE_NAME)" Optional
    VERSION   STRING = "$(INF_VERSION)" Optional BUILD_NUM = $(BUILD_NUMBER)
  }
```

The above rule stipulates that for file type `PEIM` in all build
architectures, the generated FFS may contain at most one optional `PEI_DEPEX`
section, must contain one TE section, and may contain at most one UI section
with the UI string set to the INF file's module name, and at most one `VERSION`
section.

### 10.3.3 Create an Apriori File

Some firmware volumes may require, an `APRIORI` file to be created. An
`APRIORI` file is a text file containing a GUID-named list of two or more
modules in the firmware volume. The modules will be invoked or dispatched in
the order they appear in the `APRIORI` file. Only one of each PEI and DXE
Apriori file is permitted within a single Firmware Volume. Nested Firmware
Volumes are permitted, so `Apriori` files are limited to specifying the files
(and not FVs) that are within the scope of the FV image in which it is located.
It is permissible for nested FV images to have one PEI and one DXE Apriori file
per FV. Scoping is accomplished using the curly "{}" braces.

The following example demonstrates an example of multiple `APRIORI` files.

```ini
[Fv.Root]
  DEFINE NT32     = $(WORKSPACE)/EdkNt32Pkg
  DEFINE BuildDir = $(OUTPUT_DIRECTORY)/$(PLATFORM_NAME)/ $(TARGET)_$(TOOL_CHAIN_TAG)
  APRIORI DXE {
    FILE DXE_CORE = B5596C75-37A2-4b69-B40B-72ABD6DD8708 {
      SECTION COMPRESS {
        SECTION PE32 = $(BuildDir)/X/Y/Z/B5596C75-37A2-4b69-B40B-72ABD6DD8708-DxeCore.efi
        SECTION VERSION "1.2.3"
      }
    }
    INF VERSION = "1" ${NT32)/Dxe/WinNtThunk/Cpu/Cpu.inf
  }

  FILE FV_IMAGE = EF41A0E1-40B1-481f-958E-6FB4D9B12E76 {
    SECTION GUIDED 3EA022A4-1439-4ff2-B4E4-A6F65A13A9AB {
      SECTION FV_IMAGE = Dxe {
        APRIORI DXE {
          INF a/a/a.inf
          INF a/c/c.inf
          INF a/b/b.inf
        }
        INF a/d/d.inf
        ...
      }
    }
  }
```

In the example above, there are three FFS files in the `Fv.Root` and one
Encapsulated FV image. The build tools will create an `APRIORI` file that will
dispatch the `DXE_CORE` first, then the CPU module second. In the FV image,
named by the GUID `EF41A0E`..., there will be at least five FFS files, the
`APRIORI` file, named Dxe, listing the GUID names of modules `a.inf`, `c.inf`
and `b.inf`, which will be dispatched in this order. Once complete, the `d.inf`
module may be dispatched.

### 10.3.4 Create FFS Files from Leaf Sections

Section 9.2 shows the INF and FILE statements in an FDF to describe FFS files
that will be placed into FV. The `FILE` statement is straight forward, letting
you know how an FFS file is organized, as it contains section information
within its scope. The `INF` statement, on the other hand, will use a particular
`RULE` that is determined by the module type in the INF and specified build
architecture.

The `[Rule]` section of the FDF file is used to define custom rules. Custom
rules may also be applied to a given INF file listed in an `[FV]` section. The
`[Rule]` section is also used to define rules for module types that permit the
user to define the content of the FFS file - when an FFS type is not specified
by either PI or UEFI specifications.

The Rules can have multiple modifiers as shown below.

`[Rule.$(ARCH).$(MODULE_TYPE).$(TEMPLATE_NAME)]`

If no `$(TEMPLATE_NAME)` is given then the match is based on `$(ARCH)` and
`$(MODULE_TYPE)` modifiers. BINARY is a reserved TEMPLATE_NAME as the default
rule name for binary modules. The `$(TEMPLATE_NAME)` must be unique to the
`$(ARCH)` and `$(MODULE_TYPE)`. It is permissible to use the same
`$(TEMPLATE_NAME)` for two or more `[Rule]` sections only if the `$(ARCH)` and
the `$(MODULE_TYPE)` listed are different for each of the sections.

A `[Rule]` section is terminated by another section header or the end of file.

The content of the `[Rule]` section is based on the `FILE` and section grammar
of the FV section. The difference is the `FILE` referenced in the `[RULE]` is a
`MACRO`. The section grammar is extended to include an optional argument,
`Optional`. The `Optional` argument is used to say a section is optional, that
is to say if it does not exist it's O.K.

The generic form of the entries for leaf sections is:

`<SectionType> <FileType> [Options] [{<Filename>} {<Extension>}]`

When processing the FDF file, the rules apply in the following order:

1. If `<SectionType>` not defined or not a legal name, then error
2. If `<FileType>` not defined or not a legal name, then error
3. If `[FilePath/FileName],` then:
   * Add one section to FFS with a section type of `<SectionType>`
4. Else:
   * Find all files defined by the INF file whose file type is `<FileType>`
     and add each one to the FFS with a section type of `<SectionType>`

   * Add files defined in `[Sources]` followed by files defined in `[Binaries]`
5. If more than 1 `UI` section in the final FFS file, then error
6. If more than 1 `VER` section in the final FFS file, then error
7. If more than 1 `DXE_DEPEX` section in final the FFS file, then error
8. If more than 1 `PEI_DEPEX` section in the final FFS file, then error
9. If more than 1 `SMM_DEPEX` section in the final FFS file, then error.

### 10.3.5 Create Encapsulation Sections

There are two types of encapsulation sections, a `COMPRESSION` section and the
GUIDED section. A `COMPRESSION` section uses standard UEFI
compression/decompression mechanisms. Other compression schemes must use the
`GUIDED` form of encapsulation section.

The `COMPRESS` encapsulation section uses the following format.

```c
SECTION COMPRESS [type] {
  SECTION EFI_SECTION_TYPE = FILENAME
  SECTION EFI_SECTION_TYPE = "string"
}
```

The `[type]` argument is optional, only `EFI_STANDARD_COMPRESSION` is supported
by the PI specification. The current EDK enumerations for compression are a
violation of the PI specification, and `SECTION GUIDED` must be used instead.

The `EFI_SECTION_TYPE` and `FILENAME` are required sub-elements within the
compression encapsulation section. for most sections, however both the `VERSION`
(`EFI_SECTION_VERSION`) and UI (`EFI_SECTION_USER_INTEFACE`) may specify a
string that will be used to create an EFI section.

The `GUIDED` encapsulation section uses one of the following formats.

```c
SECTION GUIDED $ (GUID_CNAME) [auth] {
  SECTION EFI_SECTION_TYPE = FILENAME
  SECTION EFI_SECTION_TYPE = "string"
}
SECTION GUIDED $ (GUID_CNAME) [auth] FILENAME
```

The required argument is the `GUIDED` name followed by an optional `auth`
flag. If the argument `auth` flag is specified, then the attribute
`EFI_GUIDED_SECTION_AUTH_STATUS_VALID` must be set.

For statements that do not use a scoping notation, (the second `SECTION`
statement of the two listed above), if `FILENAME` exists, the attribute
`EFI_GUIDED_SECTION_PROCESSING_REQUIRED` must be set to `TRUE`. The file
pointed to by `FILENAME` is the data. If `FILENAME` does not exist
`EFI_GUIDED_SECTION_PROCESSING_REQUIRED` is cleared and normal leaf sections
must be used.

**GenSec** tool uses information from these encapsulated section definition as
input parameters to generate the corresponding section format.
