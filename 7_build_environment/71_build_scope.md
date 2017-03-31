<!--- @file
  7.1 Build Scope

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

## 7.1 Build Scope

This section of the document describes the some of the rules that control what
gets built and rules that the build system uses when parsing meta-data files.

### 7.1.1 The precedence of what (platform or module) gets built

1. Content of the current working directory.  If the current working directory
   contains an INF file, then only the module is built in the context of an
   `ACTIVE_PLATFORM`, otherwise the following apply.
2. **build.exe** option statements (command-line options)
3. `target.txt` file's `ACTIVE_PLATFORM` statement

**********
**Note:** There are two different options, the `-p` option specifies the
`ACTIVE_PLATFORM` to be used for a build, so that if the current working
directory contains a module INF file, then the module will be built in the
context of the `ACTIVE_PLATFORM`.
**********
**Note:** If the INF file is not listed in the `ACTIVE_PLATFORM`'s DSC file,
the build will result in an error. The `-m` option is used to specify building
an individual module (in the context of the `ACTIVE_PLATFORM`.
**********

If the `ACTIVE_PLATFORM` value is not set using the methods above, then, if the
current working directory contains a DSC file, then the platform is built
(unless a specific module is specified by the **-m** option on the command line).

If the user attempts to build a module that is not part of the current
`ACTIVE_PLATFORM`, the build system should provide an appropriate error message
and the build should break.

### 7.1.2 The precedence of the TARGET value

It is possible to build more than one `TARGET` (i.e., `DEBUG`, `RELEASE`,
`NOOPT`, etc.) with a single build command. The precedence of the `TARGET`
value is:

1. **build.exe -b TARGET** option statements.  One or more of the **-b TARGET**
   options may be specified on the command line of the **build** tool.
2. `TARGET` statement in the `target.txt` file
3. DSC file's `BUILD_TARGETS` statement

### 7.1.3 The precedence of the TARGET_ARCH values

The target architectures that are specified on the command-line override the
`TARGET_ARCH` entry in the `target.txt` file. The resulting architecture must
also be listed as one of the architectures in the `SUPPORTED_ARCHITECTURES`
entry in the DSC file's `[Defines]` section. If the target architecture is not
specified on the command-line and the `TARGET_ARCH` entry does not exist in the
`target.txt` file, then all valid architectures specified in the DSC file, for
which tools are available, will be built.

**********
**Note:** If a module's INF file does not contain a `[Sources]` or
`[Sources.common]` section, and does contain a `[Sources.IA32]` section, then the
module is only valid for IA32 builds. The module will not be built for other
architectures.
**********

If an architecture set on a command line or specified in the `target.txt` file
is not in the list of the DSC file's `SUPPORTED_ARCHITECTURES` statement, the
command will fail.

### 7.1.4 Third Party tools using -t TOOL_CHAIN_TAG

It is possible to specify a different set of third party tools using the **-t**
`TOOL_CHAIN_TAG` option to the build command. This option takes precedence over
the `target.txt` file setting.

1. build command **-t** `TOOL_CHAIN_TAG` option
2. `target.txt` file `TOOL_CHAIN_TAG` statement

If the `TOOL_CHAIN_TAG` is not specified on the command-line nor in the
`target.txt` file, the build system will break with an error.

### 7.1.5 Precedence of Build Option FLAGS values

The flags needed by third party tools can be specified on a file, module or
platform basis. The default flags provided in the `tools_def.txt` file are for
size optimization. These flags may be modified to provide better debugging
capability. The precedence of the `FLAGS` values for third party tools follows.
The reasoning behind this precedence is that flags are appended to a single
line from the lowest to highest, with third party tools using the right most
option. If a flag line for the Microsoft compiler contains **/O1** (specified in
the `tools_def.txt` file) and **/Od** (for example, from the DSC file's
`[BuildOptions]` section), then the compiler only recognizes the **/Od** flag.

Flag entries can be defined in the INF and DSC files to replace all previous
flags by using two equal signs as in the following example:

`GCC:*_*_X64_NASM_FLAGS == -f elf32`

The following is the precedence list for flag entries, and as such, the would
be processed in reverse order.

1. Highest - DSC file, INF `<BuildOptions>` section statements
2. DSC file, `[BuildOptions.<arch>.<codebase>.<moduletype>]` section statements
3. DSC file, `[BuildOptions.<arch>.<codebase>]` section statements
4. DSC file, `[BuildOptions.<arch>]` section statements
5. DSC file, `[BuildOptions]` section statements
6. INF file, `[BuildOptions]` section statements
7. Lowest - `tools_def.txt` file `_FLAGS` statements

The following demonstrates the way tools process flags statements.

```
CCFLAGS = ToolsDef.CC_FLAGS + INF.BuildOptions + DSC.BuildOptions.CC_FLAGS + DSC.Inf.BuildOptions
```

The DSC and INF specifications define the "==" character string as a
replacement rather than append. This allows the INF file to replace the all
options specified in the `tools_def.txt` file, and also allows the platform DSC
to override all options specified in either the INF or `tools_def.txt` file.

**********
**Note:** Most tools will process the flag values from left to right, with the
right most of a duplicated flag taking priority over identical flags that are
to the left. This includes the **-D** option of the build command.
**********
