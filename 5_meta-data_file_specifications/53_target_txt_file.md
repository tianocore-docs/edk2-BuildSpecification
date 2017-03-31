<!--- @file
  5.3 target.txt File

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

## 5.3 target.txt File

This file is used to filter the build so that only required components are
used. It also provides pointers to the `tools_def.txt` file, and the active
`build_rule.txt` files. All file names are relative to the system environment
variable, `WORKSPACE`. No wildcard characters are permitted in this file. All
entries in this file are case-sensitive.

While the values in this file filter what will be built, the `TARGET`,
`TARGET_ARCH` and `TOOL_CHAIN_TAG` values may also be overridden on the build
tool's command line.

The following well known macro names may be used in other EDK II meta-data
files, `$(TARGET)`, `$(ARCH)` and `$(TOOL_CHAIN_TAG)` and are mapped to the
`TARGET`, `TARGET_ARCH` and `TOOL_CHAIN_TAG` in this file or from options
specified on the command line which override the settings in this file.

#### Prototype

```c
<TargetText>   ::= [<Platform>]
                   [<Target>]
                   [<TargetArch>]
                   [<ToolsDef>]
                   [<ToolTagName>]
                   [<ThreadEnable> <EOL>]
                   [<BldRuleConf>]
<TabSpace>     ::= {0x09} {0x20}
<TS>           ::= <TabSpace>*
<MTS>          ::= <TabSpace>+
<Eq>           ::= <TS> "=" <TS>
<Platform>     ::= "ACTIVE_PLATFORM" <Eq> [PlatformFile] <EOL>
<Target>       ::= "TARGET" <Eq> [<Targets>] <EOL>
<Targets>      ::= TargetVal [" " TargetVal]*
<TargetArch>   ::= "TARGET_ARCH" <Eq> [<Archs>] <EOL>
<Archs>        ::= Arch [" " Arch]*
<ToolsDef>     ::= "TOOL_CHAIN_CONF" <Eq> ToolDefsFile <EOL>
<ToolTagName>  ::= "TOOL_CHAIN_TAG" <Eq> TagName <EOL>
<ThreadEnable> ::= "MAX_CONCURRENT_THREAD_NUMBER" <Eq> [<NumThrds>]
<NumThrds>     ::= (1-9) [(0-9)]*
<BldRuleConf>  ::= "BUILD_RULE_CONF" <Eq> BuildRulesFile <EOL>
<Paths>        ::= (a-zA-Z0-9) (a-zA-Z0-9\_\-)* "/"
<Filenames>    ::= <Paths>* (a-zA-Z0-9)(a-zA-Z0-9\_\-)* ["." <Ext>]
<Ext>          ::= (a-zA-Z0-9)+
<EOL>          ::= <TS> 0x0D 0x0A
```

#### Parameters

**_PlatformFile_**

Specify the `WORKSPACE` relative Path and Filename of the platform DSC file
that will be used for the build. This line is required only if the current
working directory does not contain one or more DSC files.

**_TargetVal_**

Zero or more of the following: `NOOPT`, `DEBUG`, `RELEASE`, a user defined word
in the `tools_def.txt` file; separated by a space character. If the line is
missing or no value is specified, all valid targets specified in the DSC file
will attempt to be built.

**_Arch_**

The target architectures that are specified on the command-line override the
`TARGET_ARCH` entry in the `target.txt` file. The resulting architecture must
also be listed as one of the architectures in the `SUPPORTED_ARCHITECTURES`
entry in the DSC file's `[Defines]` section. If the target architecture is not
specified on the command-line and the `TARGET_ARCH` entry does not exist in the
`target.txt` file, then all valid architectures specified in the DSC file, for
which tools are available, will be built. The architectures are space
separated.

**_ToolDefsFile_**

Specify the name of the filename to use for specifying the tools to use for the
build.

If not specified, the file: `WORKSPACE/Conf/tools_def.txt` will be used for the
build. The path and file name must be relative to the `WORKSPACE` directory.

**_TagName_**

Specify the name of the `tools_def.txt` tool chain tag name to use. If not
specified in this file and it is not specified using the `-t` option on the
command-line, then the build will break.

**_Integer_**

The number of concurrent threads. The default, if not specified or set to zero,
is 2 Recommend setting this value to one more than the number of computer cores
or CPUs of the development workstation.

**_BuildRulesFile_**

Specify the file name to use for the build rules that are followed when
generating Makefiles. If not specified, the file:
`WORKSPACE/Conf/build_rule.txt` will be used.

The path and file name must be relative to the `WORKSPACE` directory.
