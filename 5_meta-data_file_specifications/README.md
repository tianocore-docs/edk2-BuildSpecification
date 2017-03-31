<!--- @file
  5 Meta-Data File Specifications

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

# 5 Meta-Data File Specifications

This chapter defines the format of two files used by the build. The two files
are: `tools_def.txt`, which defines the location and options for third party
tools and `target.txt`, which defines the top level default configuration. A
third file, `build_rule.txt`, which specifies the rules for creating binary
files, will not normally be modified by users, however since this file is
closely coupled with the build system, certain changes to build tools will
require updating (overwriting) the active copy. The format for `build_rule.txt`
is not included in this document.

Templates for these files are in the `$(EDK_TOOLS_PATH)/Conf` directory. The
`edksetup` script installs the active copies of these files into the
`$(WORKSPACE)/Conf` directory only if they do not exist. It is permissible to
have the `Conf` directory (the directory containing `target.txt`) located
outside of the `WORKSPACE` directory, however either the absolute or `WORKSPACE`
relative directory must be specified on the build command-line using the
"--conf" option when they are not in the active `WORKSPACE/Conf` directory.
