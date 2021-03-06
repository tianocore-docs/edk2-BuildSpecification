<!--- @file
  10 Post-Build ImageGen Stage - FLASH

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

# 10 Post-Build ImageGen Stage - FLASH

This chapter describes the processing of the EFI files generated by the $(MAKE)
Stage into FLASH binary images. Some of the PCDs defined or used in conditional
directives in the FDF are set in the platform's DSC file. The tools must make
at least one pass over the DSC file to get PCD values for conditional
directives and other PCD entries used in the FDF file. If a FeatureFlag or
FixedAtBuild PCD value, used in a conditional directive, cannot be determined
the build must break.

For the remainder of this chapter, the `WORKSPACE` and `$(WORKSPACE)` refer to
the ordered list of directories specified by the combination of
`WORKSPACE + PACKAGES_PATH`.

### 10.0.1 ImageGen File Extensions

Table 19 and Table 20 describe intermediate file extensions and final file
extensions in the ImageGen stage of the build for a platform. The ImageGen
stage takes the output of the $(MAKE) stage (typically the `.efi` files) and
converts the files into EFI section files using the **GenSec** tool. The next
step combines the section files into FFS files using the **GenFfs** tool. Once
the Ffs files have been generated, they are combined into an FV image file
using the **GenFv** tool. FV image files are combined into FD image files by the
**GenFds** tool (which also controls all of the other steps in this stage).

Binary files listed in the FDF file's [FD] region section are included without
processing. This allows for the addition of VPD data files (generated during
the AutoGen Stage) to be included in the FD output file.

###### Table 19 GenFds Image Generation: Intermediate File Extensions

| Input Extension                       | Output Extension | Description                                                                                  |
| ------------------------------------- | ---------------- | -------------------------------------------------------------------------------------------- |
| .efi                                  | .pe32            | EFI_SECTION_PE32                                                                             |
| .pe32, .ui, .ver                      | .com             | EFI_SECTION_COMPRESSION                                                                      |
| .ui                                   | .ui              | EFI_SECTION_USER_INTERFACE                                                                   |
| .depex                                | .dpx             | EFI_SECTION_PEI_DEPEX or EFI_SECTION_DXE_DEPEX                                               |
| .tmp, .sec                            | .guided          | EFI_SECTION_GUID_DEFINED                                                                     |
| .ver                                  | .ver             | EFI_SECTION_VERSION                                                                          |
| .acpi, .aml, .bin, .bmp               | .raw             | EFI_SECTION_RAW                                                                              |
| ANY                                   | SAME as Input    | EFI_SECTION_FREEFORM_SUBTYPE_GUID                                                            |
| .com, .dpx, .guided, .pe32, .ui, .ver | .ffs             | FFS file images                                                                              |
| .ffs                                  | .fv              | Firmware Volume Image files                                                                  |
| .fv                                   | .sec             |                                                                                              |
| .txt                                  | .mcb             | Microcode Binary File generated from the Microcode text files                                |
| .map                                  | .bin             | VPD binary image file (created by VPD_TOOL) where the file name is the GUID of the VPD tool. |

###### Table 20 ImageGen Final Output File Extensions

| Input Extensions | Output Extension | Description                |
|:----------------:|:----------------:| -------------------------- |
| .fv, .mcb        | .fd              | Firmware Device Images     |
| .efi, .pe32      | .rom             | UEFI PCI Option ROM Images |

For UEFI compliant PCI Option ROMs, the EfiRom tool is used to process `.efi`
or `.pe32` files into the `.rom` file.

For UEFI applications, the `.efi` file generated at the end of the $(MAKE)
stage can be used directly, or, if the application will be included as part of
a flash device image (all of the shell applications) the `.efi` file is
processed using the standard steps for including a driver in an image.
