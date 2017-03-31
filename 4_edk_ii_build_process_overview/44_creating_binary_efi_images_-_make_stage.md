<!--- @file
  4.4 Creating Binary EFI Images - $(MAKE) stage

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

## 4.4 Creating Binary EFI Images - $(MAKE) stage

Binary EFI images are created in two steps; the first step uses 3rd party
assemblers, compilers and linkers to generate a PE32/PE32+/COFF image file,
while the second step uses code from the GenFw application provided in EDK II
to modify the PE32/PE32+/COFF image file to create an EFI file with an
`EFI_IMAGE_SECTION_HEADER` structure. Since different `EFI_SECTION` types may
require different values for alignment offsets, the GenFw tool must specify the
component type, which is derived from the INF metadata's ModuleType statement.

This stage is executed by the build tool calling either the `NMAKE` or the MAKE
tool for each module. The Makefiles (`Makefile` or `GNUMakefile`) are created
during the metadata processing stage. The Makefiles specify the compiler,
linker, assembler, and `GenFw` tool commands and options. Once all modules have
been built by the 3rd party tools, the build tool will call the `GenFds`
application to initiate the third stage of a build, if there is a Flash
Definition File (FDF) specified in the DSC file. If no FDF file is specified,
then the build will terminate with the creation of individual module EFI
formatted (EFI) images.
