<!--- @file
  Appendix K Third Party Tool Flags

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

# Appendix K Third Party Tool Flags

The following tables provide a summary of these "Best Known" options.

**********
**Note:** A reserved keyword, `MDEPKG_NDEBUG`, can be used for code size
reduction purposes.
**********

###### Table 24 Standard C File Compiler Options

| Microsoft                 | Intel          | GCC                    | Description                                                                                                                                                                      |
| ---------------------     | -------------- | ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `/nologo`                 | `/nologo`      |                        | Do not display compiler version information                                                                                                                                      |
| `/c`                      | `/c`           | `-c`                   | Compile C files to object (.obj) files only, do not link                                                                                                                         |
| `/WX`                     | `/WX`          | `-Werror`              | Force warnings to be reported as errors.                                                                                                                                         |
| `/GS-`                    | `/GS-`         |                        | Disable security checks                                                                                                                                                          |
|                           |                | `-Wno-missing-braces`  | Warn if an aggregate or union initializer is not fully bracketed. In the following example, the initializer for 'a' is not fully bracketed, but that for 'b' is fully bracketed. |
|                           |                | `-Wno-array-bounds`    | Disables warnings if subscripts to arrays are out of bounds.                                                                                                                     |
| `/W4`                     | `/W4`          | `-Wall`                | Warning level 4 - print errors, warnings and remarks (or enable most warning messages)                                                                                           |
| `/Gs32768`                |                |                        | Control stack (32768 bytes) checking calls                                                                                                                                       |
| `/Gy`                     | `/Gy`          |                        | Separate functions for linker.                                                                                                                                                   |
| `/O1ib2`                  | `/O1`          |                        | Optimize for minimum space, enable intrinsic functions, enable in-line expansion.                                                                                                |
|                           | `/Oi`          |                        | Enable Intrinsic functions                                                                                                                                                       |
|                           | `/Ob2`         | `-default-inline`      | In-line any function, at the compiler's discretion (same as `/Qip`)                                                                                                              |
|                           |                | `-O`                   | Optimize output file                                                                                                                                                             |
| `/GL`                     |                |                        | Enable link-time code generation                                                                                                                                                 |
| `/EHs-c-`                 |                |                        | Combine `/EHs-` and `/EHc-`                                                                                                                                                      |
|                           | `/EHs-`        |                        | Disable C++ EH (no SHE exceptions)                                                                                                                                               |
|                           | `/EHc-`        |                        | Disable extern C defaults to no throw                                                                                                                                            |
| `/GF`                     | `GF`           |                        | Enable read-only string pooling                                                                                                                                                  |
| `/GR-`                    |                |                        | Disable C++ RTTI                                                                                                                                                                 |
| **EDK II Specific Flags** |
| `/D UNICODE`              | `/D UNICODE`   | `-DUNICODE`            | define macro UNICODE                                                                                                                                                             |
| `/FIAutoGen.h`            | `/FIAutoGen.h` | `--include AutoGen.h`  | Always include AutoGen.h file                                                                                                                                                    |
| **Debug Specific Flags**  |
| `/Zi`                     | `/Zi`          | `-g`                   | Enable debugging information                                                                                                                                                     |
| `/Gm`                     | `/Gm`          |                        | Enable minimum rebuild                                                                                                                                                           |
|                           |                | `-fshort-wchar`        | Force the underlying type for "wchar_t" to be "unsigned short"                                                                                                                   |
|                           |                | `-fno-stack-protector` |                                                                                                                                                                                  |
|                           |                | `-fno-strict-aliasing` |                                                                                                                                                                                  |
|                           |                | `-ffunction-sections`  |                                                                                                                                                                                  |
|                           |                | `-fdata-sections`      |                                                                                                                                                                                  |
| **IPF Specific Flags**    |
| `/Ox`                     |                |                        | Maximum Optimization (`/Ogityb2 /Gs`)                                                                                                                                            |
| `/X`                      |                |                        | ignore standard places                                                                                                                                                           |
| `/QIPF_fr32`              |                |                        | Do not use upper 96 Floating Point Registers                                                                                                                                     |
| `/Zx`                     |                |                        | Generates debug-able optimized code. Only available in the IPF cross compiler or IPF native compiler.                                                                            |

###### Table 25 Assembly Flags

| Microsoft | GCC                  | Description                                  |
| --------- | -------------------- | -------------------------------------------- |
| `/nologo` |                      | Do not display assembler version information |
| `/c`      | `-c`                 | Generate object (.obj) files, do not link    |
| `/WX`     |                      | Treat warnings as errors                     |
| `/W3`     |                      | Warning level 3                              |
| `/Cx`     |                      | Preserve case in publics and externs         |
| `/coff`   |                      | Generate COFF format object files            |
| `/Zd`     |                      | Add line number debug info                   |
| `/Zi`     |                      | Add symbolic debug info (DEBUG target)       |
|           | `-x assembler`       | Input files are in assembly language         |
|           | `-imacros AutoGen.h` | Accept definition of macros in AutoGen.h     |


###### Table 26 C Compiler's Preprocessor Options

| Microsoft      | Intel          | GCC                     | Description                                       |
| -------------- | -------------- | ----------------------- | ------------------------------------------------- |
| `/nologo`      | `/nologo`      |                         | Do not display compiler version information       |
| `/E`           | `/E`           | `-E`                    | Preprocess only; do not compile, assemble or link |
| `/TC`          | `/TC`          | `-x assembler-with-cpp` | Compile as .c files                               |
| `/FIAutoGen.h` | `/FIAutoGen.h` | `--include AutoGen.h`   | Always include AutoGen.h file                     |

###### Table 27 C Compiler's Preprocessor Options for VFR files ONLY

| Microsoft                 | Intel           | GCC            | Description                                                              |
| ------------------------- | --------------- | -------------- | ------------------------------------------------------------------------ |
| `/nologo`                 | `/nologo`       |                | Do not display compiler version information                              |
| `/E`                      | `/E`            | `-E`           | Preprocess only; do not compile, assemble or link                        |
| `/TC`                     | `/TC`           | `-x c`         | Compile as .c files                                                      |
| `/D VFRCOMPILE`           | `/D VFRCOMPILE` | `-DVFRCOMPILE` | Used only for Preprocessing VFR files                                    |
|                           |                 | `-P`           | Used only for Preprocessing VFR files - do not generate #line directives |
| `/FI$(MOD_NAME)StrDefs.h` |                 |                | Force include of the module's StrDefs.h file.                            |

###### Table 28 Pre-compiled Header (PCH) Creation Flags


| Microsoft          | Intel     | GCC               | Description                                                                            |
| ------------------ | --------- | ----------------- | -------------------------------------------------------------------------------------- |
| `/nologo`          | `/nologo` |                   | Do not display compiler version information                                            |
| `/c`               | `/c`      | `-c`              | Compile C files to object (.obj) files only, do not link                               |
| `/W4`              | `/W4`     | `-Wall`           | Warning level 4 - print errors, warnings and remarks (or enable most warning messages) |
| `/WX`              | `/WX`     | `-Werror`         | Force warnings to be reported as errors.                                               |
| `/Gy`              | `/Gy`     |                   | Separate functions for linker.                                                         |
| `/GS-`             | `/GS-`    |                   | Disable security checks                                                                |
| `/O1`              | `/O1`     |                   | Optimize for Maximum Speed                                                             |
| `/Oi`              | `/Oi`     |                   | Enable Intrinsic functions                                                             |
| `/Ob2`             | `/Ob2`    | `-default-inline` | In-line any function, at the compiler's discretion (same as `/Qip`)                    |
| `/GL`              |           |                   | Enable link-time code generation                                                       |
| `/EHs-`            | `/EHs-`   |                   | Disable C++ EH (no SHE exceptions)                                                     |
| `/EHc-`            | `/EHc-`   |                   | Disable extern C defaults to no throw                                                  |
| `/GF`              | `/GF`     |                   | Enable read-only string pooling                                                        |
| `/Gs8192`          | `/Gs8192` |                   | Control stack (8192 bytes) checking calls                                              |
| `/TC`              | `/TC`     |                   | Compile as .c files                                                                    |
| `/Yc`              |           |                   | Create the .pch file                                                                   |
| `/Gm`              |           |                   | Enable minimal rebuilds                                                                |
| `/FpAutoGen.h.gch` |           |                   |                                                                                        |
| `/X`               | `/X`      |                   | Ignore standard places                                                                 |
| `/Zi`              | `/Zi`     |                   | Produce debugging information                                                          |

###### Table 29 Static Linker Flags

| Microsoft   | GCC | Description                                 |
| ----------- | --- | ------------------------------------------- |
| `/nologo`   |     | Do not display compiler version information |
| `/LTCG`     |     | Use link-time code generation               |

###### Table 30 Dynamic Linker Flags

| Microsoft              | GCC                        | Description                                                                                   |
| ---------------------- | -------------------------- | --------------------------------------------------------------------------------------------- |
| `/NOLOGO`              |                            | Do not display compiler version information                                                   |
| `/NODEFAULTLIB`        | `-nostdlib`                | Disable using default libraries                                                               |
| `/IGNORE:4086`         | N/A                        | Use `/Gz` option instead                                                                      |
| `/OPT:ICF=10`          |                            | Perform identical COMDAT folding (10 iterations) to remove duplicates.                        |
| `/MAP`                 | `-Map filename.map`        | Create a map file.                                                                            |
| `/ALIGN:32`            | `--section-alignment 0x20` | Use 32-byte alignment instead of the default 4K                                               |
|                        | `--file-alignment 0x20`    |                                                                                               |
| `/MACHINE:$$`          | N/A                        | Where $$ is one of: `I386`, `AMD64` or `IA64`                                                 |
| `/DLL`                 | `--dll`                    | The output is a DLL                                                                           |
| `/LTCG`                |                            | Use link-time code generation                                                                 |
| `/ENTRY:$(ENTRYPOINT)` | `--entry _$(ENTRYPOINT)`   | The function that specifies a starting address.                                               |
| `/SUBSYSTEM:CONSOLE`   | `--subsystem console`      | Do not use the EFI_* subsystem interface, as this is EFI 1.0 compliant, not UEFI compliant.   |
| `/SAFESEH:NO`          |                            | Do not produce an image with a table of safe exception handles                                |
| `/BASE:0`              | `--image-base 0x0`         | Base address is always 0, and will be adjusted later by the build tools when creating images. |
| `/DRIVER`              |                            | Specify Kernel mode                                                                           |
| `/DEBUG`               |                            | Create debugging information                                                                  |
|                        | `-O2`                      | Optimize                                                                                      |
|                        | `--gc-sections`            | Enable garbage collection of unused input sections                                            |
|                        | `--export-all-symbols`     | All global symbols in the objects used to build a DLL will be exported by the DLL.            |
