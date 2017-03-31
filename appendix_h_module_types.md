<!--- @file
  Appendix H Module Types

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

# Appendix H Module Types

###### Table 23 EDK II Module Types

| MODULE_TYPE          | Supported Architecture Types | Description                                                                                                                                                                                                                                                                                                      |
| -------------------- |:----------------------------:| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BASE`               | Any                          | Modules or Libraries can be ported to any execution environment. This module type is intended to be used by silicon module developers to produce source code that is not tied to any specific execution environment.                                                                                             |
| `SEC`                | Any                          | Modules of this type are designed to start execution at the reset vector of a CPU. They are responsible for preparing the platform for the PEI phase.                                                                                                                                                            |
| `PEI_CORE`           | Any                          | This module type is used by PEI Core implementations that are compliant with the PI Specification.                                                                                                                                                                                                               |
| `PEIM`               | Any                          | This module type is used by PEIMs that are compliant with the PI specification.                                                                                                                                                                                                                                  |
| `DXE_CORE`           | Any                          | This module type is used by DXE Core implementations that are compliant with the PI Specification.                                                                                                                                                                                                               |
| `DXE_DRIVER`         | Any                          | This module type is used by DXE Drivers that are compliant with the PI Specification.                                                                                                                                                                                                                            |
| `DXE_RUNTIME_DRIVER` | Any                          | This module type is used by DXE Drivers that are compliant to the PI Specification. These modules execute in both boot services and runtime services environments.                                                                                                                                               |
| `DXE_SAL_DRIVER`     | IPF                          | This module type is used by DXE Drivers that can be called in physical mode before SetVirtualAddressMap() is called and either physical mode or virtual mode after SetVirtualAddressMap() has been called. This module type is only available for IPF processor types.                                           |
| `DXE_SMM_DRIVER`     | IA32, X64                    | This module type is used by DXE Drivers that are loaded into SMRAM.                                                                                                                                                                                                                                              |
| `SMM_CORE`           | Any                          | This is the SMM core.                                                                                                                                                                                                                                                                                            |
| `UEFI_DRIVER`        | Any                          | This module type is used by UEFI Drivers that are compliant with the EFI 1.10 and UEFI specifications. These modules provide services in the boot services execution environment. UEFI Drivers that return EFI_SUCCESS are not unloaded from memory. UEFI Drivers that return an error are unloaded from memory. |
| `UEFI_APPLICATION`   | Any                          | This module type is used by UEFI Applications that are compliant with the EFI 1.10 and EFI 2.0 specifications. UEFI Applications are always unloaded when they exit.                                                                                                                                             |
