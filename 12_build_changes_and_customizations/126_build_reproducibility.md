<!--- @file
  12.6 Build Reproducibility

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

## 12.6 Build Reproducibility

The EDK II build system is designed to provide functional reproducibility, not
necessarily binary reproducibility. For example, when building the `DEBUG`
targets, the absolute path and file name for a PDB file is inserted into PE32
file. Building from different directory trees will result in different
directory paths, and the PE32 files will not be identical bit for bit, but they
will be identical in functionality.

Using the `RELEASE` build target will only result in identical files if there
are no changes whatsoever to the source files. The EDK II debug libraries
insert `DebugAssert` statements into binaries. These statements record line
numbers from the source files. This means that inserting a blank line or a
comment anywhere in a module can yield multiple different line numbers from the
`DebugAssert` statements.

Fortunately, a special compiler macro, `MDEPKG_NDEBUG`, has been in the EDK II
code base debug libraries. When this macro is defined, the DebugAssert
statements are stripped completely removed from the resultant binary. So using
a `RELEASE` and adding

the `MDEPKG_NDEBUG` macro will allow generating binaries that are identical,
regardless of the directories or changes to comments in the source files.

The best method for adding this macro is again, using the `[BuildOptions]`
section of the DSC file. The following is an example that is valid for all tool
chains with a family set to MSFT in the `tools_def.txt` file.

#### Example

```ini
[BuildOptions]
MSFT:RELEASE_*_*_CC_FLAGS = -D MDEPKG_NDEBUG
```

Using this flag is also the only way to turn off DebugAssert statements when
disabling all optimizations using the **/0d** flag for Microsoft\* compilers.
