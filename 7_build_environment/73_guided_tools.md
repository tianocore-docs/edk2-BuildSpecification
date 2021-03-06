<!--- @file
  7.3 GUIDed Tools

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

## 7.3 GUIDed Tools

The `tools_def.txt` file also allows for specifying tools by GUID. For custom
guided sections specified in the FDF file, the `tools_def.txt` file must
specify a GUID that matches the GUID used in the FDF. Using compression, such
as LZMA, CRC32 or TianoCompress, as an example, the entries in the
`tools_def.txt` must define an entry for the tool as well as implementing a
decompression library in the code-base. Since the build system provides these,
the entries in the `tools_def.txt` file are:

```
*_*_*_LZMA_PATH  = LzmaCompress
*_*_*_LZMA_GUID  = EE4E5898-3914-4259-9D6E-DC7BD79403CF
*_*_*_CRC32_PATH = GenCrc32
*_*_*_CRC32_GUID = FC1BCDB0-7D31-49AA-936A-A4600D9DD083
*_*_*_TIANO_PATH = TianoCompress
*_*_*_TIANO_GUID = A31280AD-481E-41B6-95E8-127F4C984779
```

The GUIDed compression tools must use the **-e** option to encode (compress) a
file and the **-d** option to decode (decompress) a file.

### 7.3.1 PCD VPD Data

PCDs defined in the DSC file may be defined as Dynamic VPD or DynamicEx VPD.
VPD data is typically located in a separate data section of the FDF file. VPD
data also requires that each PCD be located at a known offset from the start of
the data region.

VPD data is common to all modules, therefore one and only one value can be
defined for a given PCD/SKU in the DSC file.

The DSC file permits automatic assignment for these VPD PCDs when using an
external tool, such as the provided **BPDG** tool, by specifying a offset value
of "*".

Just calling the tool to create the binary data file is not enough, as the
offset value must also be used in a header file for the PEI and DXE PCD
drivers. In order to interrupt the build system prior to auto-generating the
header files, a special entry in the DSC file, `VPD_TOOL_GUID` identifies the
GUIDed tool in defined in the `tools_def.txt` file. The format for the VPD
GUIDed tool in the `tools_def.txt` file is:

```
*_*_*_VPDTOOL_PATH = BPDG
*_*_*_VPDTOOL_GUID = 8C3D856A-9BE6-468E-850A-24F7A8D38E08
```

When using the BDPG tool provided, the build tools will create the
8C3D856A-9BE6468E-850A-24F7A8D38E08.txt file with the PCD names, SKU names,
offsets, values and size, in the build output FV directory. The VPDTOOL will be
called using the two flags and the file generated by the build system as an
argument. Any custom tool that is used to create the VPD data must support
these two flags and take the input text file that was generated by the build
system.

`<Tool> -o Filename.bin -m Filename.map Filename.txt`

Once the tool completes, if the PCD offsets have been calculated by the tool,
the map file generated by the tool will be read by the build system and will be
used to create the header files required by the PCD drivers and binary file to
be included in the flash image.

**********
**Note:** The word "Filename" in the above example will be replaced by the
`VPDTOOL_GUID` value, as in: `8C3D856A-9BE6-468E-850A-24F7A8D38E08.txt`.
**********

The format for the file created by the build system and the format of the map
file required by the build system is provided in the appendix, VPD Tool.

#### 7.3.1.1 Using VPD Data for PCD Values

1. Modify the DSC file:

  * Add "VPD_TOOL_GUID =8C3D856A-9BE6-468E-850A-24F7A8D38E08" to the [Defines]
    section.
  * Add at least one [PcdsDynamicVpd] or [PcdsDynamicExVpd] section.
  * For numeric or boolean PCDs allowing the tool to determine offsets
    automatically, add an entry for each PCD using the following format:
    ```
    <PcdTokenSpaceGuidCName>.<PcdCname> | * | <Value>
    ```
  * For `VOID*` PCDs allowing the tool to determine offsets and reserved size
    automatically, add an entry for each PCD using the following format:
    ```
    <PcdTokenSpaceGuidCName>.<PcdCname> | * | <MaxSize> | <Value>
    ```

  If using automatic offset feature, the build tools byte-align numeric values,
  while `VOID*` PCD types will be aligned using the following rules:

  * ASCII strings, "string" or 'string', will be byte aligned.
  * Unicode strings, L"string" or L'string' will be two-byte aligned.
  * Byte arrays, {0x00, 0x01} will be 8-byte aligned.

  If the developer manually assigns offset values in the DSC file, the developer
  must follow the same rules.

  **********
  **Note:** If a developer manually sets the offset of a `VOID*` PCD with
  Unicode string, L"string"/L'string' style to a value that is not 2-byte aligned,
  then an error is generated and the build halts.
  **********
  **Note:** If a developer manually sets the offset of a `VOID*` PCD with byte
  array {} style to a value that is not 8-byte aligned, then a warning is
  generated, but the build will continue.
  **********

2. Modify the FDF file:

  * Create a Region for storing the VPD binary data in the [FD] section, the
    offset and size of the region must be specified.
  * The starting address of the VPD data region must be 8-byte aligned (the
    BaseTools must halt with an appropriate error message if the address is not
    correctly aligned).
  * The PcdVpdBaseAddress PCD must be specified immediately after the region
    declaration.
  * Add the FILE statement to the region with the name of the VPD  binary
    generated by the VPD tool.

  ```ini
    # VPD Data Region
    0x0026D000|0x00001000
    gEfiMdeModulePkgTokenSpaceGuid.PcdVpdBaseAddress

    FILE = $(OUTPUT_DIRECTORY)/$(TARGET)_$(TOOL_CHAIN_TAG)/FV/8C3D856A-9BE6468E-850A-24F7A8D38E08.bin
  ```
