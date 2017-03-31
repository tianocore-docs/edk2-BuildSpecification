<!--- @file
  D.3 Build Targets and options

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

## D.3 Build Targets and options

In order to provide flexibility, the build command supports stopping the build
process after specific actions have taken place. These targets will ensure that
all previously required actions have been completed. New for this release is
the implementation of targets that permit processing files for only one given
step, such that previous steps are NOT processed. Table 22 provides the
descriptions of targets supported by the build, as well as the **GenFds** tools.

**********
**Note:** The flag, **--skip-autogen**, is required to prevent the build tool
from re-creating the auto generated C and Makefiles.
**********

###### Table 22 Build Targets and Command-line Options

| Target    | Description                                                                                                                                                                                                                                                                                                               |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| genc      | Generates the C code files (AutoGen.c, AutogGen.h and ModuleName.depex) then stops                                                                                                                                                                                                                                        |
| genmake   | Generates the C code files (AutoGen.c, AutoGen.h and ModuleName.depex) then the Makefiles, then stops                                                                                                                                                                                                                     |
| libraries | Generates the C code files, the Makefiles, then generates the object files for libraries                                                                                                                                                                                                                                  |
| modules   | Generates the C code files, the Makefiles, generates the object files for libraries, generates the object files for the modules, then links them, then calls GenFw for each of the intermediate final objects to create .efi files                                                                                        |
| fds       | Generates the C code files, the Makefiles, generates the object files for libraries, generates the object files for the modules, links them, calls GenFw for each of the intermediate final objects to create .efi files, generates SECTION files, generates FFS files, generates FV files and finally generates FD files |
