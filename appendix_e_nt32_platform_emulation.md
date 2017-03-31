<!--- @file
  Appendix E NT32 Platform Emulation

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

# Appendix E NT32 Platform Emulation

The NT32Pkg provides a platform emulation environment that executes on windows
platform. The EDK II build program is used to start the emulation environment
after it has been built. The `Nt32Pkg\Nt32Pkg.dsc` file has been modified to
also build a version that will run on 64-bit versions of Windows. The
architectural modifier, **-a**, of the **build.exe** command is used to enable
this option.

Prior to building the platform: `Nt32Pkg\Nt32Pkg.dsc`, the user may want to
modify PCD settings in the file. The following PCDs control the mappings of
your system environment to the emulation environment.

`PcdWinNtSerialPort|L"COM1!COM2"|VOID*|18`

This maps the serial port to COM1 or COM2 (if COM1 is not available).

`PcdWinNtFileSystem|L".!..\.\.\.\\EdkShellBinPkg\\bin\\ia32\\Apps"|VOID*|106`

This shows the location of the shell applications.

`PcdWinNtGop|L"UGA Window 1!UGA Window 2"|VOID*|50`

This defines label for the two windows that are started.

`PcdWinNtConsole|L"Bus Driver Console Window"|VOID*|50`

This defines label for the windows that are started.

`PcdWinNtVirtualDisk|L"FW;40960;512"|VOID*|24`

This defines the max and block sizes for the virtual disk drive that is created.

`PcdWinNtMemorySize|L"64!64"|VOID*|10`

This defines the memory available for the emulator in megabytes.

`PcdWinNtPhysicalDisk|L"a:RW;2880;512!d:RO;307200;2048!j:RW;262144;512"|VOID*|100`

This defines the available storage devices that must be present at startup, A:,
D: and J: - you may want to change the drive letters to match the development
environment - note that you must not use the C: drive, as you could
inadvertently wipe it out.

`PcdWinNtUga|L"UGA Window 1!UGA Window 2"|VOID*|50`

This defines label for the two windows that are started
