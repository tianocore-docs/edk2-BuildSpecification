<!--- @file
  F.3 [Files] Section

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

## F.3 [Files] Section

#### Summary

Defines the `[files]` tag is found only in Firmware Volume INF files. This file
is created by the build utility and is an input to the **GenFv** utility.

#### Prototype

```c
<files>          ::= "[files]" <EOL>
                     <expression>+
<expression>     ::= <Filename> [<COMPONENT_TYPE>] [<FVS>]
                     [<FFSEXT>] ["PROCESSOR=" <arch>] [<APRORI>]
                     [<EFN>] <EOL>
<Filename>       ::= <PATH> <Word> <Extension>
<COMPONENT_TYPE> ::= Refer to Table "Component (module) Types"
<PATH>           ::= [[".."]{0,1} "\"]* {<Word> {"\"}{0,1}}*
<arch>           ::= {IA32} {X64} {IPF} {EBC}
<FVS>            ::= "FVs=" <FvImageName>[", <FvImageName>]*
<FvImageName>    ::= <Word>
<FFSEXT>         ::= "FFSExt=" <Extension>
<APRIORI>        ::= "APRIORI=" <FvImageNameIdx>
                     ["," <FvImageNameIdx>]*
<FvImageNameIdx> ::= <FvImageName> ":" <PositiveInt>
<PositiveInt>    ::= Integer value greater than 0
<EFN>            ::= "EFI_FILE_NAME" "=" <Path> <Arch>
                     <FileSep> <Word> <Extension>
<Extension>      ::= "." (a-zA-Z0-9_-)+
```

#### Example

```ini
[files]
  EFI_FILE_NAME = C:EdkSamplePlatformNt32BuildIA322D2E62CF-9ECF-43b7-821994E7FC713DFE-UsbKb.dxe
  EFI_FILE_NAME = C:EdkSamplePlatformNt32BuildIA32A5C6D68B-E78A-4426-9278A8F0D9EB4D8F-UsbMassStorage.dxe
  EFI_FILE_NAME = C:EdkSamplePlatformNt32BuildIA322D2E62AA-9ECF-43b7-8219-94E7FC713DFE-UsbMouse.dxe
  EFI_FILE_NAME = C:EdkSamplePlatformNt32BuildIA32961578FE-B6B7-44c3-AF356BC705CD2B1F-Fat.dxe
```
