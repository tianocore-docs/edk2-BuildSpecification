<!--- @file
  8.2 Auto-generation Process

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

## 8.2 Auto-generation Process

This section covers, in sequence, the steps taken by the `build.exe` tool. When
creating the auto-generated files, the build system must include either the "u"
suffix or the "ull" suffix (UINT64 only) to indicate that the values are
unsigned for all numeric values specified for PCDs.

### 8.2.1 Determine What to Build

The build tool will use following algorithm to determine what will be built.
The first step the build system performs is to open the `Conf/target.txt` file.

**********
**Note:** The build system tools allow for specifying an alternate location and
filename for `Conf/target.txt` on the command-line that can be either inside or
outside of the `WORKSPACE` directory tree.
**********

The following pseudo-code demonstrates how the tools obtain command-line
overrides of the information specified in `Conf/target.txt`.

```
If ("-t  <DscFile>") {
  // Command line option specified
  ActivePlatform = <DscFile>;
} ElseIf (<ACTIVE_PLATFORM> specified in $ (WORKSPACE)/Conf/target.txt) {
  ActivePlatform = <ACTIVE_PLATFORM>;
} ElseIf (one  <DscFile>   found in current working directory) {
  ActivePlatform = <DscFile>;
} Else {
  // Unable to determine the Active Platform
  if (Number of DscFiles > 1) {
    PrintError (
      "There are %s DSC files in the folder. "
      "Use '-p' to specify one.", NumDscFiles
      );
  } else {
    PrintError (
      "No active platform specified in target.txt "
      "or command line!\n Nothing to build."
      );
  }
  BreakTheBuild();
}

// Determine whether this is a module only build or the full platform
If (("-m <InfFile>") || (one <InfFile> found in working directory)) {
  // Either a command line option was specified, or one and only
  // one INF file was found in the current working directory.
  ActiveModule = <InfFile>;
  BuildMode = "SingleModuleBuild";
} Else {
  ActiveModule = NONE;
  BuildMode = "PlatformBuild";
}

Parse ( $ (WORKSPACE) / Conf / target.txt );
Parse ( ActivePlatform );

// Determine Architectures to build
If ("-a  <ArchListFromCommandLine>") {
  // command line option given
  ActiveArchList = Intersection (
                     <ArchListFromCommandLine>,
                     <ArchListFrom (ActivePlatform)>
                     );
} Else {
  ActiveArchList = Intersection (
                     <ArchListFromTarget.Txt>,
                     <ArchListFrom (ActivePlatform)>
                     );
}

If (ActiveArchList == NULL) {
  if (ArchListFromCommandLine != NULL) {
    PrintError (
      "The architecture(s) specified on the command line "
      "(%s) are not valid for the active platform (%s\n",
      ArchListFromCommandLine,
      ArchListFrom (ActivePlatform)
      );
  } else {
    PrintError (
      "The active platform cannot be built, the "
      "architectures (%s) are not supported.\n",
      ArchListFrom (ActivePlatform)
      );
  }
  BreakTheBuild();
}

// Determine the target type, such as DEBUG and/or RELEASE
If ("-b  <TargetListFromCommandLine>") {
  // command line option given
  ActiveTargetList = Intersection (
                       <TargetListFromCommandLine>,
                       <TargetListFrom (ActivePlatform)>
                       );
} Else {
  ActiveTargetList = Intersection (
                       <TargetListFromTarget.Txt>,
                       <TargetListFrom (ActivePlatform)>
                       );
}

If (ActiveTargetList == NULL) {
  if (TargetListFromCommandLine != NULL) {
    PrintError (
      "Target (%s) specified on the command line is not "
      "valid for this platform (%s).\n",
      TargetListFromCommandLine,
      TargetListFrom (ActivePlatform)
      );
  } else {
    PrintError (
      "Target (%s) is not specified in the target.txt file.\n",
      TargetListFrom (ActivePlatform)
      );
  }
  BreakTheBuild();
}

// Determine the tool chain to use for the build
If ("-t  <ToolChainTag>") {
  // command line option given
  ActiveToolChain = <ToolChainTag>
} ElseIf (<TOOL_CHAIN_TAG> specified in $ (WORKSPACE)/Conf/target.txt) {
  ActiveToolChain = <TOOL_CHAIN_TAG>
} Else {
  if (ToolChainTag != NULL) {
    PrintError (
      "Tool chain specified on the command line (%s) is "
      "not specified in the tools_def.txt file.\n",
      ToolChainTag
      );
  } else {
    PrintError (
      "Tool chain specified in target.txt (%s) is not "
      "specified in the tools_def.txt file.\n", TOOL_CHAIN_TAG
      );
  }
  BreakTheBuild();
}

Build (ActivePlatform, ActiveModule, ActiveArchList, ActiveTargetList, ActiveToolChain, BuildMode);
```

### 8.2.2 Parse File Pointed to by TOOL_CHAIN_CONF

The file specified by `TOOL_CHAIN_CONF` (in `target.txt`) is the tool chain
definition file (`tools_def.txt`) that contains all the definitions of external
tools used to build modules and platforms, in the form of "name=value". The
definition of a tool includes the path of the executable, the path of dynamic
libraries the executable needs, and command line options. Each set of tools can
be referenced by a tag name either in the command line or in `target.txt`. For
example, WINDDK3790x1830 is used to refer a set of tools from WINDDK of version
3790x1830.

The parser of the tool chain definition file needs to expand macros and wild
cards ("*") in the tool definitions. The expanded definitions are put in a
database for easier access later. For example, if one overrides a tool's
options in DSC or INF file, the tool will look up the tool's definition in the
database and append the options to the end of options in the file specified by
`TOOL_CHAIN_CONF`.

**********
**Note:** The supported third party compiler tools will use the right most (or
last) option it encounters, permitting appended options to override options
specified first. For example, specifying a compiler option (FLAG) line: **/Od /c
/Og** will result the compiler only processing **/c /Og**, ignoring the **/Od** flag.
**********

The final result after AutoGen stage is that macros named by `<TOOLCODE>` and

`<TOOLCODE>_FLAGS` will be generated in module's makefile. For example, "CC"
and "CC_FLAGS" macros will be generated in the makefile for the compiler tool.
The path of dynamic libraries will be prefixed to system's PATH environment by
the build tools, so that the tools used in the `Makefile` can be called
correctly.

### 8.2.3 Parse build_rule.txt

The file specified by `BUILD_RULE_CONF` (in `target.txt`) contains command
steps used to build the source files into intermediate files and then
intermediate files into final image files to be put into FV/FD. The type of
source files and intermediate files are determined by the file extension. That
means the same extension cannot be used to represent different file types. But
one type of file can have more than one file extension. A single file can only
have a single extension.

The parser of this file will convert the contents of the file into a build rule
database. Each item in this database will have tool chain family, input file
information, output file information and command information. Whenever a source
file is found in module's INF file, the build tools will attempt to find a
build rule in the database corresponding to the input file's extension, and
then use the output file as input file information to find another build rule,
until no build rule uses the output file information as its input file. If
there's no build rule for a type of source file, the build tools just skip it.
But if there's build rule for it, one or more makefile targets will be
generated for it.

The sequence of build rules applied to source files and intermediate files
determines the dependency relationship between targets in makefile. One type of
file cannot be used in more than one build rule as an input file and the build
rules must not be cyclic.

### 8.2.4 Parse DSC, FDF, INF, DEC files

The platform description (DSC) file is used to instruct the build system what
modules need to be processed in order to generate the PE32/PE32+ image files.

The EDK II build system tools must be located in either the path pointed to by
the EDK_TOOLS_BIN system environment variable (on Microsoft* operating systems)
or located under a subdirectory of the Bin directory of the EDK_TOOLS_PATH
directory.

* All EDK II content used to create PE32/PE32+ images must reside in the
  directory tree pointed to by the `WORKSPACE`.

* EDK content must reside in directories pointed to by the `EFI_SOURCE`,
  `EDK_SOURCE` and `ECP_SOURCE` system environment variables.

* The build system's output directory is not required to be within the
  `WORKSPACE`.

From the DSC file, the build tools collect the mapping between library classes
and library instances (INF files), PCD data for the whole platform, the list of
modules (INF files) specified for the platform, and the build output directory.
Optionally, the name of the flash image layout description (FDF) file and build
options specific to the platform are also obtained. Parsing FDF file at this
time is just for the PCD information which might be used by some modules, and
merge these PDC values into the information set of PCDs in DSC file.

A PCD entry must only be listed once per section in the DSC or FDF files.

Multiple library class instances for a single library class must not be
specified in the same `[LibraryClasses]` or `<LibraryClasses>` section in the
DSC file.

#### 8.2.4.1 !include Files

The DSC (and FDF) file can use `!include` statements to include text files that
contain content that would appear in the DSC file. When gathering the content
from the DSC (or FDF) file, the file pointed to by the !include statement is
read before any other information that appears later in the file.

The build system does not parse the files as the lines are read, but rather the
lines are all read into a buffer prior to parsing the content. Therefore, the
directory and file names for !include statements may not contain MACROs.

If only a filename is provided, the file must be located in the same directory
as the DSC or FDF file. Use of `$(WORKSPACE)/<Path>/<Filename>` is allowed
for include files outside of the directory tree containing the DSC or FDF file,
or `<Path>/<Filename>` if the include file is in the directory tree
containing the DSC or FDF file.

#### 8.2.4.2 INF and DEC Parsing

The build tools try to parse the INF file one by one, including the INF file
for library instances. From the INF file, the build tools collect information
such as source file list, library class list, package list, GUID/Protocol/PPI
list, PCD list, etc.

After all INF files are parsed, the build tools retrieve the list of all of the
dependent DEC files and then parse them. From the DEC file, the build tools
will get the information such as common include folders, the values of
GUID/Protocol/PPI, the default setting of all PCDs in the package, etc.

The `[Packages]` section of the INF file is used by the build tools during the
generation of the Makefiles. The `[Includes]` section of the DEC file specified
in the `[Packages]` section will be added to the command-lines for compiler
tools. The `MdePkg/MdePkg.dec` file must be included in all INF files listed in
the DSC file.

EDK II INF files must contain a valid name in the `MODULE_TYPE` element of
their `[Defines]` sections. If the module type is not recognized, he build
tools should break the build with an appropriate error message.

EDK INF files must contain a valid name in the `COMPONENT_TYPE` element of
their `[defines]` sections. If the component type is not recognized, the build
tools should break the build with an appropriate error message.

For entries in the [Sources] section of the INF file, in addition to the
required file name field, there are optional fields for Family, Tool chain tag
name and Tool Code that may contain modifiers that limit the scope of the file
to a specific tool chain family, such as GCC, or tool code, such as ASM. If
these fields are blank, then there is no restriction to what tools, tagname or
tool chain family will process the file. The final field is for a FeatureFlag
Expression. This field is an expression that must evaluate to True or False. If
the field cannot be evaluated (such as an undeclared PCD used in the
expression) the build parser must provide an appropriate error message and stop
the build. If the field evaluates to False, the line is ignored. If the field
evaluates to True, the build will use this line.

For entries in the [Binaries] section of the INF file, in addition to the file
type and name fields, there are optional fields for the target (DEBUG, RELEASE,
etc.) and a FeatureFlagExpression field. This field is an expression that must
evaluate to True or False. If the field cannot be evaluated (such as an
undeclared PCD used in the expression) the build parser must provide an
appropriate error message and stop the build. If the field evaluates to False,
the line is ignored. If the field evaluates to True, the build will use this
line.

The `[Binaries]` section of an INF file may list files with a FileType of
`DISPOSABLE`. The build tools must ignore files of this type.

#### 8.2.4.3 Build.exe --ignore-sources option

When the `--ignore-sources` option is present on the build.exe command-line,
all modules specified in the DSC and FDF files must be either Binary INFs or
Mixed INFs (that contain binary images). The build tools will ignore any
content in a Mixed INF [Sources] section. If a Source INF is listed in the DSC
file, the build must break during parsing with an appropriate error message. If
an INF file is listed in the DSC file that does not contain a `[Binaries]`
section, the build must break during parsing with an appropriate error message.
The only code that will be generated during this build is the binary external
PCD database file that will be added to the PEIM and DXE PCD driver FFS files.

#### 8.2.4.4 Macros

The build and `GenFds` tools use the `-D`, `--define` command line options with
an argument formatted: `MACRO_NAME "=" value`. If the `"=" value` is omitted,
the `MACRO_NAME` is assigned a value of `0`.

Token names (words defined in the EDK II meta-data file specifications) cannot
be used as macro names. As an example, using `PLATFORM_NAME` as a macro name is
not permitted, as it is a token defined in the DSC file's `[Defines]` section.

Macros defined in INF files are local to the INF file. EDK II INF files must
not use global macros except in build option flags. In INF files, macros can
only be used for filenames, paths and, in the `[BuildOptions]` section, on the
right (value) side of the statements.

Macros can be defined or used in the INF file's `[Defines]`,
`[LibraryClasses]`, `[Sources]`, `[Binaries]`, `[Packages]` and
`[BuildOptions]` sections.

Macros defined in DEC files are local to the DEC file. DEC files must not use
global macros. In DEC files, macros can only be used for filenames and paths.

Macros can be defined or used in the DEC file's `[Defines]`, `[Includes]` or
`[LibraryClasses]` sections.

System environment variables may be referenced, however their values must not
be altered.

###### Table 9 System Environment Variable Usage

| Macro Style Used in Meta-Data files | Windows Environment Variable | Linux & OS/X Environment Variable |
|:-----------------------------------:|:----------------------------:|:---------------------------------:|
| `$(WORKSPACE)`                      | `%WORKSPACE%`                | `$WORKSPACE`                      |
| `$(EFI_SOURCE)`                     | `%EFI_SOURCE%`               | `$EFI_SOURCE`                     |
| `$(EDK_SOURCE)`                     | `%EDK_SOURCE%`               | `$EDK_SOURCE`                     |
| `$(EDK_TOOLS_PATH)`                 | `%EDK_TOOLS_PATH%`           | `$EDK_TOOLS_PATH`                 |
| `$(ECP_SOURCE)`                     | `%ECP_SOURCE%`               | `$ECP_SOURCE`                     |

**********
**Note:** The `PACKAGES_PATH` and `EDK_TOOLS_BIN` system environment variables
shall not be referenced in EDK II meta-data files.
**********

There are also four global MACRO statements that may be used in different
portions of the DSC and FDF files, `$(TARGET)`, `$(TOOL_CHAIN_TAG)`,
`$(OUTPUT_DIRECTORY)` and `$(ARCH)`.

Macros defined in the FDF file are local to the FDF file. Macros are permitted
in the entire FDF file.

**********
**Note:** In the `[Rules]` section of the FDF, the macros listed in that
section must match macro names defined for the `build_rule.txt` file.
**********

Macros defined in the DSC file's `[Defines]` section can be used in either the
DSC file or in the FDF file. Macros defined in other sections of the DSC file
can only be used in the DSC file - they cannot be used in the FDF file. Macros
in the DSC file can be used for file names, paths, PCD values, in the
`[BuildOptions]` section, on the right (value) side of the statements and in
conditional directives. Macros can also be defined or used in the `[Defines]`,
`[LibraryClasses]`, `[Libraries]`, `[Components]` and all PCD sections.

Macros defined by the user may be used in the !include statements in DSC and
FDF files.

`EDK_GLOBAL` type macros defined in the DSC file can be used in later sections
of the DSC, FDF and any of the included EDK INF files.

Macro values must be defined prior to using them in directive statements or for
PCD values. The following provides the precedence (high to low) for obtaining
macro values.

* Command-line, `-D` flags (left most has higher priority)
* FDF file, `DEFINE` statements override previous definitions in the
  `[Defines]` section
* FDF file, `DEFINE` statements in the `[Defines]` section
* DSC file, Component INF `DEFINE` statements embedded in `<subsections>`
* DSC file, `DEFINE` statements in sections following the `[Defines]` section
* DSC file, `DEFINE` statements in the `[Defines]` section

**********
**Note:** Macros defined in the DSC file's `[Defines]` section are common to
both the DSC and FDF file. Macros defined in the FDF file are local to the FDF
file. Macros defined in other sections of the DSC file are local to the section
types that define them.
**********
**Note:** Macros defined in INF and DEC files are local to the file that
defined them.
**********
**Note:** Note that all command line options for the build tool are passed to
the GenFds tool after the make portion of the build completes.
**********

Macros defined in common sections may be used in the architecturally modified
sections of the same section type. Macros defined in architectural sections
cannot be used in other architectural sections, nor can they be used in the
common section. Section modifiers in addition to the architectural modifier
follow the same rules as architectural modifiers.

When used in a `!if` or `!elseif` conditional expression statement or in an
expression used in a value filed, a macro that has not been defined has a value
of 0.

The remaining MACRO definitions will be expanded by tools when they encounter
the entry in the section except when the macro is within double quotation marks
in build options sections. The expectation is that macros in the quoted values
will be expanded by external build scripting tools, such as nmake or make; they
will not be expanded by the build tools. If a macro that is not defined is used
in locations that are not expressions or value fields (where the tools would
just do macro expansion as in C flags in a `[BuildOptions]` section), nothing
will be emitted. If the macro, `MACRO1`, has not been defined, then:

`MSFT:*_*_*_CC_FLAGS = /c /nologo $(MACRO1) /Od`

After macro expansion, the logical result would be equal to:

`MSFT:*_*_*_CC_FLAGS = /c /nologo /Od`

It is recommended that tools remove any excess space characters when processing
these types of lines.

The following table lists reserved global macro names that are completed by the
internal build tools. These macros must not be redefined.

###### Table 10 Reserved Macros Expanded by Tools

| Macro String          | Description                                                                                                                                                                                                         |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$(ARCH)`             | Architecture of current module                                                                                                                                                                                      |
| `$(BASE_NAME)`        | The file name of the module binary.                                                                                                                                                                                 |
| `$(BUILD_DIR)`        | All files for building a platform will be put in this directory                                                                                                                                                     |
| `$(BUILD_NUMBER)`     | Used in FDF file `[Rules]` sections to identify a build number used in a UEFI Version section. This is a value that is defined in the DSC file.                                                                     |
| `$(ECP_SOURCE)`       | The system environment variable that points to a version of the Edk Compatibility Package. This is only required if there are EDK components and libraries included in an EDK II platform build.                    |
| `$(EDK_SOURCE)`       | The system environment variable that points to an EDK tree containing the Foundation elements of an EDK tree. This is only required if there are EDK components and libraries included in an EDK II platform build. |
| `$(EDK_TOOLS_PATH)`   | The system environment variable that points to the path of build tools                                                                                                                                              |
| `$(EFI_SOURCE)`       | The system environment variable that points to an EDK tree containing EDK components and libraries. This is only required if there are EDK components and libraries included in an EDK II platform build.           |
| `$(FILE_GUID)`        | An EDK component's GUID value                                                                                                                                                                                       |
| `$(INF_OUTPUT)`       | Used in FDF file `[Rules]` sections to identify the location of UEFI compliant binary leaf section content                                                                                                          |
| `$(INF_VERSION)`      | Used in FDF file `[Rules]` sections to identify the version string used in a UEFI Version section.                                                                                                                  |
| `$(MODULE_NAME)`      | Current module name                                                                                                                                                                                                 |
| `$(MODULE_TYPE)`      | Current module type                                                                                                                                                                                                 |
| `$(MODULE_GUID)`      | Current module GUID                                                                                                                                                                                                 |
| `$(NAMED_GUID)`       | Used in FDF file `[Rules]` sections this macro is used by the build tools to create an FFS file named by the Module's GUID value.                                                                                   |
| `$(OUTPUT_DIRECTORY)` | This directory is where the output binary files will be generated, either an absolute path or relative to the `WORKSPACE`.                                                                                          |
| `$(TARGET)`           | Target of current module (`DEBUG`/`RELEASE`/`NOOPT`)                                                                                                                                                                            |
| `$(TOOL_CHAIN_TAG)`   | Tool chain used to build current module                                                                                                                                                                             |
| `$(WORKSPACE)`        | The system environment variable that points to the current Workspace directory.                                                                                                                                     |

The following table lists special Macros that may only be used in an FDF file's
[Rules] section. Like the Macros in the previous table, they must never be
redefined.

1. The ${d_*} macros always mean OutputPath + ModuleGuild + .ffs

2. When starting to generate FFS, the ${s_*} macros mean source INF file full
   path, but in EfiSection.py, it is changed to the full path of efi file.

###### Table 11 Reserved FDF [Rule] Section Macro Strings

| Variable String   | Description                                                                                                                      |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| "${src}"          | Source file(s) to be built (full path)                                                                                           |
| "${s_path}"       | Source INF file directory (absolute path)                                                                                        |
| "${s_dir}"        | Source file relative directory within a module. NOTE: ${s_dir} is always equals to "." if source file is given in absolute path. |
| "${s_name}"       | Source file name without path.                                                                                                   |
| "${s_base}"       | Source file name without extension and path.                                                                                     |
| "${s_ext}"        | Source file extension.                                                                                                           |
| "${dst}"          | Destination file(s) built from ${src} (full path)                                                                                |
| "${d_path}"       | Destination file directory (OutputPath + ModuleGuid.ffs)                                                                         |
| "${d_name}"       | Destination file name without path.                                                                                              |
| "${d_base}"       | Destination file name without extension and path                                                                                 |
| "${d_ext}"        | Destination file extension                                                                                                       |

Macro evaluation is done at the time the macro is used in an expression,
conditional directive or value field, not when a macro is defined. Macros in
quoted strings will not be expanded by parsing tools; all other macro values
will be expanded, without evaluation, as other elements of the build system
will perform any needed tests.

#### Example

```ini
[LibraryClasses.common]
  DEFINE MDE = MdePkg/Library
  BaseLib|$(MDE)/BaseLib.inf

[LibraryClasses.X64, LibraryClasses.IA32]
  # Can use $(MDE), cannot use $(MDEMEM)
  DEFINE PERF = PerformancePkg/Library
  TimerLib|$(PERF)/DxeTscTimerLib/DxeTscTimerLib.inf

[LibraryClasses.X64.PEIM]
  # Can use $(MDE) and $(PERF)
  DEFINE MDEMEM = $(MDE)/PeiMemoryAllocationLib
  MemoryAllocationLib|$(MDEMEM)/PeiMemoryAllocationLib.inf

[LibraryClasses.IPF]
  # Cannot use $(PERF) or $(MDEMEM)
  # Can use $(MDE) from the common section
  PalLib|$(MDE)/UefiPalLib/UefiPalLib.inf
  TimerLib|$(MDE)/BaseTimerLibNullTemplate/BaseTimerLibNullTemplate.inf
```

#### EDK_GLOBAL

The `EDK_GLOBAL` statements defined in the DSC file can be used during the
processing of the DSC, FDF and EDK INF files. The definition of the
`EDK_GLOBAL` name must only be done in the DSC `[Defines]` section. These
special macros can be used in path statements, `[BuildOptions]` and `[Rule]`
sections. These statements are used to replace the environment variables that
were defined for the EDK build tools. They must never be used in a conditional
directive statement in the DSC and FDF files, nor can they be used by EDK II
INF files.

#### 8.2.4.5 Conditional Directive Blocks

Additional build scoping can be implemented using the DSC and FDF directive
statements in combination with command line options for the build tool.
Conditional directive blocks are not permitted in the EDK II DEC and INF files.

Conditional directive statements are used by the build tools preprocessor
function to include or exclude statements in the DSC and FDF files. A limited
number of statements are supported, and nesting of conditionals is also
supported. Statements are prefixed by the exclamation "!" character.
Conditional statements may appear anywhere within the DSC and FDF files. They
are not permitted in the DSC and INF files.

Refer to the Macro Statement section for information on using Macros in
conditional directives.

Conditional directive statements are only permitted in the DSC and FDF files.

Macro and PCD Names can be used in conditional directive statements.

Only macros can be used in the `!ifdef` and `!ifndef` statements, PCDs are code
elements which have been declared in the DEC files.

When testing if a Macro has been defined, the only the Macro name is required,
while in testing for values, the Macro name must be enclosed: `$(MacroName)`.
When the Macro is a string value, the `$(MacroName)` must not be encapsulated in
quotation marks, only string literals in directive statements need to be
enclosed by double quotation marks.

When testing values for PCDs, only the PCD name is required:
`TokenSpaceGuidCname.PcdCname`; enclosing the PCD name in "$(" and ")" is not
permitted.

Supported statements are: `!ifdef`, `!ifndef`, `!if`, `!else`, `!elseif` and
`!endif`. These control statements are used to either include or exclude lines
as the parsing tool processes these files. The `!ifdef` and `!ifndef`
statements test whether a Macro has been defined or not defined (PCDs are
always defined - the build will break if a PCD is used by a module specified in
the DSC file that cannot be located in any of the dependent DEC files, from the
`[Packages]` section of an INF specified in the DSC file). FeatureFlag and
FixedAtBuild access methods are the only PCDs that can be used in conditional
directives.

The build system will process the DSC and FDF files more than once. The first
pass is to pick up all macros and PCD values for macros and PCDs used in
conditional directives, then on the second pass, process the conditional
directive content. This second pass is required as there is no required order
for sections within these files, and some PCD values may be defined in sections
that follow the use of the PCD in a conditional directive. Macros and PCDs used
in conditional directives must not be encapsulated in a conditional comparison
(`!if`) directive block. It is permissible to use an undefined macro prior to
the definition of the macro, as in the following example.

```
!ifndef FOO
DEFINE FOO=TRUE
!endif
```

When using PCDs in conditional directive statements or expressions, only the
PCD name is required. Do not encapsulate the PCD name in the "$(" and ")"
required for macro values as shown in the example below.

```
!if ( gTokenSpaceGuid.PcdCname == 1 ) AND ( $(MY_MACRO) == TRUE )
DEFINE FOO=TRUE
!endif
```

In the above example, `FOO` must not be used in a conditional directive statement.

When testing strings, the strings must to be encapsulated by double quotation
marks, as shown in the following example.

```
!if $(SETUP) == "SETUP"
DEFINE FOO=TRUE
!endif
```

For backward compatibility, the EDK II build system will process strings that
are not encapsulated by the double quotation marks, however this will not be
supported in future releases.

Strings can only be compared to strings of a like type (testing an ASCII string
against a Unicode format string must fail), numbers can only be compared
against numbers and boolean objects can only evaluate to `TRUE` or `FALSE`. See
the Operator Precedence table, below for a list of restrictions on comparisons.

Refer to the DSC and FDF file form specifications "_Conditional Directive
Blocks_" section for additional details of how directives must be processed.

#### 8.2.4.6 Expressions

Expressions can be used in conditional directive comparison statements and in
value fields for PCDs in the DSC and FDF files.

**********
**Note:** Expressions are not supported in the INF and DEC files.
**********

Expressions follow C relation, equality, logical and bitwise precedence and
associativity. Not all C operators are supported, only operators in the
following list can be used.

**********
**Note:** Due to the flexibility of the build system, a new operator, `IN`
has been added that can be used to test whether an element is in a list. The
format for this is `<Value>` IN `<MACRO_LIST>`, where MACRO_LIST can only be
one of `$(ARCH)`, `$(TOOL_CHAIN_TAG)` and `$(TARGET)`.
**********

Use of parenthesis is encouraged to remove ambiguity.

Additional scripting style operators may be used in place of C operators as
shown in the table below.

###### Table 12 Operator Precedence and Supported Operands

| Operator                                     | Use with Data Types   | Notes                                                                                                                                                                                                                         | Priority |
| -------------------------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `or`, `OR`, <code>&#124;&#124;</code>        | Number, Boolean       |                                                                                                                                                                                                                               | Lowest   |
| `and`, `AND`, `&&`                           | Number, Boolean       |                                                                                                                                                                                                                               |          |
| <code>&#124;</code>                          | Number, Boolean       | Bitwise OR                                                                                                                                                                                                                    |          |
| `^`, `xor`, `XOR`                            | Number, Boolean       | Exclusive OR                                                                                                                                                                                                                  |          |
| `&`                                          | Number, Boolean       | Bitwise AND                                                                                                                                                                                                                   |          |
| `==`, `!=`, `EQ`, `NE`, `IN`                 | All                   | The IN operator can only be used to test a quoted unary literal string for membership in a list.                                                                                                                              |          |
|                                              |                       | Space characters must be used before and after the letter operators Strings compared to boolean or numeric values using "==" or "EQ" will always return FALSE, while using the "!=" or "NE" operators will always return TRUE |          |
| `<=`, `>=`, `<`, `>`, `LE`, `GE`, `LT`, `GT` | All                   | Space characters must be used before and after the letter operators.                                                                                                                                                          |          |
| `+`, `-`                                     | Number, Boolean       | Cannot be used with strings - the system does not automatically do concatenation. Tools should report a warning message if these operators are used with both a boolean and number value                                      |          |
| `!`, `not`, `NOT`                            | Number, Boolean       |                                                                                                                                                                                                                               | Highest  |

The `IN` operator can only be used to test a literal string against elements
in the following global variables:

**_$(FAMILY)_**

`$(FAMILY)` is considered a list of families that different `TOOL_CHAIN_TAG`
values belong to. The `TOOL_CHAIN_TAG` is defined in the `Conf/target.txt` or
on the command-line. The FAMILY is associated with the `TOOL_CHAIN_TAG` in the
`Conf/tools_def.txt` file (or the `TOOLS_DEF_CONF` file specified in the
`Conf/target.txt` file) file. While different family names can be defined,
`ARMGCC`, `GCC`, `INTEL`, `MSFT`, `RVCT`, `RVCTCYGWIN` and `XCODE` have been
predefined in the `tools_def.txt` file.

**_$(ARCH)_**

`$(ARCH)` is considered the list of architectures that are to be built, that
were specified on the command line or come from the `Conf/target.txt` file.

**_$(TOOL_CHAIN_TAG)_**

`$(TOOL_CHAIN_TAG)` is considered the list of tool chain tag names specified on
the command line

**_$(TARGET)_**

`$(TARGET)` is considered the list of target (such as `DEBUG`, `RELEASE` and
`NOOPT`) names specified on the command line or come from the `Conf/target.txt`
file.

For logical expressions, any non-zero value must be considered `TRUE`.

Invalid expressions must cause a build break with an appropriate error message.

#### 8.2.4.7 EDK Overrides

For EDK component INF files, an optional sub-element of `<SOURCE_OVERRIDE_PATH>`
has been defined. If this element is specified, files listed in the directory
are used instead of the "same-named" files in the component's directory. If an
EDK component directory lists files, `A.c`, `B.c` and `C.h`, and the directory
specified in this sub-element contains the file `B.c`, then the component will
be built using files from the component directory: `A.c` and `C.h`, and the file
`B.c` from the override directory. Any other files listed in the override
directory will NOT be included in the build (no new or additional files are
permitted).

#### 8.2.4.8 DEPEX processing

EDK II modules that have dependencies must use the `[Depex]` section to define
the dependency expressions, however both EDK and EDK II may specify a
dependency expression file. If the file specified, the complete dependency
expression must be defined in the file. For EDK II modules, the build tools
will create the complete dependency expression using the information in the
`[Depex]` section along with all `[Depex]` sections from the linked in library
instances. Depex expressions listed in an INF file's `[Depex]` section are
written as in-fix expressions, while the output of the `GenDepex` tool
generating the EFI Depex section is a post-fix expression. If an INF file
specifies a `DPX_SOURCE` entry in the INF file's `[Defines]` section, the file
must also use an in-fix expression. The table below lists the operator
precedence for dependency expressions.

###### Table 13: [Depex] Expression Operator Precedence

| Operator          | Use with Data Types                                   | Notes                                                                                                                                                                     | Priority   |
| ----------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| `( )`             | TRUE, FALSE, Expression, GUID, CName or Encapsulation | Encapsulated items are processed from inner-most to outer-most                                                                                                            | Highest    |
| `NOT`             | TRUE, FALSE, Expression, GUID, CName or Encapsulation | After identifying encapsulation parameters, the NOT operator must take precedence over any other items.                                                                   |            |
| `AND`, `and`      | TRUE, FALSE, GUID or Encapsulation                    | These operators are used to create an expression                                                                                                                          |            |
| `OR`, `or`        | TRUE, FALSE, GUID or Encapsulation                    | These operators are used to create an expression                                                                                                                          | Lowest     |
| `SOR`             | TRUE, FALSE, GUID or Encapsulation                    | Only valid for DXE and SMM dependency expressions and must be the first statement followed by either a GUID, encapsulation or an expression                               |            |
| `AFTER`, `BEFORE` | GUID                                                  | Only valid for DXE and SMM dependency expressions. These must be the only operator in the dependency expression. Only one of these is permitted per dependency expression |            |

#### 8.2.4.9 PCD Access Methods

A PCD is defined as `TokenSpaceGuidCName.PcdCName`. Each PcdCName must be
unique to the Token Space declaring the PCD. The token space is a name space
that is unique to the GUID known as the TokenSpaceGuidCName.

The following list defines the five PCD access methods.

* **FeatureFlag PCD** - used in conditional directive statements in code.

* **PatchableInModule PCD** - a volatile variable that can be updated either
  during a build or by a tool that knows the offset and data size of the
  variable.

* **FixedAtBuild PCD** - a static variable that is set during the build.

* **Dynamic** - a PCD that will use the standard PcdGet/PcdSet macros; the
  values for these PCDs are common to all modules in a platform and must be
  listed (with the storage method and value) in the DSC file.

* **DynamicEx** - a PCD that uses the PcdGetEx/PcdSetEx macros; the values for
  these PCDs are common to all modules in a platform and must be listed (with
  the storage method and value) in the DSC file.

How a PCD is coded also makes a difference as to how code is generated by the
build system. FeatureFlag PCDs can only be used as FeatureFlag PCDs; very
straight forward. Modules can code the remaining types of PCDs to be either
FixedAtBuild (a const which is accessible via a `PcdGet` function),
PatchableInModule (which can be modified using an external tool), Dynamic which
is accessible via a `PcdSet`, `PcdGet` or `DynamicEx` which uses the token
space GUID and token number of a PCD in the `PcdGetEx` and `PcdSetEx` access
methods. The build system will record all (FixedAtBuild, PatchableInModule,
Dynamic and DynamicEx) PCD data into one of the two PCD databases implemented
in EDK II. Dynamic PCD definitions are an amalgamation of FixedAtBuild,
PatchableInModule and DynamicEx.

It is recommended that developers code their modules to use the Dynamic form.
The Dynamic form allows the platform integrator to select how they want to use
the PCD; selecting how they want to expose the data; FixedAtBuild,
PatchableInModule, Dynamic or (PI compliant) DynamicEx. If the platform
integrator selects the Dynamic or DynamicEx form for any PCD, then the platform
must also contain a PEI and/or DXE PCD driver to maintain a volatile database
of values that can be set or retrieved.

Dynamic and DynamicEx PCD values are common to all modules in a platform and
the storage mechanism for these PCDs must be defined by the platform developer,
so the PCD values must be specified in the DSC file under a section that
specifies the storage mechanism (Default, VPD or HII).

The DynamicEx PCDs correspond to the _PI Specification_, while the other PCD
forms are associated with EDK II.

Modules that will be distributed in binary form must use either
PatchableInModule or DynamicEx PCDs.

PatchableInModule PCDs also require the build system to generate a map file for
each module that is using PatchableInModule PCDs. This map file contains the
offset from the start of the file to the location of the first byte of the PCD.

If the Platform Integrator does not specify the format, and all of the INF
files that use the PCD state that they have been coded to use the Dynamic PCD
form, the tool must examine the methods available for a PCD that have been
declared in the DEC file. The build system uses the following priority for the
default method.

* If the PCD is listed in the DEC's `PcdsFixedAtBuild`, then use FixedAtBuild,
  otherwise,

* If the PCD is listed in `PcdsPatchableInModule`, then use PatchableInModule.

* If the PCD is not listed in either of the previous two sections, and it is
  listed in a `PcdsDynamicEx` section, then use DynamicEx.

* If not listed in any of the previous sections, and the PCD is listed in the
  `PcdsDynamic` section, then use Dynamic.

Build tools are required to process PCD values for `VOID*` PCDs into byte
arrays, C format GUIDs or as C format strings (either ASCII or [L]"string")
prior to autogenerating the code.

PCD values stored in VPD regions are processed prior to completing the final
PCD parsing. Refer to Section 8.4 for additional rules for processing PCDs to
create a platform scoped PCD Database.

#### 8.2.4.10 Precedence of PCD Values

The values that are assigned to individual PCDs required by a build may come
from different locations and different meta-data files. The following provides
the precedence (high to low) to assign a value to a PCD.

* Command-line, `--pcd` flags (left most has higher priority)
* DSC file, Component INF `<Pcd*>` section statements
* FDF file, grammar describing automatic assignment of PCD values
* FDF file, SET statements within a section
* FDF file, SET statement in the [Defines] section
* DSC file, global [Pcd*] sections
* INF file, PCD sections, Default Values
* DEC file, PCD sections, Default Values

In addition to the above precedence rules, PCDs set in sections with
architectural modifiers take precedence over PCD sections that are common to
all architectures.

When listed in the same section. If listed multiple times, the last one will be
used.

A PCD value set on the command-line has the highest precedence. It overrides
all instances of the PCD value specified in the DSC or FDF file. The following
is the syntax to override the value of a PCD on the command line:

`--pcd [<TokenSpaceGuidCname>.]<PcdCName>=<Value>`

For `VOID*` type PCDs, `<Value>` supports the following syntax:

* ASCII string value for a `VOID*` PCD

  `--pcd  [<TokenSpaceGuidCname>.]<PcdCName>="String"`

*  Unicode string value for a `VOID*` PCD

  `--pcd  [<TokenSpaceGuidCname>.]<PcdCName>=L"String"`

*  Byte array value for a `VOID*` PCD

  `--pcd  [<TokenSpaceGuidCname>.]<PcdCName>= H"{0x1, 0x2}"`

**********
**Note:** The EDK II meta-data specs have changed to permit a PCD entry (or any
other entry) to be listed only one time per section.
**********
**Caution:** Dynamic and DynamicEx `VOID*` VPD PCD array values must be hex byte
arrays. Using a Registry or C format GUID value in the value field of a `VOID*`
VPD PCD is not permitted.
**********

If the maximum size of a `VOID*` PCD is not specified in the DSC file, then the
maximum size is calculated based on the largest size of 1) the string or array
in the DSC file, 2) the string or array in the INF file and 3) the string or
array in the DEC file. If the value is a quoted text string, the size of the
string will be incremented by one to handle string termination. If the quoted
string is preceded by L, as in `L"This is a string"`, then the size of the string
will be incremented by two to handle unicode string termination. If the value
is a byte array, then the size of the byte array is not modified.

For example, if the string in the DSC file is `L"DSC Length"`, the INF file has
`L"Module Length"` and the DEC file declares the default as `L"Length"`, then
the maximum size that will be allocated for this PCD will be 28 bytes (`
L"Module Length"` 26 bytes, 2 bytes for null termination character).

`VOID*` PCDs must be byte aligned if the value is an ASCII string, two-byte
aligned if the value is a Unicode string or 8-byte aligned in the value is a
byte array.

#### 8.2.4.11 Section Handling

The INF and DEC file parsing routines must process the sections so that common
architecture sections are logically merged with the architecturally specific
sections. The architectural sections need to be processed so that they are
logically after the common section. It is recommended that EDK II developers
use a logical ordering of the sections.

Other section modifiers must also be logically appended to the merged sections
(for INFs that have architectural and common architecture sections) after the
merge.

For `[BuildOptions]` sections in the INF and DSC file, the entries with a
common left side (of the "=") will be either appended or replace previous
entries based on the "==" replace or "=" append assignment character
sequence.

```
Common Section + Architectural Section + Common Section w/extra Modifier + Architectural Section w/extra Modifier
```

#### Example

```ini
[BuildOptions.Common]
  MSFT:*_*_*_CC_FLAGS = /nologo

[BuildOptions.Common.EDK]
  MSFT:*_*_*_CC_FLAGS = /Od

[BuildOptions.IA32]
  MSFT:*_*_IA32_CC_FLAGS = /D EFI32
```

For IA32 architecture builds of an EDK II INF file would logically be:

`MSFT:*_*_IA32_CC_FLAGS = /nologo /D EFI32`

For non-IA32 architecture EDK INF files, tool parsing would logically be:

`MSFT:*_*_*_CC_FLAGS = /nologo /Od`

For IA32 architecture builds of an EDK INF file, tool parsing would logically
be:

`MSFT:*_*_IA32_CC_FLAGS = /nologo /D EFI32 /Od`

#### 8.2.4.12 PCD INFO Generation

The UEFI Platform Initialization specification defines a PEIM and Protocol that
can retrieve the PCD Token number and the PCD Token Name (the PCD C Name)
information from the PCD Database. In order to support these modules, a
`PCD_INFO_GENERATION` entry in the DSC file's `[Defines]` section is used to
enable generate the PCD Database with the required information (normally, only
the PCD Token number is available). This feature does increase the size of the
PCD drivers that contain the PCD database, so this capability is added as an
optional feature rather than always generating the content.

If the `[Defines]` section has the `PCD_VAR_CHECK_GENERATION` entry set to
TRUE, then a binary file will be created in the FV directory for Dynamic and
DynamicEx PCD HII Variable checking.

#### 8.2.4.13 Pre Build Processing

The DSC file is parsed after the tool meta-data files. If the `[Defines]`
section of the DSC file contains a `PREBUILD = entry` statement, processing
of the DSC file is suspended and the script specified in the `PREBUILD`
statement is executed. If the script file is not found, the **build** command
exits with an appropriate error message. If the script fails, it must terminate
with a non-zero exit code and the **build** command terminates with the exit
value from the pre-build script. The script is required to generate error
messages that provide the reason for the termination.

All of the command line options passed into the **build** command are also
passed  into the script along with the command line options for `TARGET`,
`ARCH`, and `TOOL_CHAIN_TAG`. The values for `TARGET`, `ARCH`, and
`TOOL_CHAIN_TAG` are from the command line options passed into the **build**
command. If these values are not passed into the **build** command, then they
are retrieved from `target.txt`.

If the script terminates successfully (exit value of 0), parsing of the DSC
file continues, and build tools may retrieve environment variables that have
been updated by the script.

**********
**Note:** This entry may be wrapped in a conditional directive that uses the
value of the `TOOL_CHAIN_TAG` determined earlier. Using a MACRO value other
than `$(TOOL_CHAIN_TAG)` is prohibited, as the DSC file has not been processed
at the time the ENTRY was found.
**********
**Note:** Quotes are needed when the script's additional options are present.
Quotes are also required if the path to the pre-build command contains space
or special characters.
**********

#### 8.2.4.14 NMAKE Command line limitation handling

`NMAKE` is limited to command-line length of 4096 characters.  Due to the large
number of `/I` directives specified on command line (one per include directory),
the path length of `WORKSPACE` is multiplied by the number of `/I` directives
and can exceed this command-line length limitation. When this issue occurs, the
build tools pass the command line options via a response file instead of
directly on the command line.  The contents of the response file is combination
of `FLAGS` options and `INC` options. If a build fails, the build tools print
the response file's file location and the contents of the response file.

The **build** command supports the options `-l` and `--cmd-len` to set the
maximum command line length.  The default value is 4096.

**********
**Note:** The following `FLAGS` options are included in the response file:
`PP_FLAGS`, `CC_FLAGS`, `VFRPP_FLAGS`, `APP_FLAGS`, `ASLPP_FLAGS`, `ASLCC_FLAGS`,
and `ASM_FLAGS`.
**********

### 8.2.5 Post processing

Once all files are parsed, the build tools will do following work for each EDK
II module:

* Resolve the library classes to library instances, inherit and resolve library
  classes from them recursively, until no new library instances are found.

* Re-order the library instances according to the consuming relationship and
  their constructors. For each EDK II module, the tools must select one library
  instance per required library class (with the exception of the NULL library
  class keyword) using the following precedence (high to low):

  - The DSC file's component INF scoping `<LibraryClasses>` section
  - The DSC file's `[LibraryClasses.arch.module_type]` section tags with both
    architecture and module type modifiers
  - The DSC file's common arch with a module type modifier,
    ```
    [LibraryClasses.common.module_type]
    ```
  - DSC file's architecture specific modifier only `[LibraryClasses.arch]`
  - The DSC file's common `[LibraryClasses]` section

  **********
  **Note:** For modules of type **USER_DEFINED**_, if a `NULL` library class
  is required, the library instance should be listed in the INF scoping
  `<LibraryClasses>` section of the component.
  **********

* Inherit GUIDs, Protocols and PPIs from all library instances obtained above,
  and determine values or type of them. The value of a GUID, Protocol or PPI is
  defined in DEC file.

  **********
  **Note:** If GUID, Protocol or PPI is listed in a DEC file, where the
  `Private` modifier is used in the section tag (`[Guids.common.Private]` for
  example), only modules within the package are permitted to use the GUID,
  Protocol or PPI. If a module or library instance outside of the package
  attempts to use the item, the build must fail with an appropriate error
  message.
  **********

* Inherit PCDs from all library instances obtained above and determine values
  and type. The value and type of a PCD are obtained from a DSC file, INF file
  or DEC file if it cannot be found in the DSC or INF file. For each EDK II
  module, the tools must obtain unique PCD values using the following
  precedence (high to low):

  - Command-line, `--pcd` flags (left most has higher priority)
  - The DSC file's component INF scoping `<Pcds*>` sections
  - FDF file, grammar describing automatic assignment of PCD values
  - FDF file, SET statements within a section
  - FDF file, SET statement in the [Defines] section
  - The DSC file's `[Pcd*.arch.skuid]` sections
  - The DSC file's `[Pcd*.common.skuid]` sections
  - The DSC file's `[Pcd*.arch]` sections
  - The DSC file's `[Pcd*.common]` sections
  - The INF file's PCD sections
  - The DEC file's PCD sections

  **********
  **Note:** Values of PCDs using the FeatureFlag, PatchableInModule and
  FixedAtBuild access methods set for this INF file are local to the INF file and
  do not pertain to any other INF files. Dynamic and DynamicEx access method PCD
  values are global to a platform and should not be overridden by specifying them
  here. If, however, the dynamic PCDs are only valid for this INF, it is
  permissible to set them here.
  **********

* Inherit library instance dependency (`[Depex]` sections) expressions if a
  module does not list a separate dependency file.

* If the DSC file contains PCD sections for `DynamicVpd` or `DynamicExVpd`
  access methods, special processing is required. Refer to the appendix "VPD
  PCD Intermediate Files" for additional details.

* Determine if a module has specified Unicode file names, designated by the
  `.uni` file extension, in the INF file.

* Determine if a module has specified Image definition file names, designated
  by the `.idf` file extension, in the INF file.

* Any Visual Forms Representation (.vfr) files found during the pre-processing
  steps will be processed during the $(MAKE) stage. Refer to the "VFR
  Programming Language" document for additional details.

* Generate the Build Output Directory structure

* Generate the code files

* Generate the `Makefiles`

* Generate the "AsBuilt" INF files
