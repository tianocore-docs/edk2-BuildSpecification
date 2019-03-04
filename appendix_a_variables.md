<!--- @file
  Appendix A Variables

  Copyright (c) 2008-2019, Intel Corporation. All rights reserved.<BR>

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

# Appendix A Variables

One of the core concepts of this utility is the notion of symbols. Use of
symbols follows the makefile convention of enclosing within $(), for example
$(EFI_SOURCE). As the utility processes files during execution, it will often
perform parsing of variable assignments. These variables can then be referenced
in other sections of the DSC file. Variable assignments will be saved
internally in either a local or global symbol table. The local symbol table is
purged following processing of individual Platform (DSC) files. Global symbol
values persist throughout execution of the utility. Local symbol values take
precedent over global symbols. The following table describes the symbols
generated internally by the utility. They can be overridden either on the
command line, in the DSC file, or in individual INF files. The G/L column
indicates whether the symbol is typically a global (appears in all Makefiles)
or a local (to the module's Makefile) symbol.

Variable descriptions follow in Table 21.

**********
**Note:** This table does not list required system environment variables or
optional system environment variable.
**********

###### Table 21 Variable Descriptions

| Variable Name     | G/L | Description                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------------- |:---:| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BIN_DIR`         | L   | Specifies the directory where final component binaries are deposited during build. Typically `$(BUILD_DIR)\$(PROCESSOR)`                                                                                                                                                                                                                                                                                                   |
| `BUILD_DIR`       | G   | Defines the build tip directory for the current platform. For example, this may be `$(EFI_SOURCE)\Platform\Anacortes_870`.                                                                                                                                                                                                                                                                                                 |
| `BUILD_TYPE`      | L   | If defined, then the utility will copy the `[build.$(PROCESSOR).$(BUILD_TYPE)]` section from the DSC file to the component's makefile. If not specified, then the `[build.$(PROCESSOR).$(COMPONENT_TYPE)]` section will be used to emit command to build the component.                                                                                                                                                    |
| `DEST_DIR`        | L   | For a component, defines the directory (typically under `BUILD_DIR`) where the component object files are to be built.                                                                                                                                                                                                                                                                                                     |
| `DSC_FILENAME`    | G   | Name of the DSC file as specified on the command line. Can be used for dependencies in the Makefiles.                                                                                                                                                                                                                                                                                                                      |
| `FILE`            | L   | As the utility processes each source file in the Platform (DSC) file, this symbol gets assigned the name of the file, less the file extension.                                                                                                                                                                                                                                                                             |
| `FV_EXT`          | L   | Common component type (BS driver, application, etc.) have predefined file name extensions assigned (.dxe, .app, etc.). If there is a deviation from the convention, or a new (unknown to the utility) component type is being built, then `FV_EXT` may need to be defined for the component so the utility knows the result file name extension. This information is necessary to generate dependencies in `makefile.out`. |
| `INF_FILENAME`    | L   | Name of the INF file for a given component. Can be used for dependencies in the Makefiles.                                                                                                                                                                                                                                                                                                                                 |
| `LIB_DIR`         | L   | Specifies the directory where EFI libraries are deposited after building. Typically `$(BUILD_DIR)\$(PROCESSOR)`                                                                                                                                                                                                                                                                                                            |
| `MAKEFILE_NAME`   | L   | Name of the output makefile for the component. Default is "makefile". This value can be overridden to support building different variations of a component in the same DEST_DIR directory.                                                                                                                                                                                                                                 |
| `OUT_DIR`         | L   | Unused, but typically `$(BUILD_DIR)\$(PROCESSOR)`                                                                                                                                                                                                                                                                                                                                                                          |
| `PACKAGE`         | L/G | If defined, then the utility will create a package file named `$(DEST_DIR)\$(BASE_NAME).pkg`, and copy, with macro expansion, the `[package.$(COMPONENT_TYPE).$(PACKAGE)]` section from the DSC file to the output file.                                                                                                                                                                                                   |
| `PACKAGE_FILE`    | L   | If defined, then the utility will not generate a package file. The build can then use the value `$(PACKAGE_FILE)` to have GenFfsFile use an existing package file for creating the firmware file.                                                                                                                                                                                                                          |
| `PLATFORM`        | L   | This symbol can be used to provide more selectivity of files in the Platform (DSC) files. If assigned, then the utility will also process any files in the INF file under sections `[sources.$(PROCESSOR).$(PLATFORM)]`, `[includes.$(PROCESSOR).$(PLATFORM)]`, and `[libraries.$(PROCESSOR).$(PLATFORM)]`.                                                                                                                |
| `PROCESSOR`       | G/L | Defines the target processor for which the code is to be built. This symbol will typically be used to include or exclude source files in Platform (DSC) files, and to define the tool chain for building.                                                                                                                                                                                                                  |
| `SOURCE_DIR`      | L   | For a component, defines the directory of the component source files.                                                                                                                                                                                                                                                                                                                                                      |
