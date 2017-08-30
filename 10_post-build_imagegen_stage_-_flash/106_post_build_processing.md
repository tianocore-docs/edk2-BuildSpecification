<!--- @file
  10.6 Post Build Processing

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


## 10.6 Post Build Processing

If the `[Defines]` section of the DSC file contains a `POSTBUILD = entry`
statement, prior to exiting, the script specified in the `POSTBUILD` statement
is executed. The entry of `POSTBUILD` support multiple arguments. And Tool
will convert arguments that are `WORKSPACE` or `PACKAGES_PATH` relative paths
to absolute paths. If the script file is not found, the **build** command
exits with an appropriate error message. If the script fails, it must terminate
with a non-zero exit code and the **build** command terminates with the exit
value from the post-build script. The script is required to generate error
messages that provide the reason for the termination.

All of the command line options passed into the **build** command are also
passed into the script along with the options for `TARGET`, `ARCH`,
`TOOL_CHAIN_TAG`, `ACTIVE_PLATFORM`, `Conf Directory`, and `build target`.

If the script terminates successfully (exit value of 0), then the **build**
command terminates normally.

**********
**Note:** This entry may be wrapped in a conditional directive. Unlike the
`PREBUILD` entry, there are no restrictions on the MACRO values used in a
conditional directive.
**********
**Note:** Quotes are needed when the script's additional options are present.
Quotes are also required if the path to the post-build command contains space
or special characters. Quotes may be used for arguments that have spaces or
special characters.
**********
