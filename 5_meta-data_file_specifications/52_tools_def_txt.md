<!--- @file
  5.2 tools_def.txt

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

## 5.2 tools_def.txt

This file describes the tools used by a developer, providing the flexibility to
have multiple tool chains and different profiles for each tool chain. In the
simplest of terms, the file provides a variable mapping of compiler tool chains
and flags. The structure of this text file is described below.

There are three types of statements, the `IDENTIFIER` statement which defines a
"User Interface" name for identifying this file. The second statement type is
the `DEFINE` statement which is used to identify a fully qualified path macro,
while the third type of statement is a record statement containing mappings
that are processed by the build tools to generate `Makefile` and `GNUMakefile`
commands that are executed by a compiler's "make" utility or function.

The left side of the record is subdivided into five groups, defined below. The
build tools will process the file and assign the following priority during the
parsing. After parsing the right hand `<string>` is substituted into the
makefile using the `build_rule.txt` templates.

If a wildcard value is permitted, the wildcard character is the star "*"
character.

For tool chains that expect to use a Windows-style nmake utility one entry, the
**NMAKE** `COMMANDTYPE` is required. The *NIX-based make and MAKE utilities are
typically in a developer's path environment (`/usr/bin`). Specifying a **MAKE**
command that will use an alternate make utility for *NIX-based tool chains is
optional.

```
  format: TARGET_TOOLCHAIN_ARCH_COMMANDTYPE_ATTRIBUTE = <string>
  priority:
          TARGET_TOOLCHAIN_ARCH_COMMANDTYPE_ATTRIBUTE (Highest)
          ******_TOOLCHAIN_ARCH_COMMANDTYPE_ATTRIBUTE
          TARGET_*********_ARCH_COMMANDTYPE_ATTRIBUTE
          ******_*********_ARCH_COMMANDTYPE_ATTRIBUTE
          TARGET_TOOLCHAIN_****_COMMANDTYPE_ATTRIBUTE
          ******_TOOLCHAIN_****_COMMANDTYPE_ATTRIBUTE
          TARGET_*********_****_COMMANDTYPE_ATTRIBUTE
          ******_*********_****_COMMANDTYPE_ATTRIBUTE
          TARGET_TOOLCHAIN_ARCH_***********_ATTRIBUTE
          ******_TOOLCHAIN_ARCH_***********_ATTRIBUTE
          TARGET_*********_ARCH_***********_ATTRIBUTE
          ******_*********_ARCH_***********_ATTRIBUTE
          TARGET_TOOLCHAIN_****_***********_ATTRIBUTE
          ******_TOOLCHAIN_****_***********_ATTRIBUTE
          TARGET_*********_****_***********_ATTRIBUTE
          ******_*********_****_***********_ATTRIBUTE (Lowest)
```

All entries in this file are case-sensitive.

### 5.2.1 Macros and Other Variable Statements (tools_def.txt only)

The use of MACRO statements is limited in EDK II `tools_def.txt` meta-data file
to be local to the meta-data file. The format and usage for the macro
statements is:

```
DEFINE MACRO = Value
DEF(MACRO)/filename.foo
```

Any defined MACRO will be expanded by tools when they encounter the entry in
the section.

The macro statements are positional, in that only statements following a macro
definition are permitted - a macro cannot be used before it has been defined.

MACRO statements are permitted in DSC and FDF files to reference PATH
statements, assign values to PCDs and to provide a minimum level of directive
statements - refer to the corresponding specification for additional details.

System environment variables may be used in value portion of statements. The
system environment value is specified using the following format:

`ENV(OsEnvironmentVariableName)`

The following variables, `$(MODULE_NAME)`, `$(IMAGE_ENTRY_POINT)`,
`$(MODULE_ENTRY_POINT)`, `$(ARCH_ENTRY_POINT)`, `$(DEBUG_DIR)`, `$(BASE_NAME)`,
`$(DEST_DIR_DEBUG)`, `$(EDK_TOOLS_PATH)`, `$(ARCHASM_FLAGS)`,
`$(PLATFORM_FLAGS)`, `$(ARCHCC_FLAGS)`, `$(ARCHDLINK_FLAGS)`,
`$(DLINKPATH_FLAG)`, `$(ASMPATH_FLAG)`, `$(CCPATH_FLAG)` and `$(SLINKPATH_FLAG)`
are never expanded when data is emitted to Makefiles.

These variables are used in values for statements having the `FLAG` attribute or
in macros that are used in the value fields of entries with the `FLAG` attribute.

### 5.2.2 Guided Tools

There are four GUIDed tools that are provided by the EDK II build system.

* `CRC32` - `FC1BCDB0-7D31-49AA-936A-A4600D9DD083`
  + This tool provides CRC32 (Cyclic Redundancy Check) methods for error
    detection using the `GenCrc32` tool.

* `TIANO` - `A31280AD-481E-41B6-95E8-127F4C984779`
  + This tool provides Tiano Compression using the `TianoCompress` application.

* `LZMA` - `EE4E5898-3914-4259-9D6E-DC7BD79403CF`
  + This tool provides LZMA Compression using the `LzmaCompress` application.

* `VPDTOOL` - `8C3D856A-9BE6-468E-850A-24F7A8D38E08`
  + This tool provides VPD binary data and map file generation using the `BPDG`
    application.

* `LZMAF86` - `D42AE6BD-1352-4bfb-909A-CA72A6EAE889`
  + `LzmaF86Compress` tool definitions with converter for x86 code. It can
    improve the compression ratio if the input file is IA32 or X64 PE image.
    Note: If X64 PE image is built based on GCC44, it may not get the better
    compression.

* `RSA2048SHA256SIGN` - `A7717414-C616-4977-9420-844712A735BF`
  + This tool definition uses a test signing key for development purposes only.
    The tool `Rsa2048Sha256GenerateKeys` can be used to generate a new
    private/public key and the
    `gEfiSecurityPkgTokenSpaceGuid.PcdRsa2048Sha256PublicKeyBuffer` PCD value.
    A custom tool/script can be implemented using the new private/public key
    with the `Rsa2048Sha256Sign` tool and this tool definition can be updated to
    use a custom tool/script.

Additional GUIDed tools may be added. If the GUID value is used in the FDF
file's GUIDed Encapsulation, the tool, named by the GUID, will be called using
a **-e** option to encode the content.

### 5.2.3 tools_def.txt EBNF Definition

#### Summary

EDK II tools will not expand `<MacroVal>` statements that appear within
quotation marks; the expectation is that external tools or the operating system
will expand them during execution.

When specifying Macros for paths for Windows tools, paths that contain space
characters do not need to be quoted. When specifying a path in a `FLAGS`
section, any path that contains a space character will need to be enclosed with
double quotation marks.

After the `IDENTIFIER = UiString` entry and Macro definition statements, all
other entries consist of `Token = Value` pairings. The Token is actually a
token that is constructed of five fields which are separated by an underscore
character.

Comments are only allows on separate lines and may not be appended appear on
actual entry lines.

The following EBNF defines the valid entries in the `tools_def.txt` file.

#### Prototype

```c
<ToolsDef>           ::= "IDENTIFIER" <Eq> <UiString> <EOL>
                         <DefineStatements>*
                         <ToolChainEntries>* <GuidedEntries>*
<TS>                 ::= <TabSpace>*
<MTS>                ::= <TabSpace>+
<Tab>                ::= 0x09
<Space>              ::= 0x20
<TabSpace>           ::= {<Tab>} {<Space>}
<UiString>           ::= (a-zA-Z0-9)<Chars>* <EOL>
<Chars>              ::= (0x20-0x7E)
<PathChars>          ::= {0x20} {0x28} {0x29} {(0-9a-zA-Z_-)} {0x2E}
<Eq>                 ::= <TS> "=" <TS>
<AsciiChars>         ::= (0x21 - 0x7E)
<AsciiString>        ::= [ <TS>* <AsciiChars>* ]*
<FlagString>         ::= <AsciiString>
<DefineStatements>   ::= <TS> "DEFINE" <MTS> <MACRO> <Eq> <Value> <EOL>
<MACRO>              ::= (A-Z)(a-zA-Z0-9_)*
<Value>              ::= {<Path>} {<FlagString>} {<Numbers>}
<Path>               ::= {<DosPath>} {<NixPath>} {<EnvPath>} {<MacroPath>}
<DosPath>            ::= {<AbsPath>} {<RelPath>}
<AbsPath>            ::= <A-Za-z> ":" ["\" <PathChars>+]+
<RelPath>            ::= ["\" <PathChars>+]*
<NixPath>            ::= ["/" <PathChars>+]*
<Numbers>            ::= (0-9)+ ["." (0-9)*]*
<MacroVal>           ::= "DEF(" <MACRO> ")"
<MacroPath>          ::= <MacroVal> {<NixPath>} {<RelPath>}
<EnvPath>            ::= "ENV(" <SysEnvVar> ")"
                         [[{"\"} {"/"}] <PathChars>*]+
<SysEnvVar>          ::= (A-Z)(A-Z0-9_)* # System Environment Variable
<ToolChainEntries>   ::= <RequiredEntry>
                         [<TS> <OptionalEntry>]*
<Wildcard>           ::= "*"
<RequiredEntry>      ::= <TS> <MakeEntry>
                         <TS> <FamilyEntry>
<MakeEntry>          ::= <Field1> "_" <Tagname> "_" <Arch> "_" <MakePath> <EOL>
<FamilyEntry>        ::= <Wildcard> "_" <Tagname> "_" <Arch> <FamilyType>
<FamilyType>         ::= "_" <Wildcard> "_" <Family>
<Family>             ::= "FAMILY" <Eq> <SupFamily> <EOL>
<SupFamily>          ::= {"ARMGCC"} {"MSFT"} {"INTEL"} {"GCC"} {"RVCT"}
                         {"RVCTCYGWIN"} {"XCODE"} {<NewFamily>}
<NewFamily>          ::= (A-Z) (A-Z0-9)+
<MakePath>           ::= "MAKE_PATH" <Eq> <EXECPATH> <Command> <EOL>
<OptionalEntry>      ::= <Field1> "_" <Field2> "_" <Field3>
<Field1>             ::= {<Target>} {<Wildcard>}
<Target>             ::= {<PreDefinedTargets>} {(A-Z) (A-Za-z0-9)*
<PreDefinedTargets>  ::= {"DEBUG"} {"RELEASE"} {"NOOPT"}
<Field2>             ::= {<TagName>} {<Wildcard>}
<Tagname>            ::= {<PreDefinedTags>} {(A-Z) (A-Za-z0-9)*}
<PreDefinedTags>     ::= {"ARMGCC"} {"ARMLINUXGCC"} {"CYGGCC"}
                         {"CYGGCCxASL"} {"DDK3790"} {"DDK3790xASL"}
                         {"ELFGCC"} {"GCC44"} {"GCC45"} {"GCC46"}
                         {"GCC47"} {"GCC48"} {"GCC49"} {"ICC"}
                         {"ICC11"} {"ICC11x86"} {"ICC11x86xASL"}
                         {"ICC11xASL"} {"ICCx86"} {"ICCx86ASL"}
                         {"ICCx86xASL"} {"ICCxASL"} {"MYTOOLS"}
                         {"RVCT"} {"RVCTCYGWIN"} {"RVCTLINUX"}
                         {"UNIXGCC"} {"VS2003"} {"VS2003xASL"}
                         {"VS2005"} {"VS2005x86"} {"VS2005x86xASL"}
                         {"VS2005xASL"} {"VS2008"} {"VS2008x86"}
                         {"VS2008x86xASL"} {"VS2008xASL"} {"VS2010"}
                         {"VS2010x86"} {"VS2010x86xASL"}
                         {"VS2010xASL"} {"VS2012"} {"VS2012x86"}
                         {"VS2012x86xASL"} {"VS2012xASL"} {"VS2013"}
                         {"VS2013x86"} {"VS2013x86xASL"}
                         {"VS2013xASL"} {"XCLANG"} {"XCODE32"} {"XCODE5"
<Field3>             ::= <Arch> "_" <Field4> "_" <Attributes>
<Arch>               ::= {"IA32"} {"X64"} {"IPF"} {"EBC"} {"ARM"} {<Wildcard>}
                         {(A-Z) (A-Z0-9)*}
<Field4>             ::= {<CommandCode>} {"*"}
<CommandCode>        ::= "}{"APP"} {"ASL"} {"ASLCC"} {"ASLDLINK"}
                         {"ASLPP"} {"ASM"} {"ASM16"} {"ASMLINK"}
                         {"ASMPATH"} {"CC"} {"CCPATH"} {"CRC32"}
                         {"DLINK"} {"DLINKPATH"} {"DSYMUTIL"} {"FLAGS"}
                         {"FROMELF"} {"FROMELFPATH"} {"GENFW"} {"LZMA"}
                         {"LZMAF86"} {"MAKE"} {"MTOC"} {"NASM"}
                         {"OBJCOPY"} {"OPTROM"} {"PP"} {"PPPATH"} {"RC"}
                         {"RSA2048SHA256SIGN"} {"SLINK"} {"SLINKPATH"}
                         {"SYMRENAME"} {"TIANO"} {"VFR"} {"VFRPP"}
                         {"VFRPPPATH"} {"VPDTOOL"}
<Attributes>         ::= {<ExecAttrs>} {<FlagAttr>} {<MiscAttrs>}
<ExecAttrs>          ::= "PATH" "=" <EXECPATH> <Command> <EOL>
<MiscAttrs>          ::= [<DllPath>} {<UserDefined>} {<RuleOrder>}
<DllPath>            ::= {"DLL"} {"DPATH"} <Eq> <EXECPATH> <EOL>
<EXECPATH>           ::= {<Definition>} {<Environ>} {<AbsolutePath>}
<Command>            ::= <Word> ["." <Ext>]
<Definition>         ::= "DEF(" <MACRO> ")" <Sep> [<Path>]*
<RuleOrder>          ::= "BUILDRULEORDER" <Eq> <ExtensionList> <EOL>
<ExtensionList>      ::= <Word> [<SP> <Word>]*
<GuidedEntries>      ::= <TS> <GuidDef>
                         <GuidPath>
                         [<GuidFlags>]
                         <GuidAttrs>*
<GuidedEntry>        ::= <Field1> "_" <Field2> "_" <Arch> "_" <Code>
<GuidDef>            ::= <GuidedEntry> "_" <Guid>
<Code>               ::= {"VPDTOOL"} {"LZMA"} {"TIANO"} {"CRC32"}
                         {"LZMAF86"} {"RSA2048SHA256SIGN"} {<NewTool>}
<NewTool>            ::= (A-Z)*
<Guid>               ::= "GUID" <Eq> <RegistryFormatGUID> <EOL>
<RegistryFormatGUID> ::= <RHex8> "-" <RHex4> "-" <RHex4> "-" <RHex4> "-"
                         <RHex12>
<RHex4>              ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<RHex8>              ::= <RHex4> <RHex4>
<RHex12>             ::= <RHex4> <RHex4> <RHex4>
<RawH2>              ::= <HexDigit>? <HexDigit>
<GuidPath>           ::= <GuidedEntry> "_PATH" <Eq> [<EXECPATH>]
                         <Command> <EOL>
<GuidFlags>          ::= <GuidedEntry> "_" <FlagAttr>
<GuidAttrs>          ::= <GuidedEntry> "_" <UserDefined>
<UserDefined>        ::= <Word> "=" <UserDefinedValues> <EOL>
<FlagAttr>           ::= "FLAG" "=" <FlagValues> <EOL>
<EOL>                ::= <TS> 0x0D 0x0A
```

#### Parameters

No space characters are permitted on the left side of the expression (before
the equal sign). All of the keywords that make up the left side of the
expression must be alphanumeric only - no special characters are permitted.

**_FlagValues_**

This is a string of zero or more tool specific flags. All flags must be
printable characters. The flag string starts with the character following the
first "=" sign in the line and terminates with the end of line.

**_Paths_**

The paths specified in the `tools_def.txt` file must be valid path names for
the workstation OS that will be using the tool chain identified by the tag
name. Since this file can contain numerous tool chains for multiple operating
systems, only the tool chain name specified in `target.txt` or on the
command-line needs to be valid paths.

**_Target_**

A keyword that uniquely identifies the build target; the first field, where
fields are separated by the underscore character. Three values, `NOOPT`,
`DEBUG` and `RELEASE` have been pre-defined. This keyword is used to bind
command flags to individual commands.

Users may want to add other definitions, such as, PERF, SIZE or SPEED, and
define their own set of `FLAGS` to use with these tags. The wildcard character,
"*", is permitted after it has been defined one time for a tool chain.

**_TagName_**

A keyword that uniquely identifies a tool chain group; the second field.
Wildcard characters are permitted only if a command is common to all tools that
will be used by a developer. As an example, if the development team only uses
IA32 Windows workstations, the ACPI compiler can be specified as
`DEBUG_*_*_ASL_PATH and RELEASE_*_*_ASL_PATH`.

**_Arch_**

A keyword that uniquely identifies the tool chain target architecture; the
third field. This flag is used to support the cross-compiler features, such as
when a development platform is IA32 and the target platform is X64 Using this
field, a single tag name can be setup to support building multiple target
platform architectures with different tool chains.

For example, if a developer is using Visual Studio .NET 2003 for generating
IA32 platform and uses the WINDDK version 3790.1830 for X64 or IPF platform
images, a single tag. See the `MYTOOLS` `PATH` settings in the generated
`Conf/tools_def.txt` or provided `BaseTools/Conf/tools_def.template` file.

The wildcard character, "*", is permitted only if the same tool is used for
all target architectures.

**_CommandCode_**

A keyword that uniquely identifies a specific command; the fourth field.
Several CommandCode keywords have been predefined, however users may add
additional keywords, with appropriate modifications to `build_rule.txt`. See
Table 7 below for the pre-defined keywords and functional mappings. The
wildcard character, "*", is permitted only for the `FAMILY`, `DLL` and
`DPATH` attributes (see **_Attributes_** below).

###### Table 7 Predefined Command Codes

| CommandCode         | Function                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `APP`               | C compiler for applications.                                                                                                   |
| `ARCHASM`[^1]       | Flags for a macro assembler that is specific to an architecture                                                                |
| `ARCHCC`[^1]        | Flags for a C compiler that is specific to an architecture                                                                     |
| `ARCHDLINK`[^1]     | Flags for a dynamic linker that is specific to an architecture                                                                 |
| `ASL`               | ACPI Compiler for generating ACPI tables.                                                                                      |
| `ASLCC`             | A C compiler for ACPI code prior to running the ASL compiler                                                                   |
| `ASLDLINK`          | A dynamic linker for the ACPI code                                                                                             |
| `ASLPP`             | A C Pre-processor for the ACPI code                                                                                            |
| `ASM`               | A Macro Assembler for assembly code in some libraries.                                                                         |
| `ASM16`             | A 16-bit assembler for SEC assembly code in some libraries                                                                     |
| `ASMLINK`           | The Linker to use for assembly code generated by the ASM tool.                                                                 |
| `ASMPATH`[^1]       | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `CC`                | C compiler for PE32/PE32+/Coff images.                                                                                         |
| `CCPATH`[^1]        | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `CRC32`             | This tool provides CRC32 (Cyclic Redundancy Check) methods for error detection using the GenCrc32 tool.                        |
| `DLINK`             | The C dynamic linker.                                                                                                          |
| `DLINKPATH`[^2]     | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `DSYMUTIL`          | This command code is specific to the XCODE32 and XCLANG tool chain tags.                                                       |
| `FROMELF`[^1]       | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `FROMELFPATH`[^1]   | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `GENFW`             | This command is for the EDK II build system GenFw utility, and allows user customization of the tool's flags.                  |
| `LZMA`              | This tool provides LZMA Compression using the LzmaCompress application.                                                        |
| `LZMAF86`           | LzmaF86Compress tool definitions with converter for x86 code.                                                                  |
| `MAKE`              | Required for tool chains. This identifies the utility used to process the Makefiles generated by the first phase of the build. |
| `MTOC`              | This command code is specific to the XCODE32 and XCLANG tool chain tags.                                                       |
| `OBJCOPY`           | This system command is specific to GCC tool chains, it is used to covert ELF images to PE32+ images.                           |
| `OPTROM`            | This command is for the EDK II build system EfiRom utility, and allows user customization of the tool's flags.                 |
| `PLATFORM`[^1]      | This command is for ARM based tool chains                                                                                      |
| `PP`                | The C pre-processor command.                                                                                                   |
| `PPPATH`[^1]        | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `RC`                | This is the command code for resource compilers.                                                                               |
| `RSA2048SHA256SIGN` | This tool definition uses a test signing key for development purposes only.                                                    |
| `SLINK`             | The C static linker.                                                                                                           |
| `SLINKPATH`[^1]     | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `SYMRENAME`         | This command code is by some of the GCC family tool chains.                                                                    |
| `TIANO`             | This tool provides Tiano Compression using the TianoCompress application.                                                      |
| `VFR`               | This command is for the EDK II build system Visual Forms Representation tool, VfrCompile                                       |
| `VFRPP`             | The C pre-processor used to process VFR files.                                                                                 |
| `VFRPPPATH`[^1]     | This command code is specific to the RVCT31CYGWIN tool chain tag.                                                              |
| `VPDTOOL`           | This tool provides VPD binary data and map file generation using the BPDG.                                                     |

[^1] These command codes are only used for `FLAG` attribute statements and are
not related to actual executable applications.

[^2] This is the path to standard Microsoft libraries (`.dll`).

**_Attribute_**

A keyword to uniquely identify a property of the command; the fifth and last
field.Several pre-defined attributes have been defined: `DLL`, `FAMILY`,
`FLAGS`, `GUID`, `OUTPUT` and `PATH`. Use quotation marks only if the quotation
marks must be included in the flag string. The following example shows the
format for the required quoted string,
`"C:\Program Files\Intel\EBC\Lib\EbcLib.lib"`. Normally, the quotation
characters are not required as everything following the equal sign to the end of
the line is used for the flag.

`*_*_EBC_DLINK_FLAGS = "C:\Program Files\Intel\EBC\Lib\EbcLib.lib" /NOLOGO`

###### Table 8 Predefined Attributes

| Attribute         | Description |
| ----------------- | ----------- |
| `ADDDEBUGFLAG`    | This flag is used by objcopy to set the option: `--add-gnu-debuglink` |
| `BUILDRULEFAMILY` | This flag is used by some tool chain tags to set a special `FAMILY` value when processing the build_rule.txt file. Normally, the `FAMILY` attribute is used to identify the type of makefile the tools need to generate. Tools such as `XCODE` will use `GCC` as the `FAMILY`, but uses different (from `GCC`) processing rules. If present and if a build rule (in `build_rules.txt`) contains an attribute with the value specified in this entry, that rule will be processed and the rule with the `FAMILY` attribute will be ignored. |
| `DLL`             | The path to the 3rd party tool's required DLLs - required for some tools to generate debug files. |
| `FAMILY`          | A flag to the build command that will be used to ensure the correct commands and flags are used in the generated Makefile or GNUMakefile, as well as to use the correct options for independent tools, such as the ACPI compiler. This is typically used to identify the type of Makefile that needs to be generated. |
| `FLAG` or `FLAGS` | The arguments for individual CommandCode tools. |
| `GUID`            | This defines the Registry Format GUID (8-4-4-4-12). The tool is identified by the GUID value specified which is also specified in the DSC file. These GUID tools call other tools that modify the code outside of the normal EDK II build system process flow. |
| `OUTFLAGS`        | This specified an output flag for ACPI (ASL and IASL) tools. |
| `OUTPUT`          | This specifies an output flag for the Assembler (ASM) command. |
| `PATH`            | This is the full path and executable name for a command code. For executables that are in the BaseTools paths (or that are in directories specified in the OS PATH environment variable) only the name of the executable is required.
| `BUILDRULEORDER`  | This attribute is used by tools to process files listed in INF [Sources] sections in priority order. If a filename is listed with multiple extensions, the tools will use only the file that matches the first extension in the space separated list.

#### 5.2.3.1 BUILDRULEORDER Example

The following is an example use of the `BUILDRULEORDER` attribute.

```ini
[Sources]
  Foo.s
  Foo.asm
  Foo.nasm
```
The tools_def.txt file has the entry.

`*_*_*_*_BUILDRULEORDER = nasm asm Asm ASM S s`

The Foo.nasm file will be processed, and the Foo.s and Foo.asm files will be
ignored during the build. If a file is listed in the [Sources] section and the
file extension is not listed a section that is specified for a build FAMILY
(or BUILDRULEFAMILY if specified as an attribute in the build_rule.txt file)
for the selected tool chain (GCC for example) in the build_rule.txt file, then
the file is ignored. For example, if the INF has the following section listed:
UefiCpuPkg/Library/BaseUefiCpuLib/BaseUefiCpuLib.inf

```ini
[Sources.IA32]
  Ia32/InitializeFpu.asm
  Ia32/InitializeFpu.S

[Sources.X64]
  X64/InitializeFpu.asm
  X64/InitializeFpu.S
```

If the tool chain is a GCC tool chain, then only the .S files would be
processed and the .asm files will be ignored.
