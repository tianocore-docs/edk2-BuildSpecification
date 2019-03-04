<!--- @file
  8.5 Auto-generated Makefiles

  Copyright (c) 2008-2019, Intel Corporation. All rights reserved.<BR>

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

## 8.5 Auto-generated Makefiles

The actual build actions are done via **"MAKE"** system. This system is
**"nmake"** in Windows environment and **"make"** in GCC (Linux and Mac OS/X)
environment. The `Makefiles` are created at the module level. For one platform,
one makefile is generated for each tool chain, build target
(`DEBUG`/`RELEASE`/`NOOPT`) and architecture.

In Platform mode, the `build` tool calls the build script tool (**nmake** or
**make**) for each Module's `Makefile`.

In Module mode, the **build** tool calls the build script tool, giving the
Module `Makefile` as an argument. However, in Module Mode, if the build tool
target is "fds", after the module builds successfully, the build tool calls the
GenFds tool to regenerate an FD file.

### 8.5.1 Module Makefile

This section describe the formats of the individual component/module Makefiles.
Users may generate a custom makefile for their EDK II module based on the information 
provided by this section.

The module `Makefile` is composed by two parts: macro definitions and target
definitions.

In the pseudo-code provided, the MACRO, `$(MODULE_BUILD_DIR)` is constructed
using the following rules:

* If the .dsc file's `OUTPUT_DIRECTORY` value (path) starts with an alpha
  character, the value of the `OUTPUT_DIRECTORY` statement is relative to the
  directory specified in the system environment variable, `WORKSPACE`.
  Otherwise, it is considered an absolute directory path.

```c
If (isalpha (getValue ("OUTPUT_DIRECTORY", DscFile)[0]) {
  MOD_BUILD_DIR = "$WORKSPACE)\" + getValue("OUTPUT_DIRECTORY", DscFile)
} else {
  MOD_BUILD_DIR1 = getValue ("OUTPUT_DIRECTORY", DscFile)
}
Foreach Target in ActiveTargetList {
  Foreach ToolChainTag in ActiveToolChain {
    MOD_BUILD_DIR2 = $ (MOD_BUILD_DIR1) + "\" + Target + "_" + ToolChainTag + "\";
    foreach Arch in ActiveArchList {
      MODULE_BUILD_DIR = $ (MOD_BUILD_DIR2) + Arch + "\";
      MODULE_BUILD_DIR += getDirPart (InfFile) + "\";
      MODULE_BUILD_DIR += getValue ("BASE_NAME", InfFIle) + "\";
      MAKEFILE = $ (MODULE_BUILD_DIR) + "Makefile";
      genModuleMakefile ($ (MAKEFILE));
      addModuleToList ($MAKEFILE, MakefileList);
    }
    genTopMakefile ($ (MOD_BUILD_DIR2) + "Makefile")
  }
}
```

#### 8.5.1.1 Macro definitions

##### 8.5.1.1.1 Platform information

These come from `[Defines]` section in the DSC file.

```c
MakefileList = $ (PLATFORM_MAKEFILE)
Foreach InfFile {
  MakefileList += $ (MODULE_MAKEFILE)
}
foreach Makefile in MakefileList {
  include_statement ($ (MODULE_BUILD_DIR)\Makefile, "
    PLATFORM_NAME = getValue("PLATFORM_NAME", DscFile);
    PLATFORM_GUID = getValue ("PLATFORM_GUID", DscFile);
    PLATFORM_VERSION = getValue ("PLATFORM_VERSION", DscFile);
    PLATFORM_RELATIVE_DIR = getDirPart (ActivePlatform);
    PLATFORM_DIR = "$(WORKSPACE)\" + getDirPart(ActivePlatform);
    pLATFORM_OUTPUT_DIR = getValue ("OUTPUT_DIRECTORY", DscFile);
  ");
}
```

#### Example

```
PLATFORM_NAME = NT32
PLATFORM_GUID = EB216561-961F-47EE-9EF9-CA426EF547C2
PLATFORM_VERSION = 0.3
PLATFORM_RELATIVE_DIR = Nt32Pkg
PLATFORM_DIR = $(WORKSPACE)\Nt32Pkg
PLATFORM_OUTPUT_DIR = Build\NT32
```

##### 8.5.1.1.2 Module information

These come from `[Defines]` section in the INF file and `[Components]` section
in DSC file.

```c
Foreach InfFile {
  include_statement ($ (MODULE_BUILD_DIR)\Makefile, "
    MODULE_NAME = getValue ("BASE_NAME", InfFile)
    MODULE_GUID = getValue ("FILE_GUID", InfFIle)
    MODULE_VERSION = getValue ("VERSION_STRING", InfFile)
    MODULE_TYPE = getValue ("MODULE_TYPE", InfFile);
    MODULE_FILE_BASE_NAME = getValue ("BASE_NAME", InfFile)
    BASE_NAME = $ (MODULE_NAME)
    MODULE_RELATIVE_DIR = getDirPart (InfFile)
    MODULE_DIR = "$(WORKSPACE)\" + getDirPart(InfFile)

  ");
}
```

#### Example

```
MODULE_NAME = HelloWorld
MODULE_GUID = 6987936E-ED34-44db-AE97-1FA5E4ED2116
MODULE_VERSION = 1.0
MODULE_TYPE = UEFI_APPLICATION
MODULE_FILE_BASE_NAME = HelloWorld
BASE_NAME = $(MODULE_NAME)
MODULE_RELATIVE_DIR = MdeModulePkg\Application\HelloWorld
MODULE_DIR = $(WORKSPACE)\MdeModulePkg\Application\HelloWorld
```

##### 8.5.1.1.3 Build configuration

These come from `$(WORKSPACE)/Conf/target.txt`, command line options, or
`[Defines]` section in DSC file.

```
ARCH = IA32
TOOLCHAIN_TAG = MYTOOLS
TARGET = DEBUG
```

##### 8.5.1.1.4 Build directories

These are determined by build tools. Macro `DEST_DIR_OUTPUT` and
`DEST_DIR_DEBUG` are generated for backward compatibility.

```
PLATFORM_BUILD_DIR = $(WORKSPACE)\Build\NT32
BUILD_DIR = $(WORKSPACE)\Build\NT32\DEBUG_MYTOOLS
BIN_DIR = $(BUILD_DIR)\IA32
LIB_DIR = $(BIN_DIR)
MODULE_BUILD_DIR = $(BUILD_DIR)\IA32\MdeModulePkg\Application\HelloWorld\HelloWorld
OUTPUT_DIR = $(MODULE_BUILD_DIR)\OUTPUT
DEBUG_DIR = $(MODULE_BUILD_DIR)\DEBUG
DEST_DIR_OUTPUT = $(OUTPUT_DIR)
DEST_DIR_DEBUG = $(DEBUG_DIR)
```

##### 8.5.1.1.5 Tools flags,

These are used to concatenate the flags from different places in the predefined
order. The order makes sure that the flags defined DSC file can override flags
in INF file and default ones. In the code example below, the tools will expand
the values into a single line - `$(TOOLS_DEF_LZMA_FLAGS)` does not appear in the
Makefile, only the flag values appear.

```
LZMA_FLAGS    = $(TOOLS_DEF_LZMA_FLAGS) $(INF_LZMA_FLAGS) $(DSC_LZMA_FLAGS) $(DSC_INF_LZMA_FLAGS)
PP_FLAGS      = $(TOOLS_DEF_PP_FLAGS) $(INF_PP_FLAGS) $(DSC_PP_FLAGS) $(DSC_INF_PP_FLAGS)
SLINK_FLAGS   = $(TOOLS_DEF_SLINK_FLAGS) $(INF_SLINK_FLAGS) $(DSC_SLINK_FLAGS) $(DSC_INF_SLINK_FLAGS)
CC_FLAGS      = $(TOOLS_DEF_CC_FLAGS) $(INF_CC_FLAGS) $(DSC_CC_FLAGS) $(DSC_INF_CC_FLAGS)
APP_FLAGS     = $(TOOLS_DEF_APP_FLAGS) $(INF_APP_FLAGS) $(DSC_APP_FLAGS) $(DSC_INF_APP_FLAGS)
VFRPP_FLAGS   = $(TOOLS_DEF_VFRPP_FLAGS) $(INF_VFRPP_FLAGS) $(DSC_VFRPP_FLAGS) $(DSC_INF_VFRPP_FLAGS)
DLINK_FLAGS   = $(TOOLS_DEF_DLINK_FLAGS) $(INF_DLINK_FLAGS) $(DSC_DLINK_FLAGS) $(DSC_INF_DLINK_FLAGS)
ASM_FLAGS     = $(TOOLS_DEF_ASM_FLAGS) $(INF_ASM_FLAGS) $(DSC_ASM_FLAGS) $(DSC_INF_ASM_FLAGS)
TIANO_FLAGS   = $(TOOLS_DEF_TIANO_FLAGS) $(INF_TIANO_FLAGS) $(DSC_TIANO_FLAGS) $(DSC_INF_TIANO_FLAGS)
MAKE_FLAGS    = $(TOOLS_DEF_MAKE_FLAGS) $(INF_MAKE_FLAGS) $(DSC_MAKE_FLAGS) $(DSC_INF_MAKE_FLAGS)
ASMLINK_FLAGS = $(TOOLS_DEF_ASMLINK_FLAGS) $(INF_ASMLINK_FLAGS) $(DSC_ASMLINK_FLAGS) $(DSC_INF_ASMLINK_FLAGS)
ASL_FLAGS     = $(TOOLS_DEF_ASL_FLAGS) $(INF_ASL_FLAGS) $(DSC_ASL_FLAGS) $(DSC_INF_ASL_FLAGS)
```

##### 8.5.1.1.6 Tools path

These come from the file specified by `TOOL_CHAIN_CONF` definition in
`$(WORKSPACE)/Conf/target.txt`.

```
LZMA    = H:\dev\AllPackagesDev\IntelRestrictedTools\Bin\Win32\LzmaCompress.exe
PP      = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\cl.exe
SLINK   = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\lib.exe
CC      = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\cl.exe
APP     = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\cl.exe
VFRPP   = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\cl.exe
DLINK   = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\link.exe
ASM     = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\ml.exe
TIANO   = TianoCompress.exe
MAKE    = C:\Program Files\Microsoft Visual Studio 8\Vc\bin\nmake.exe
ASMLINK = C:\WINDDK\3790.1830\bin\bin16\link.exe ASL = C:\ASL\iasl.exe
```

##### 8.5.1.1.7 Shell commands

These are used to make sure that the file operations for both nmake and GNU
make system become as the same as possible.

```ini
# shell commands for nmake
RD = rmdir /s /q
RM = del /f /q
MD = mkdir
CP = copy /y
MV = move /y

# shell commands for gnu make
RD = rm -r -f
RM = rm -f
MD = mkdir -p
CP = cp -u -f
MV = mv -f
```

##### 8.5.1.1.8 Source files and target files list macro

In these, `<FILE_TYPES>` macros are generated from
`$(WORKSPACE)/Conf/build_rule.txt` and files listed in `[Sources]` section
in INF file, "`INC`" macro is generated from `[Includes]` section in DEC file
and `[Packages]` section in INF file, "`LIBS`" macro is generated from
`[LibraryClasses]` section in INF file and DSC file, and "`COMMON_DEPS`"
macro is generated by parsing recursively the "`#include`" preprocessor
directives in source code files.

```
C_CODE_FILES = $(WORKSPACE)\MdeModulePkg\App\Hello\HelloWorld.c
DYNAMIC_LIBRARY_FILE_LIST = $(DEBUG_DIR)\$(MODULE_NAME).dll
UNKNOWN_TYPE_FILE_LIST = $(DEBUG_DIR)\$(MODULE_NAME).efi
OBJECT_FILE_LIST = $(OUTPUT_DIR)\HelloWorld.obj
STATIC_LIBRARY_FILE_LIST = $(OUTPUT_DIR)\$(MODULE_NAME).lib

INC = <include search path list>
LIBS = <dependent library file list>
COMMON_DEPS = <header file list>
```

##### 8.5.1.1.9 Target macros

In these `CODA_TARGET` is generated according to the last rule(s) in rule
chains defined in `$(WORKSPACE)/Conf/build_rule.txt`.

```
INIT_TARGET = init
CODA_TARGET = $(DEBUG_DIR)\$(MODULE_NAME).efi
```

#### 8.5.1.2 Target definitions

##### 8.5.1.2.1 "all" target

Default target which actually executes against the "`mbuild`" target.

##### 8.5.1.2.2 "pbuild" target

Target which is used to build the source files of current module only. It's
always used in top-level makefile because the libraries will be built above all
non-library modules.

`pbuild: $(INIT_TARGET) $(CODA_TARGET)`

##### 8.5.1.2.3 "mbuild" target

Actual default target which is used for single module build mode. Because in
single module build mode the top-level `Makefile` will not be called, the build
system has to build libraries that the current module needs in module's
`Makefile`. "`mbuild`" target is used for this purpose.

```
mbuild: $(INIT_TARGET) gen_libs $(CODA_TARGET)
gen_libs:
    cd $(BUILD_DIR)\X64\MdePkg\Library\DxePcdLib\DxePcdLib && "$(MAKE)"
$(MAKE_FLAGS)
    cd $(BUILD_DIR)\X64\MdePkg\Library\BaseLib\BaseLib && "$(MAKE)"
    cd $(MODULE_BUILD_DIR)
```

##### 8.5.1.2.4 "init" target

Target used to print verbose information and create necessary directories used
for build.

```
init:
    -@echo Building ... $(MODULE_NAME) $(MODULE_VERSION) [$(ARCH)] in platform $(PLATFORM_NAME) $(PLATFORM_VERSION)
    -@if not exist $(DEBUG_DIR) mkdir $(DEBUG_DIR)
    -@if not exist $(OUTPUT_DIR) mkdir $(OUTPUT_DIR)
```

##### 8.5.1.2.5 Miscellaneous build targets

Targets which are used to build source files to object files and then in turn
into final `.lib` file, `.efi` file or other files. These targets are generated
according to the rule chains in `$(WORKSPACE)/Conf/build_rule.txt`. For example:

```
$(OUTPUT_DIR)\ModuleFile.obj : $(COMMON_DEPS)
    "$(CC)" /Fo$(OUTPUT_DIR)\ModuleFile.obj $(CC_FLAGS) $(INC) $(WORKSPACE)\MyPlatformPkg\MySubDir\ModuleFile.c

$(OUTPUT_DIR)\$(MODULE_NAME).lib : $(OBJECT_FILE_LIST)
    "$(SLINK)" $(SLINK_FLAGS) /OUT:$(OUTPUT_DIR)\$(MODULE_NAME).lib $(OBJECT_FILE_LIST)

$(DEBUG_DIR)\$(MODULE_NAME).dll : \
    $(OUTPUT_DIR)\$(MODULE_NAME).lib $(LIBS) $(MAKE_FILE)
    "$(DLINK)" /OUT:$(DEBUG_DIR)\$(MODULE_NAME).dll $(DLINK_FLAGS) $(DLINK_SPATH) $(LIBS) $(OUTPUT_DIR)\$(MODULE_NAME).lib

$(DEBUG_DIR)\$(MODULE_NAME).efi : $(DEBUG_DIR)\$(MODULE_NAME).dll
    GenFw -e $(MODULE_TYPE) -o $(DEBUG_DIR)\$(MODULE_NAME).efi $(DEBUG_DIR)\$(MODULE_NAME).dll
    $(CP) $(DEBUG_DIR)\$(MODULE_NAME).efi $(OUTPUT_DIR)
    $(CP) $(DEBUG_DIR)\$(MODULE_NAME).efi $(BIN_DIR)
    -$(CP) $(DEBUG_DIR)\*.map $(OUTPUT_DIR)

$(OUTPUT_DIR)\AutoGen.obj : \
$(WORKSPACE)\Build\MyPlatform\DEBUG_ICC\X64\MyPlatformPkg\MyModDir\MyModDir\DEBUG\AutoGen.c
    "$(CC)" /Fo$(OUTPUT_DIR)\AutoGen.obj $(CC_FLAGS) $(INC) $(WORKSPACE)\Build\MyPlatform\DEBUG_ICC\X64\MyPlatformPkg\MyModDir\MyMod Dir\DEBUG\AutoGen.c
```

##### 8.5.1.2.6 clean, cleanall, cleanlib

Targets used to delete part or all files generated during build.

```
clean:
    if exist $(OUTPUT_DIR) rmdir /s /q $(OUTPUT_DIR)

cleanall:
    if exist $(DEBUG_DIR) rmdir /s /q $(DEBUG_DIR)
    if exist $(OUTPUT_DIR) rmdir /s /q $(OUTPUT_DIR)
    del /f /q *.pdb *.idb > NUL 2>&1

cleanlib:
    cd $(BUILD_DIR)\X64\MdePkg\Library\DxePcdLib\DxePcdLib && \
      "$(MAKE)" $(MAKE_FLAGS) cleanall
    cd $(BUILD_DIR)\X64\MdePkg\Library\BaseLib\BaseLib && \
      "$(MAKE)" $(MAKE_FLAGS) cleanall
    cd $(MODULE_BUILD_DIR)
```
