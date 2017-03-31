<!--- @file
  4.3 Pre-Build Stage Overview

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

## 4.3 Pre-Build Stage Overview

This section provides an overview of the first three meta-data files that are
used to control different aspects of the build. There are three files of
interest, `build_rule.txt`, `tools_def.txt` and `target.txt`. The next chapter
defines the format of the `tools_def.txt` and `target.txt` files. The
`build_rule.txt` file is not documented here, as it is internal to the EDK II
build system. See the `build_rule.txt` file for additional information.

### 4.3.1 target.txt

The `$(WORKSPACE)/Conf/target.txt` file is created the first time `edksetup`
script is run. The default version of this file is the
`$(EDK_TOOLS_PATH)/Conf/target.template` file.

The `target.txt` file is used as a filter, allowing a developer to build items
of interest without having to build everything. The variables set in this file
include the target platform, the target architecture, the tool chain and
pointers to the tool chain configuration and build rule files. If no options
are provided on the build command-line, values from this file are used to
determine what to build, what tool chain will be used and where to obtain the
rules for processing the files.

### 4.3.2 tools_def.txt

The `$(WORKSPACE)/Conf/tools_def.txt` file is created the first time the
`edksetup` script is run. The default version of this file is the
`$(EDK_TOOLS_PATH)/Conf/tools_def.template`.

The `tools_def.txt` describes sets of user configurable paths, commands and
default flags for external tools (as well as optional tools provided with the
build system). Since advanced developers may have multiple versions of tool
chains installed, this file allows setting up paths and flags for different
tool chains. Each tool chain is identified by a unique name.

#### 4.3.2.1 "Best Known Safe" flags

The `tools_def.txt` file flags for the supported (tested) compiler tool chains
that contain "Best Known Safe" flags for generating libraries, drivers and
applications. These flags are set for speed optimization. The build system does
provide for modifying flags for individual modules and platforms, so flags may
be modified for better debug ability.

### 4.3.3 build_rule.txt

The `$(WORKSPACE)/Conf/build_rule.txt` file is created the first time the
`edksetup` script is run. The default version of this file is the
`$(EDK_TOOLS_PATH)/Conf/build_rule.template`.

The `build_rule.txt` file is used by the tools to define how different file
types are processed. This includes how different files and module types are
compiled, as well as how the build tools manipulate the binary image files.
Normally, users should not make changes to this file.

### 4.3.4 Parse EDK II Meta-Data - AutoGen stage

Once the build system knows the basic information needed for the build, the
build system parses the additional EDK II meta-data files. The meta-data in the
EDK II code base is stored in text-based, INI-style, documents. Refer to the
_EDK II INF Specification_, _EDK II DSC Specification_,
_EDK II DEC Specification_, and _EDK II Flash Description File Specification_
to see the format of these files.

The DSC file is the first of the EDK II meta-data files that gets parsed. This
file provides a list of the other EDK II meta-data files that need to be
parsed. The DSC file may be parsed twice in order to resolve PCD values that
are used are used in conditional directive statements.

The contents of current working directory (at the time the build command is
executed) may alter what gets built. If the working directory contains one INF
file, only the module gets built. This is useful when debugging a driver, as
only the one module will be rebuilt. If more than one INF file exists, you will
need to use an command-line option to select which INF file you want to build.
(The INF filename must be listed in the `ACTIVE_PLATFORM` DSC file's
`[Components]` section.)

**********
**Note:** More than one INF file may exist in a module directory, however the
BASENAME and `FILE_GUID` for these INF files must be different if both modules
will be included in a single FV for platform.
**********
**Note:** The build system has also been modified to support building multiple
versions of a single INF using the format defined in the DSC specification.
This permits having multiple versions of a module linked against different
libraries.
**********

The second file to be parsed will be the FDF file if one is specified in the
DSC file or on the command-line. While the FDF file specified what content gets
assembled into the final firmware device image, PCD values from this file may
be required for building specific modules specified in the DSC file.

**********
**Note:** INF files that are listed in the DSC file must include the package,
MdePkg/MdePkg.dec in order to build properly (even if the module does not
contain C files).
**********

The parse stage creates individual module and library `AutoGen.c`, `AutoGen.h`
and _Makefiles_. Since EDK II supports Microsoft, Intel and GCC complier tool
chains, the Microsoft Build Tool, **NMAKE**/`Makefile` is for Windows developer
Workstations using Microsoft or Intel tool chains on a Microsoft Windows
operating system development workstation. For UNIX based development
workstations, the GCC build tool, **MAKE**/`GNUmakefile` is used.

All third party tools and flags for those tools get expanded in the generated
Makefiles. The following is an example of makefile statements that support this
mode.

```
...
PP      = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\cl.exe
CC      = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\cl.exe
APP     = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\cl.exe
VFRPP   = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\cl.exe
DLINK   = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\link.exe
PCH     = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\cl.exe
ASM     = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\ml.exe
TIANO   = TianoCompress.exe
SLINK   = C:\Program Files\Microsoft Visual Studio .NET 2003\Vc7\bin\lib.exe
ASMLINK = C:\WINDDK\3790.1830\bin\bin16\link.exe ASL = C:\ASL\iasl.exe
...
DEFAULT_PP_FLAGS      = /nologo /E /TC /FI$(DEST_DIR_DEBUG)/AutoGen.h
DEFAULT_CC_FLAGS      = /nologo /W4 /WX /Gy /c /D UNICODE /O1ib2 /GL / FI$(DEST_DIR_DEBUG)/AutoGen.h /EHs-c- /GF /Gs8192 /Zi /Gm
DEFAULT_APP_FLAGS     = /nologo /E /TC
DEFAULT_VFRPP_FLAGS   = /nologo /E /TC /DVFRCOMPILE /FIAutoGen.h
DEFAULT_DLINK_FLAGS   = /NOLOGO /NODEFAULTLIB /IGNORE:4086 /OPT:REF /OPT:ICF=10 /MAP /ALIGN:32 /MACHINE:I386 /LTCG /DLL /ENTRY:$(ENTRYPOINT) /SUBSYSTEM:CONSOLE /SAFESEH:NO /BASE:0 /DRIVER /DEBUG /PDB:$(DEST_DIR_DEBUG)/$(BASE_NAME).pdb
DEFAULT_PCH_FLAGS     = /nologo /W4 /WX /Gy /c /D UNICODE /O1ib2 /GL /FI$(DEST_DIR_DEBUG)/AutoGen.h /EHs-c- /GF /Gs8192 /Fp$(DEST_DIR_OUTPUT)/AutoGen.h.gch /Yc /TC /Zi /Gm
DEFAULT_ASM_FLAGS     = /nologo /W3 /WX /c /coff /Cx /Zd /W0 /Zi
DEFAULT_TIANO_FLAGS   =
DEFAULT_SLINK_FLAGS   = /nologo /LTCG
DEFAULT_ASMLINK_FLAGS = /link /nologo /tiny
DEFAULT_ASL_FLAGS     =
...
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj : $(COMMON_DEPS)
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj :
$(WORKSPACE)\MdePkg\Include\Library\DebugLib.h
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj : $(WORKSPACE)\MdePkg\Include\Library\BaseLib.h
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj :
$(WORKSPACE)\MdePkg\Include\Library\TimerLib.h
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj :
$(WORKSPACE)\MdePkg\Include\Library\BaseMemoryLib.h
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj : $(WORKSPACE)\MdePkg\Include\Library\PcdLib.h
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj :
$(WORKSPACE)\MdePkg\Library\BaseLib\Ia32\WriteGdtr.c
$(OUTPUT_DIR)\Ia32\WriteGdtr.obj :
$(WORKSPACE)\MdePkg\Library\BaseLib\BaseLibInternals.h
  "$(CC)" /Fo$(OUTPUT_DIR)\Ia32\WriteGdtr.obj $(CC_FLAGS) $(INC) $(WORKSPACE)\MdePkg\Library\BaseLib\Ia32\WriteGdtr.c

$(OUTPUT_DIR)\Ia32\WriteDr3.obj : $(COMMON_DEPS)
$(OUTPUT_DIR)\Ia32\WriteDr3.obj :
$(WORKSPACE)\MdePkg\Library\BaseLib\Ia32\WriteDr3.c
  "$(CC)" /Fo$(OUTPUT_DIR)\Ia32\WriteDr3.obj $(CC_FLAGS) $(INC)
$(WORKSPACE)\MdePkg\Library\BaseLib\Ia32\WriteDr3.c
...
```

One makefile is generated for each module under a combination of
`$(TARGET)_$(TOOL_CHAIN_TAG)` and `$(ARCH)`, package name, module directory,
directory name and the `BASE_NAME` of the module's INF file.

#### Parse DSC file

1. Obtain platform FixedAtBuild and FeatureFlag PCD values and Macro values
   that are used in the Conditional Directives - if the value of a Macro,
   FixedAtBuild PCD or FeatureFlag PCD used in the conditional directives
   cannot be determined, the build will break with an appropriate error
   message. The use of FixedAtBuild or FeatureFlag PCD names which are defined
   by SET statements in the FDF file cannot be used in conditional directive
   statements in the DSC file.

2. Second pass over the DSC files will crush out any conditional directives
   where the feature flag expression used in the conditional directive is False.

3. Obtain platform PCD values which will go into the individual module
   AutoGen.h files where needed.

   * If `VPD_TOOL_GUID` was specified in the DSC file's `[Defines]` section,
     the build processes is suspended while the tool specified by the GUID is
     called after the build process generates a text file containing the VPD
     PCDs. If the tool returns successfully (an exit code of 0), the build
     system parses the name of the 'map' file that contains an ordered list
     of VPD PCDs.

   * There are some PCD values that get set in the FDF file, listed in binary
     INF files or listed in source INF files, so generating the C files is
     delayed until all PCD values have been finalized.

4. Obtain the FDF filename and obtain the Flash related PCDs from the FDF file.
   FeatureFlag and FixedAtBuild PCD names which are defined in the DSC file can
   be used in conditional directives within the FDF file.

5. For each component listed in the DSC file, parse the Module's INF file

   * Create a directed graph list of the EDK II Library Instances that will be
     used for the EDK II modules.

   * Create the PEI, DXE or SMM DEPEX file

   * Create the Library Instance's AutoGen.c files containing PCD, Guid,
     Protocol, Ppi and EntryPoint definitions and data structures. PCD values
     come from FDF file, DSC's INF scoped section, DSC's global PCD sections,
     default values in the INF file's PCD section, or the DEC file's default
     values.

   * Create the module's library instance Makefiles

     + Individual modules may require different compilation options, over-riding
       any global definitions.

   * Create the AutoGen.c files containing PCD, Guid, Protocol, Ppi and
     EntryPoint definitions and data structures.

   * Create any Strings header file required for VFR processing.

     + The VFR file name cannot be same as a C file name in a module directory.
       If so, the same output files will be generated and overwritten.
       (A.vfr --> A.c --> A.obj, A.c --> A.obj)

   * Create the module Makefiles

   Individual modules may require different compilation options, over-riding any
   global definitions. If an INF file is not listed in the DSC file and is listed
   in the FDF file, the parsing tools must check if the INF in the FDF file
   contains `PatchableInModule` or `DynamicEX` entries. If the INF lists other PCD
   access methods (FeatureFlag,

   FixedAtBuild or Dynamic), and the INF contains files listed in a `[Sources]`
   section and does not contain a `[Binaries]` section, then the build tools must
   break the build with an appropriate error message.

6. The tools are also responsible for creating binary files containing all
   `DynamicEx` PCDs that are listed in the DSC, FDF and Binary INF files
   (listed in the FDF file).
   These binaries are automatically placed into the (PEIM and DXE) PCD driver FFS
   files.

7. If the build option, --ignore-sources is present on the build command-line,
   none of the source files listed in a [Sources] section will be processed,
   even if the module is listed in the DSC file and no files (AutoGen.h,
   AutoGen.c or Makefile) will be generated; the INF must be treated as a
   Binary only INF file.
