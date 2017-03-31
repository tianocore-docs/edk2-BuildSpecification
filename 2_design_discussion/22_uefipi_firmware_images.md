<!--- @file
  2.2 UEFI/PI Firmware Images

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

## 2.2 UEFI/PI Firmware Images

UEFI and PI specifications define the standardized format for EFI firmware
storage devices (FLASH or other non-volatile storage) which are abstracted into
"Firmware Volumes". Build systems must be capable of processing files to create
the file formats described by the UEFI and PI specifications. The tools
provided as part of the EDK II BaseTools package process files compiled by
third party tools, as well as text and Unicode files in order to create UEFI or
PI compliant binary image files. In some instances, where UEFI or PI
specifications do not have an applicable input file format, such as the Visual
Forms Representation (VFR) files used to create PI compliant IFR content, tools
and documentation have been provided that allows the user to write text files
that are processed into formats specified by UEFI or PI specifications.

A Firmware Volume (FV) is a file level interface to firmware storage. Multiple
FVs may be present in a single FLASH device, or a single FV may span multiple
FLASH devices. An FV may be produced to support some other type of storage
entirely, such as a disk partition or network device. For more information
consult the _Platform Initialization Specification_, Volume 3.

In all cases, an FV is formatted with a binary file system. The file system
used is typically the Firmware File System (FFS), but other file systems may be
possible in some cases. Hence, all modules are stored as "files" in the FV.
Some modules may be "execute in place" (linked at a fixed address and executed
from the ROM), while others are relocated when they are loaded into memory and
some modules may be able to run from ROM if memory is not present (at the time
of the module load) or run from memory if it is available.

Files themselves have an internally defined binary format. This format allows
for implementation of security, compression, signing, etc. Within this format,
there are one or more "leaf" images. A leaf image could be, for example, a PE32
image for a DXE driver.

Therefore, there are several layers of organization to a full UEFI/PI firmware
image. These layers are illustrated below in Figure 1. Each transition
between layers implies a processing step that transforms or combines previously
processed files into the next higher level. Also shown in Figure 1 are the
reference implementation tools that process the files to move them between the
different layers.

![](../media/image1.png)

###### Figure 1 UEFI/PI Firmware Image Creation

In addition to creating images that initialize a complete platform, the build
process also supports creation of stand-alone UEFI applications (including OS
Loaders) and Option ROM images containing driver code. Figure 2, below, shows
the reference implementation tools and creation processes for both of these
image types.

![](../media/image2.png)

###### Figure 2 EFI PCI Expansion Option ROM and UEFI Application Creation

The final feature that is supported by the EDK II build process is the creation
of Binary Modules that can be packaged and distributed for use by other
organizations. Binary modules do not require distribution of the source code.
This will permit vendors to distribute UEFI images without having to release
proprietary source code.

This packaging process permits creation of an archive file containing one or
more binary files that are either Firmware Image files or higher (EFI Section
files, Firmware File system files, etc.). The build process will permit
inserting these binary files into the appropriate level in the build stages.
