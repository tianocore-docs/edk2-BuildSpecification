<!--- @file
  4.6 File Specifications

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

## 4.6 File Specifications

The EDK II Build is used to generate UEFI and PI compliant images. Additional
reference modules may conform to Intel Framework Specifications only if there
are no applicable UEFI or PI specification modules.

The EDK II Build Tools will only generate UEFI/PI compliant images.

The EDK II Compatibility Package provides libraries and header files to permit
building some* EDK Libraries and EDK Components referenced in an EDK II
platform (DSC) file.

**********
**Note:** \* indicates any EDK libraries or components that do not include
assembly files and do not access flash memory can use the EDK II compatibility
Package
**********

For some development activities, the EDK II Compatibility package can be used
to develop and maintain original EDK platforms, components and libraries. This
package also provides all of the tool source code used in the EDK. These tools
are for building components and platform using the original EDK code. None of
these tools is used for the EDK II build process. This EDK Compatibility
package can also be used to generate files conforming to earlier releases of
EFI and UEFI specifications.

This build specification does not cover the tools or build processes for EDK
builds nor tools provide by the EDK II Compatibility Package.

The binary image files generated at the end of the $(MAKE) stage conform to the
UEFI Images section of the _UEFI specification_. UEFI uses a subset of the
PE32+ image format with a modified header signature. The PE32/PE32+ files are
modified by the GenFw application.

**********
**Note:** This application will also modify an ELF image and generate a
PE32/PE32+ image.
**********

Each PE32/PE32+ file will have sections of the original "DOS Header"
over-written, a new NT_HEADER (for the PeHeader) and possibly one Optional
Header for 32-bit or 64-bit options.
