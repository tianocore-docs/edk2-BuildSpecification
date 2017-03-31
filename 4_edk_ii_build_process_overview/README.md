<!--- @file
  4 EDK II Build Process Overview

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

# 4 EDK II Build Process Overview

The EDK II build system is used to process EDK II meta-data files, EDK II
source and/or binary files and some legacy EDK components and libraries. The
code-base for EDK II content can be obtained from various sources or various
distribution methods. The EDK II build system provides the UEFI Distribution
Packaging Tool (UEFIPT) that can be used to create, install or remove UEFI
distribution packages. The UEFI distribution package format does not depend on
any specific build system. However, the UEFIPT must be used within the context
of the EDK II build system.

The EDK II EdkCompatibilityPkg in the EDK II source tree provides backward
compatibility for existing EDK components and platforms; using EDK processes
and tools will not be described in this document. Some EDK components may be
built using the EDK II build tools, where those components are included in an
EDK II platform file. The exact list of EDK components, or the compatible
component types are not provided here - other EDK II documentation contains
information on using EDK components with EDK II.
