<!--- @file
  1 Introduction

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

# 1 Introduction

## 1.1 Overview

This document describes the EDK II Build Architecture. This specification was
designed to support new build requirements for building EDK II modules and EDK
components within the EDK II build infrastructure as well as to generate binary
firmware images and Unified Extensible Firmware Image (UEFI) applications.

EDK II Build utilities described in this document use INI style text based
meta-data files to describe components, modules, libraries, platforms, firmware
volumes and firmware device images.

This document describes the high level EDK II Build Architecture, which has the
following goals:

**_Compatible_**

The EDK II build environment must maintain backward compatibility with the
existing EDK files. This means that the changes made to this specification must
not require changes to existing files.

Compatibility is maintained by providing the EDK Tools in the EDK Compatibility
package. Also, some INF files may require modification for the EDK II build
environment if they are used to access Flash firmware - the PI 1.0
specification modified the flash data structures and defined some new GUID
values.

EDK II Build system tools must test the format of the EDK II meta-data files.
The EDK II build tools must provide an error if, during parsing of the EDK II
meta-data files, a version of the files is encountered that is higher than the
version of the files that the tools support.

**_Simplified platform build and configuration_**

One goal of this format is to simplify the build setup and configuration for a
given platform. It was also designed to simplify the process of adding EDK and
EDK II firmware components to a firmware volume on a given platform.

**_Specification Conformance_**

The EDK II Build infrastructure supports building UEFI 2.5 and PI 1.4 compliant
platforms. Existing EDK components may need to be updated to align with these
specifications.

###### Table 1 EDK Build Infrastructure Support Matrix

|                    | EDK DSC | EDK II DSC | EDK FDF | EDK II FDF | EDK INF | EDK II INF |
| ------------------ |:-------:|:----------:|:-------:|:----------:|:-------:|:----------:|
| EDK Build Tools    | YES     | NO         | YES     | NO         | YES     | NO         |
| EDK II Build Tools | NO      | YES        | NO      | YES        | YES     | YES        |

## 1.2 Target Audience

This document is intended for persons performing EFI development and support
for different platforms.

