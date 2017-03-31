<!--- @file
  12.1 Building for Debug

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

## 12.1 Building for Debug

The build tool defaults support three building targets: NOOPT, DEBUG and
RELEASE. This section describes how to enable DEBUG target when building and
how to setup compiler flags used for DEBUG. The NOOPT target disables all
optimizations in addition to setting the flags for DEBUG.

There are three ways provided in build tool to define the target that will be
used in the building process.

* Situation A: Setup by overriding file "target.txt"

  - After executing "edksetup" , there will be a file named "target.txt" under
    `$(WORKSPACE)/Conf`.

  - Users can edit this file and change the value of item `TARGET`.

  - A specific example of this is `TARGET = DEBUG`, which sets the current building method.

  - In this example, the default value of the `TARGET` is set to `DEBUG`.

* Situation B: Use a parameter `-b BUILDTARGET` when executing building command

  - Users can type a command with the format `build -b BUILDTARGET` to specify
    the target used in current building.

  - A specific example of this command is `build -b DEBUG`.

  - In this example, the value set in the file `target.txt` will be ignored.

* Situation C: Setup in the DSC file of a platform

  - When the `BUILDTARGET` is not specified in the command line or in the file
    `target.txt`, the build tool will attempt to build all valid targets
    specified in the DSC file.

  - This contrasts with situations A and B, where only the targets specified as
    valid in the DSC file can be used.

### 12.1.1 Debugging Files

For a debugging build, the files created will be saved to
`$(WORKSPACE)/$(OUTPUT_DIRECTORY)/$(BUILDTARGET)_$(TOOL_CHAIN_TAG)/$(ARCH)/`.
For each single module, the temporary files created in `DEBUG` building process
will be saved to
`$(WORKSPACE)/$(OUTPUT_DIRECTORY)/$(BUILDTARGET)_$(TOOL_CHAIN_TAG)/$(ARCH)/$(PACKAGE_NAME)/$(MODULE_NAME)/DEBUG/`
and `$(WORKSPACE)/$(OUTPUT_DIRECTORY)/$(BUILDTARGET)_$(TOOL_CHAIN_TAG)/$(ARCH)/$(PACKAGE_NAME)/$(MODULE_NAME)/OUTPUT/`,
so such as .map, .pdb and other `DEBUG` files can be found in these two directories.

User can also define a specific directory to save `DEBUG` files. A detailed
example is given in the next subsection.

### 12.1.2 Debugging Options

Build tool supports customized `DEBUG` flags in the `<BuildOptions>` section of
the DSC file, the INF file and `tools_def.txt`. The highest priority for a same
complier flag is the one defined in INF file, the medium is that in DSC file
and the lowest is the one in `tools_def.txt`.

For example, to generate the .cod files for the .obj files of a platform,
user can add one line such as `*_MYTOOLS_*_CC_FLAGS = /FAcs /FA $(OUTPUT_DIR)\`
in section `[BUILD_OPTIONS]` of DSC file. This option tells build tool to
generate a .cod file for each .obj file and put them to $(OUTPUT_DIR).

For only generating the .cod files for one single module, one way is to add
the option in section `[BUILD_OPTIONS]` of the module's INF file; another way is
to add the option to DSC file's `<BuildOptions>` for the INF file like below:

```
MdeModulePkg/Universal/PCD/Pei/Pcd.inf {
  <BuildOptions>
    *_MYTOOLS_*_CC_FLAGS = /FAcs /FA $(OUTPUT_DIR)\
}
```

### 12.1.3 Advanced Debugging

For generating disassembly (.cod) files for debugging, the following is one
way to setup **dumpbin -disasm** for individual modules as well as using it for
every .efi file generated.

To generate the disasm for the efi files, the user can add two definitions in
`tools_def.txt`:

```
DEBUG_MYTOOLS_IA32_DISASM_PATH = DEF(VS2005TEAMSUITE_BIN)\dumpbin.exe
DEBUG_MYTOOLS_IA32_DISASM_FLAGS = -dump -disasm -out:$(DEST_DIR)
```

Then user can use build option `-D`, `--define` with a reserved MACRO name:
DISASM to start building. The build tool automatically detects if a **DISASM**
tool defined in the TagName of Tool Chain, then after ever link command that
generates an EFI file, the tool will run the **DISASM** tool (with the flags)
against the EFI file. In the example, the output file will be next to the EFI
file based on the FLAGS entry, **-out:$(DEST_DIR)** which is the same location
as the `.efi` file.
