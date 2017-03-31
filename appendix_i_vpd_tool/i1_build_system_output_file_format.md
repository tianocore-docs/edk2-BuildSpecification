<!--- @file
  I.1 Build System Output File Format

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

## I.1 Build System Output File Format

#### Summary

The build system will generate a text file containing a list of PCDs that have
been declared as type VPD. An external tool that processes this file must be
capable of reading the following format.

#### Prototype

```c
<File>            ::= <AutoGenHeading>
                      [<CommentBlock>] [<PcdEntry>]*

<AutoGenHeading>  ::= "## @file" <EOL> "#" <EOL>
                      "# THIS IS AUTO-GENERATED FILE BY BUILD TOOLS"
                      " AND PLEASE DO NOT MAKE MODIFICATION." <EOL>
                      "#" <EOL>
                      "# This file lists all VPD information for a"
                      " platform collected by build.exe." <EOL>
                      "#" <EOL>
                      "# Copyright (c) 2010, Intel Corporation. All"
                      " rights reserved.<BR>" <EOL>
                      "# This program and the accompanying materials" <EOL>
                      "# are licensed and made available under the"
                      " terms and conditions of the BSD License" <EOL>
                      "# which accompanies this distribution. The"
                      " full text of the license may be found at" <EOL>
                      "# "
                      "http://opensource.org/licenses/bsd-license.php"
                      <EOL> "#" <EOL>
                      "# THE PROGRAM IS DISTRIBUTED UNDER THE BSD"
                      " LICENSE ON AN \"AS IS\" BASIS," <EOL>
                      "# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY"
                      " KIND, EITHER EXPRESS OR IMPLIED." <EOL>
<CommentBlock>    ::= ["#" <String> <EOL>]*
<PcdEntry>        ::= <PcdName> "|" <Offset> "|" <Size> "|" <PcdValue> <EOL>
<PcdName>         ::= <TokenSpaceCName> "." <PcdCName>
<TokenSpaceCName> ::= C Variable Name of the Token Space GUID
<PcdCName>        ::= C Variable Name of the PCD
<Offset>          ::= {"*"} {<Number>}
<HexNumber>       ::= "0x" (a-fA-F0-9)8
<Size>            ::= <Number>
<PcdValue>        ::= if (pcddatumtype == "BOOLEAN"):
                        <Boolean>
                      elif (pcddatumtype == "UINT8"):
                        <HexByte>
                      elif (pcddatumtype == "UINT16"):
                        <HexWord>
                      elif (pcddatumtype == "UINT32"):
                        <HexLong>
                      elif (pcddatumtype == "UINT64"):
                        <HexLongLong>
                      else:
                        <StringData> [<MaxSize>]
<Number>          ::= {<HexNumber>} {<NonNegativeInt>}
<PcdNumber>       ::= if NumType == UINT8
                        <HexByte>
                      if NumType == UINT16
                        <HexWord>
                      if NumType == UINT32
                        <HexLong>
                      if NumType == UINT64
                        <HexLongLong>
<HexByte>         ::= "0x"(a-fA-F0-9){1,2}
<HexWord>         ::= "0x" (a-fA-F0-9){1,4}
<HexLong>         ::= "0x" (a-fA-F0-9){1,8}
<HexLongLong>     ::= "0x" (a-fA-F0-9){1,16}
<Boolean>         ::= {<True>} {<False>}
<True>            ::= {"TRUE"} {"True"} {"true"} {"1"} {"0x1"} {"0x01"}
<False>           ::= {"FALSE"} {"False"} {"false"} {"0"} {"0x0"} {"0x00"}
<NonNegativeInt>  ::= (0-9)+
<StringData>      ::= {<QString>} {<CArray>}
<QString>         ::= ["L"] <DblQuote> <String> <DblQuote>
<DblQuote>        ::= 0x22
<CArray>          ::= "{" <NList> "}"
<NList>           ::= <HexByte> ["," <HexByte>]*
```

#### Example

```ini
## @file
#
# THIS IS AUTO-GENERATED FILE BY BUILD TOOLS AND PLEASE DO NOT MAKE MODIFICATION.
#
# This file lists all VPD information for a platform collected by build.exe.
#
# Copyright (c) 2010, Intel Corporation. All rights reserved.<BR>
# This program and the accompanying materials
# are licensed and made available under the terms and conditions of the BSD License
# which accompanies this distribution. The full text of the license may be found at
# http://opensource.org/licenses/bsd-license.php
#
# THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN "AS IS" BASIS,
# WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR IMPLIED.
#
gEfiMdeModulePkgTokenSpaceGuid.PcdVideoHorizontalResolution|*|4|800
gEfiMdeModulePkgTokenSpaceGuid.PcdVideoVerticalResolution|*|4|600
gEfiMdeModulePkgTokenSpaceGuid.PcdConOutRow|*|4|25
gEfiMdeModulePkgTokenSpaceGuid.PcdConOutColumn|*|4|80
```
