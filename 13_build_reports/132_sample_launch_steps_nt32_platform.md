<!--- @file
  13.2 Sample Launch Steps: NT32 platform

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

## 13.2 Sample Launch Steps: NT32 platform

BRG functionality is switched on by **-y** or **-Y** option from **build**
command. The following steps output the build report for NT32 platform:

1. Check out edk2 packages from https://svn.code.sf.net/p/edk2/code/trunk/edk2
   to `c:\Users\YourLogin\Documents\edk2` directory[^1].
2. Run **cmd.exe**, cd to your Documents directory and enter `subst s:`.
3. Cd to `s:\edk2`
4. Run **edksetup.bat --nt32**
5. Run **build.exe -a IA32 -p Nt32Pkg\Nt32Pkg.dsc -y ReportFile.txt**
  * **-y**: This option specifies the output file name for build report.
  * **-Y**: This option specifies flags that control the type of build report.
    It must be from the set of **PCD**, **LIBRARY**, **DEPEX**, **BUILD_FLAGS**,
    **FLASH**, **FIXED_ADDRESS** and **EXECUTION_ORDER**. To specify more than
    one flag, repeat the option on the command line. Example of usage:

    On the command line, append the following arguments:

    **-y report_filename.txt -Y PCD -Y FLASH -Y DEPEX**

    The default set of flags (if **-Y** is not specified) is: **PCD**,
    **LIBRARY**, **FLASH**, **DEPEX**, **BUILD_FLAGS** and **FIXED_ADDRESS**.


[^1] On Microsoft Windows 7, you must be an administrator to create a directory
in the root of the C: drive. It recommended that you checkout edk2 into your
User directory, then use the subst command to map that directory to a virtual
drive.
