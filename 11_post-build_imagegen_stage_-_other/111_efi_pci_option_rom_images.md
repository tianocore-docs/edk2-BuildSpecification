<!--- @file
  11.1 EFI PCI Option ROM Images

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

## 11.1 EFI PCI Option ROM Images

To generate the EFI PCI Option ROM, the EFI PE32 files and optionally the
legacy OptROM (from a separate tool) are needed.

The **EfiRom** tool is used on the PE32 and optionally the legacy Option ROM
binary images. The tool will check the header of each file to determine the
type.

* If the input file(s) are EFI PE32 image,

  - fill in EFI PCI OptROM header and PCI data structure in the output EFI PCI
    Option ROM image

  - then copy the input EFI PE32 file content to the output EFI PCI Option ROM
    image to create the EFI PCI Option ROM image.

* If the input file(s) are legacy OptROM binary image,

  - fill in EFI PCI OptROM header in the output EFI PCI Option ROM image

  - then copy the input file content to the output EFI PCI Option ROM image to
    create the EFI PCI Option ROM image.

The final image is placed in the FV folder of the build directory.
