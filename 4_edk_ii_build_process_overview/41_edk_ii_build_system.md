<!--- @file
  4.1 EDK II Build System

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

## 4.1 EDK II Build System

EDK II build system produces binary images that conform to UEFI and PI
specification file formats. In some cases, the tools have been extended to
follow the _Intel Innovation Framework for EFI Specifications_. Some binary
content may also follow other industry standard specifications, such as ACPI
and PCI specifications.

While the build system may seem complex, it was designed to be extremely
flexible.

The original build system worked on files within a development `WORKSPACE`. All
content for the build had to be located within the `WORKSPACE` directory tree.

The build system has been updated to allow the setting of multiple paths that
will be searched when attempting to resolve the location of EDK II packages.
This new feature allows for more flexibility when designing a tree layout or
combining sources from different sources. The new functionality is enabled
through the addition of a new environment variable: `PACKAGES_PATH`.

The `PACKAGES_PATH` variable is an ordered list of additional search paths using
the default path separator of the host OS between each entry ( ";" on Windows,
":" on Linux and OS/X). The path specified by the `WORKSPACE` variable always has
the highest search priority over any `PACKAGE_PATH` entries. The first path (left
to right) in the `PACKAGES_PATH` list has the highest priority and the last path
has the lowest priority.

As soon as a match has been found the build tools will stop searching.

The output of the build system may be located outside of the development
workspace. The `WORKSPACE`, `PACKAGES_PATH` and `EDK_TOOLS_BIN` system
environment variables contain directory paths that must never contain space
characters even though they are permitted by the operating system.

The build system works in the context of a platform, using the Platform
Description (DSC) file to define what will get built. When building a single
driver, or an application, the DSC file is used to define what will be built,
along with the libraries, configuration settings and custom build flags.

All ASCII source files (see Table 14) in the EDK II code base must use the
DOS CRLF character sequence for the end-of-line terminator except those that
are strictly for GCC, such as assembly files that are only to be processed by
\*NIX tools that use an extension of ".s" (lower case). Unicode files use the
UCS-2 character set.

The Base Tools ASCII source files (C and Python) must use the DOS CRLF
character sequence for the end-of-line terminator as well as the DOS batch
files with an extension of `.bat`. The \*NIX shell scripts identified by an
extension of .sh as well as the scripts in `BaseTools/BinWrappers/PosixLike` and
the `BaseTools/Bin/CYGWIN_NT-5.1-i686` directories must always use the Linux LF
character for the end-of-line terminator. Apple's Mac\* OS/X operating system
correctly translates the Linux LF character into the native CR character for
the end-of-line terminator.

### 4.1.1 Development Environments

The EDK II development environments include Windows\*, Linux\* and OS/X\*
development workstations. Development workstations must be running an IA32 or
X64 operating system. Intel(R) Itanium Processor Family workstations are not
supported.

The new build tools allow directories containing EDK II packages to be located
anywhere on a developer's workstation. The recommended method for setting up a
development structure on a Windows workstation is create a directory in the
root of a drive:

```
mkdir C:\Work
cd C:\Work
set WORKSPACE=%CD%
```

The `edk2` directory can then be placed in this directory and additional
directories for platforms and tools should be placed in the top level directory
as well:

```
C:\Work\edk2
C:\Work\MyPlatform
C:\Work\edk2-BaseTools-win32
set PACKAGES_PATH=C:\Work\edk2;C:\Work\MyPlatform
set EDK_TOOLS_BIN=C:\Work\edk2-BaseTools-win32
```

In order to complete the setup:

```
C:\Work> mkdir Conf
C:\Work> edk2\edksetup.bat
```

After running this command, the configuration files, `target.txt`,
`tools_def.txt` and `build_rule.txt` will be placed in the `C:\Work\Conf`
directory.

The EDK II Build output directory is typically created in the `WORKSPACE`
directory (based on configuration set in the DSC files). After executing a
**build.exe** command, the `C:\Work\Build` directory will be created.

When using this feature, remember the system environment variables,
`WORKSPACE`, `PACKAGES_PATH` and `EDK_TOOLS_BIN` must be set _before_ running
the `edksetup.bat` script.

### 4.1.2 Supported Development Tools

The list of validated Third-Party Compiler Tool chains that can be used with
the EDK II build system is documented in the `tools_def.template` file in the
EDK II source tree's `BaseTools/Conf` directory.

_Install the tool chains for compilers and/or additional tools prior to
building any image_.

### 4.1.3 Build Process Restrictions

The build process for all development environments must be identical, with the
caveat that only applicable EDK II Packages need compile for any given
operating system.

**********
**Note:** All EDK II content is built in the context of a Platform, using a
Platform Description (DSC) file to describe what needs to be built, as well as
any customization needed for a build. The DSC file is required even though the
target may be only an application, a PCI Option ROM or a binary UEFI driver.
**********

System Environment Variables will not be overridden by tools. System
Environment Variable names cannot be overloaded - only the value of the System
Environment Variable will be used.

There is no restriction on the location of the `EDK_TOOLS_PATH`, it may be
located within a directory identified as the `WORKSPACE` directory, or in any
other location that is accessible on the development workstation.

When using multi-directory feature, the system environment variables,
`WORKSPACE`, `PACKAGES_PATH` and `EDK_TOOLS_BIN` must be set before running the
`edksetup.bat` script.
