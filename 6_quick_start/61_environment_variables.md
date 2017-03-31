<!--- @file
  6.1 Environment Variables

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

## 6.1 Environment Variables

There are two required system level environment variables that must be set, and
several optional environment variables.

### 6.1.1 Required Environment Variables

The first of the two required variables is `WORKSPACE`. This variable points to
a directory that will contain a Conf directory (containing the text files that
are used to control build options) and the typical Build output directory tree.
The following two lines are an example of setting this variable, the first in a
Microsoft Windows\* Command Prompt Window, while the second represents setting
the variable in a UNIX terminal bash shell.

```
set WORKSPACE=C:\MyWork\Proj1\edk2
export WORKSPACE=/usr/local/src/proj1/edk2
```

The second required environment variable, `EDK_TOOLS_PATH`, required points to
the directory containing the Conf directory for the BaseTools directory. The
EDK II project contains a BaseTools directory,that contains setup scripts,
template files and XML Schema files. Only one copy of the `BaseTools`
directories needs to be installed on a workstation (although multiple copies
are permitted, such as having one in each workspace). The `EDK_TOOLS_PATH`
variable must point to the directory containing the `BaseTools/Conf` directory.
The following lines are an example of setting this variable in a Microsoft
Windows\* Command Prompt window. The first line sets an absolute path to single
location, outside of the workspace, while the second line uses tools located
within the workspace.

```
set EDK_TOOLS_PATH=C:\Tools
set EDK_TOOLS_PATH=%WORKSPACE%\BaseTools
```

If assembly code is used by the modules and the NASM assembler is used, the
system environment variable, `NASM_PREFIX` must be set as shown below and must
include the trailing backslash character:

`set NASM_PREFIX=C:\nasm\`

### 6.1.2 Optional Environment Variables

There are two types of optional environment variables. The first type are used
for complex development trees, while the second type of optional environment
variables are needed build EDK components and libraries for use in an EDK II
platform. Some EDK components and libraries can be used without modifications,
while other EDK components and libraries will require porting to the new EDK II
development environment.

When EDK II Packages are distributed within different directory trees on a
developer's workstation, the `PACKAGES_PATH` environment variable is used to list
directories (prioritized from left to right) that contain EDK II Package
directories. The operating system delimiter, such as the semi-colon character
for Microsoft operating systems, is used to separate the directory names. If
all development is performed under the root of the edk2 source tree, this
variable is not required. The edk2 reference build system will look for EDK II
packages in the directory specified in the `WORKSPACE`, then search for the
package directory in the directories listed in the `PACKAGES_PATH`; the first
occurrence of an EDK II package found will be used.

For Microsoft windows environments, the EDK_TOOLS_BIN environment variable can
be used to point to the directory that contains the Win32 BaseTools binaries.
If these Win32 binaries are located in edk2 directory tree under the
`BaseTools\Bin\Win32` directory; this variable is not required. Since developers
using \*NIX operating systems must build the 'C'-based tools prior to using them
and run the Python based tools from source, this environment variable is not
required. The edksetup script is used to add the path to the binaries to the
system PATH environment variable.

The `EDK_SOURCE` environment variable must point to either the head of an
existing EDK directory tree (not the EDK II directory) or the EDK II's
`EdkCompatibilityPkg` directory.

Another optional environment variable, `EFI_SOURCE`, is needed if the
`EDK_SOURCE` environment variable is set and an EDK component and/or library is
located outside of the `EDK_SOURCE` tree. If these values are not set, the EDK
II build system will automatically set both values to point to the
`EdkCompatibilityPkg` directory in the `WORKSPACE`.

The final optional environment variable, `ECP_SOURCE`, is used to define the
location of the EDK Compatibility Package content for building EDK modules. If
these values are not set, the build system will automatically set the value to
the `EdkCompatibilityPkg` directory in the `WORKSPACE`.

### 6.1.3 Configuring the Environment Variables

If all development will be done within the root of the edk2 directory tree, and
the Win32 BaseTools binaries are in the BaseTools\Bin\Win32 directory, then the
edksetup script may be used to setup the development workspace by setting
system environment variables, `WORKSPACE` and `EDK_TOOLS_PATH`.

If a more complex development environment is used (multiple directories
containing EDK II Packages), then the `WORKSPACE`, `PACKAGES_PATH` and
`EDK_TOOLS_BIN` environment variables must be set before running the edksetup
script.

The three optional environment variables, `ECP_SOURCE`, `EDK_SOURCE` and
`EFI_SOURCE` which are required when building EDK libraries and components in
the context of an EDK II platform will also be set if they have not been set
previously.

The script must be executed prior to building in a new command prompt window or
new terminal shell.

Another feature of the script is that it adds the path of the build system
tools into the OS environment variable, `PATH`.
