<!--- @file
  F.4 [Options] Section

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

## F.4 [Options] Section

#### Summary

Defines the `[options]` tag is found only in Firmware Volume INF files. This is
an optional section.

#### Prototype

```c
<options>    ::= "[options]" <EOL>
                 <expression>+
<expression> ::= <Variable> "=" <Value> <EOL>
<Variable>   ::= {"EFI_BASE_ADDRESS"} {"EFI_BLOCK_SIZE"}
                 {"EFI_FILE_NAME"} {"EFI_NUM_BLOCKS"}
                 {"EFI_SYM_FILE_NAME"} ("IA32_RST_BIN"}
<CName>      ::= A valid C variable name
<VAL>        ::= "0x" <HexDigit>{1,8}
<Value>      ::= {<String>} {<VAL>} {<Filename>}
```

#### Example

```ini
[options]
  EFI_BASE_ADDRESS = 0xFFD80000
  EFI_FILE_NAME    = FvRecovery.fv
  EFI_NUM_BLOCKS   = 0x28
  EFI_BLOCK_SIZE   = 0x10000
```
