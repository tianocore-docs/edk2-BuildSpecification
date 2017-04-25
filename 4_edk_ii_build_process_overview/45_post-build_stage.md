<!--- @file
  4.5 Post-Build Stage

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

## 4.5 Post-Build Stage

### 4.5.1 Assemble FLASH Images - ImageGen stage

Once all the modules' EFI image files have been created, the final stage of a
build process is called. For FLASH images, this stage uses the FDF file, and
some parts of the DSC file to create the final binary image files. This stage
processes the individual EFI files, formatting them into leaf `EFI_SECTION`
types and combining them using implied rules (or custom rules) into different
firmware files (FFS), firmware volumes (FVs) and the final FLASH images (FDs).
The construction of these images is based on the content of the FDF file (with
a very limited amount of data being obtained from the DSC file).

A binary file with a file type of `DISPOSABLE` will not be placed into a
`EFI_SECTION_DISPOSABLE` encapsulation section. This keyword is used by UEFI
Packaging Tool to ensure that files, such as debug symbol files, get packaged
correctly.

The default build rules specify removal of `.reloc` sections of the PE32/PE32+
file for all `SEC`, `PEI_CORE` and `PEIM` modules and components. To prevent
removal of the .reloc section, a module developer will need to specify a
keyword, `SHADOW` in the INF file.

Assuming that all FD and FV images are going to be generated (based on the
default value of the top-level build command), for each FV image specified the
following must occur.

1. The FDF file is parsed to create a directed graph structure for each FV
   image, so that all leaf EFI sections are created first. During this stage,
   INF files that contain a `[Binaries]` section and that do not contain a
   `[Sources]` section will be processed. An INF file that contains a
   `[Binaries]` section that contains an entry that starts with `DISPOSABLE`,
   that entry must be ignored - these files are not to be placed into an
   `EFI_SECTION_DISPOSABLE` encapsulation section.

2. If an INF not listed in the DSC file, but is listed in the FDF file and the
   INF contains a `[PatchPcd]` section, the tools must test to determine if the
   PCD is listed in the DSC (or FDF) file, and whether the value listed in the
   DSC (or FDF) file is different from the value in the INF file. If the value
   is different, the tools must patch the binary .efi file with the value from
   the FDF or DSC file prior to creating the EFI leaf section.

3. The tools are also responsible for creating binary files containing all
   DynamicEx PCDs that are listed in the DSC, FDF and Binary INF files (listed
   in the FDF file). These binaries are automatically placed into the (PEIM and
   DXE) PCD driver FFS files.

4. These leaf sections are either put into encapsulated sections or put
   directly into an FFS file following the implied rules (or user defined
   rules) defined later in this document.

5. As each FFS File is created, the file is either encapsulated into another
   FFS file or appended to an FV image.

6. Once all of the FFS files have been placed into an FV image file, the FV
   file is put into an FD file at the location specified by the FD section of
   the FDF file.

### 4.5.2 EFI PCI Expansion Option ROM Images

There are two methods for creating an option ROM image, when the FDF file is
specified and when an FDF file is not present. To build from source and no FDF
file is present, if the module's INF file contains the keywords,
`PCI_DEVICE_ID`, `PCI_VENDOR_ID` and `PCI_CLASS_CODE`, the build will terminate
after creating EFI files - there will be no call to the GenFds tool. These key
words also force the creation of an option ROM image, after the EFI files have
been created, using the EfiRom program to create the EFI PCI Expansion ROM
image. If an FDF is present, then the build tools will parse the FDF file
looking for an `[OptionRom]` section, and create the option ROM based on the
contents of this section. Note that the FDF specification permits adding binary
images, such as the legacy option rom binary, as well as support for multiple
architecture driver images to the option ROM image.

A binary flag, `PCI_COMPRESS`, when set to true, tells the tools to use EFI
standard compression to compress the entire option ROM image.

Option ROM images are always created in the output FV directory.

### 4.5.3 UEFI Applications

When building only UEFI applications, no FDF file is specified in the DSC file;
the build would normal terminate after creating EFI files - there would be no
call to the GenFds tool. Using an option on the build tool command line to
specify building a UEFI Application forces the parsing stage to generate the
module's `Makefile` with an alternate path. This path will force the creation
of a UEFI application, after the EFI files have been created, using UEFI
application specific arguments for the `GenFds` tool.
