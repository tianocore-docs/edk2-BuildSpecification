<!--- @file
  F.2 [Attributes] Section

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

## F.2 [Attributes] Section

#### Summary

Defines the `[Attributes]` tag is found only in Firmware Volume INF files. This
file is created by the tools and is an input to the **GenFv** utility. Refer to
the document, "_Intel(R) Platform Innovation Framework for EFI, Firmware Volume
Block Specification_" for more information on these values. This is an optional
section.

#### Prototype

```c
<attributes> ::= "[attributes]" <EOL>
                 <expression>

<expression> ::= ["EFI_READ_DISABLED_CAP" "=" <TrueFalse> <EOL>]
                 ["EFI_READ_ENABLED_CAP" "=" <TrueFalse> <EOL>]
                 ["EFI_READ_STATUS" "=" <TrueFalse> <EOL>]
                 ["EFI_WRITE_DISABLED_CAP" "=" <TrueFalse> <EOL>]
                 ["EFI_WRITE_ENABLED_CAP" "=" <TrueFalse> <EOL>]
                 ["EFI_WRITE_STATUS" "=" <TrueFalse> <EOL>]
                 ["EFI_LOCK_CAP" "=" <TrueFalse> <EOL>]
                 ["EFI_LOCK_STATUS" "=" <TrueFalse> <EOL>]
                 ["EFI_ERASE_POLARITY" "=" <ZeroOne> <EOL>]
                 ["EFI_STICK_WRITE" "=" <TrueFalse> <EOL>]
                 ["EFI_MEMORY_MAPPED" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_CAP" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_2" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_4" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_8" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_16" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_32" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_64" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_128" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_256" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_512" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_1K" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_2K" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_4K" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_8K" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_16K" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_32K" "=" <TrueFalse> <EOL>]
                 ["EFI_ALIGNMENT_64K" "=" <TrueFalse> <EOL>]
<TrueFalse>  ::= {<ZeroOne>} {<TF>}
<TF>         ::= {<True>} {<False>}
<True>       ::= {"TRUE"} {"True"} {"true"}
<False>      ::= {"FALSE"} {"False"} {"false"}
<ZeroOne>    ::= {"0"} {"1"}
<EOL>        ::= end of line
```

#### Example

```ini
[attributes]
  EFI_READ_DISABLED_CAP  = TRUE
  EFI_READ_ENABLED_CAP   = TRUE
  EFI_READ_STATUS        = TRUE
  EFI_WRITE_DISABLED_CAP = TRUE
  EFI_WRITE_ENABLED_CAP  = TRUE
  EFI_WRITE_STATUS       = TRUE
  EFI_LOCK_CAP           = TRUE
  EFI_LOCK_STATUS        = FALSE
  EFI_STICKY_WRITE       = TRUE
  EFI_MEMORY_MAPPED      = TRUE
  EFI_ERASE_POLARITY     = 1
  EFI_ALIGNMENT_CAP      = TRUE
  EFI_ALIGNMENT_2        = TRUE
  EFI_ALIGNMENT_4        = TRUE
  EFI_ALIGNMENT_8        = TRUE
  EFI_ALIGNMENT_16       = TRUE
  EFI_ALIGNMENT_32       = TRUE
  EFI_ALIGNMENT_64       = TRUE
  EFI_ALIGNMENT_128      = TRUE
  EFI_ALIGNMENT_256      = TRUE
  EFI_ALIGNMENT_512      = TRUE
  EFI_ALIGNMENT_1K       = TRUE
  EFI_ALIGNMENT_2K       = TRUE
  EFI_ALIGNMENT_4K       = TRUE
  EFI_ALIGNMENT_8K       = TRUE
  EFI_ALIGNMENT_16K      = TRUE
  EFI_ALIGNMENT_32K      = TRUE
  EFI_ALIGNMENT_64K      = TRUE
```
