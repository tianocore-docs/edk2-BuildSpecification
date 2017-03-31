<!--- @file
  8.6 Binary Modules

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

## 8.6 Binary Modules

EDK II accommodates distribution of binary module code for inclusion into a
firmware volume. This feature is used by vendors who have a proprietary code
base, but need to provide their customers with the ability to use that code in
a platform. Vendors may protect their IP by distributing only module code in
either lib, bin, or efi format, without distributing debug files or sources.

No Makefile is generated for binary only modules.

A binary module must have a `[Binaries]` section. It is recommended that binary
INF files not be listed in DSC file so that the build tools will not try to do
a module build for a binary module. The INF file of a binary module is always
put in FDF file for flash image generation. The binary files can also be
referenced directly in FDF. Please refer to Section 10 (Post-Build ImageGen
Stage - FLASH Images) for details.

Binary modules are used only with FDF files unless a PCD using
PatchableInModule access method is used by the binary module and the platform
developer wants to change the value for this PCD in the binary module.

The build command has an option flag, `--ignore-sources`, that will treat all
INF files listed in the DSC file as though they were binary INF files. The
build will not generate any Makefiles, totally ignoring any files listed in a
`[Sources]` section. If a module is specified in the DSC file that does not
contain a [Binaries] section, the build will provide an appropriate error
message and terminate. This mode allows distribution of binary modules with
source files that can be used during debugging.
