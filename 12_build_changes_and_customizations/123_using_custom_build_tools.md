<!--- @file
  12.3 Using Custom Build Tools

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

## 12.3 Using Custom Build Tools

This section introduces how to use the custom tools in EDKII build system.

The custom tools can be classified to two types. One is used to create the EFI
guided section data, which must have its matched GUID value, such as the custom
compression tool introduced in the last section. Another is only used to
process files, which may not require its GUID, such as ASL compiler. This
section focuses on the later one.

First, the custom tool path and name needs to be added into `tools_def.txt`
file. For ASL compiler, it can be added like:

`*_*_*_ASL_PATH = DEF(ASL_PATH)\iasl.exe`

Then, "`$(TOOLNAME)`" is specified in `build_rule.txt` file to call this tool.
For ASL compiler, it can be used to process ASL source file. The rule to
process ASL file is added in `build_rule.txt` like:

```
[Build.Acpi-Source-Language-File]
  <InputFile>
    ?.asl, ?.Asl, ?.ASL
  <OutputFile>
    $(OUTPUT_DIR)(+)${s_base}.aml
  <Command.MSFT, Command.INTEL>
    "$(PP)" $(APP_FLAGS) $(INC) ${src} > ${d_path}(+)${s_base}.i
    "$(ASL)" -p ${dst} ${d_path}(+)${s_base}.i
```
