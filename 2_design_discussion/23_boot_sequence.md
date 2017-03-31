<!--- @file
  2.3 Boot Sequence

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

## 2.3 Boot Sequence

PI compliant system firmware must support the six phases: security (SEC),
pre-efi initialization (PEI), driver execution environment (DXE), boot device
selection (BDS), run time (RT) services and After Life (transition from the OS
back to the firmware) of system. Refer to Figure 3 below.

![](../media/image3.png)

###### Figure 3 PI Firmware Phases

### 2.3.1 Security (SEC)

The Security (SEC) phase is the first phase in the PI Architecture and is
responsible for the following:

* Handling all platform restart events
* Creating a temporary memory store
* Serving as the root of trust in the system
* Passing handoff information to the PEI Foundation

The security section may contain modules with code written in assembly.
Therefore, some EDK II module development environment (MDE) modules may contain
assembly code. Where this occurs, both Windows and GCC versions of assembly
code are provided in different files.

### 2.3.2 Pre-EFI Initialization (PEI)

The Pre-EFI Initialization (PEI) phase described in the PI Architecture
specifications is invoked quite early in the boot flow. Specifically, after
some preliminary processing in the Security (SEC) phase, any machine restart
event will invoke the PEI phase.

The PEI phase initially operates with the platform in a nascent state,
leveraging only on-processor resources, such as the processor cache as a call
stack, to dispatch Pre-EFI Initialization Modules (PEIMs). These PEIMs are
responsible for the following:

* Initializing some permanent memory complement
* Describing the memory in Hand-Off Blocks (HOBs)
* Describing the firmware volume locations in HOBs
* Passing control into the Driver Execution Environment (DXE) phase

### 2.3.3 Drive Execution Environment (DXE)

Prior to the DXE phase, the Pre-EFI Initialization (PEI) phase is responsible
for initializing permanent memory in the platform so that the DXE phase can be
loaded and executed. The state of the system at the end of the PEI phase is
passed to the DXE phase through a list of position independent data structures
called Hand-Off Blocks (HOBs). HOBs are described in detail in the
_Platform Initialization Specification_.

There are several components in the DXE phase:

* DXE Foundation
* DXE Dispatcher
* A set of DXE Drivers

### 2.3.4 Boot Device Selection (BDS)

The Boot Device Selection (BDS) phase is implemented as part of the BDS
Architectural Protocol. The DXE Foundation will hand control to the BDS
Architectural Protocol after all of the DXE drivers whose dependencies have
been satisfied have been loaded and executed by the DXE Dispatcher. The BDS
phase is responsible for the following:

* Initializing console devices
* Loading device drivers
* Attempting to load and execute boot selections

### 2.3.5 Transient System Load (TSL) and Runtime (RT)

The Transient System Load (TSL) is primarily the OS vendor provided boot
loader. Both the TSL and the Runtime Services (RT) phases may allow access to
persistent content, via UEFI drivers and UEFI applications. Drivers in this
category include PCI Option ROMs.

### 2.3.6 After Life (AL)

The After Life (AL) phase consists of persistent UEFI drivers used for storing
the state of the system during the OS orderly shutdown, sleep, hibernate or
restart processes.
