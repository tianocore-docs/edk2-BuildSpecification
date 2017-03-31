<!--- @file
  10.4 Create the FV Image File(s)

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

## 10.4 Create the FV Image File(s)

Once all of the EFI FFS files have been created, these images are bundled into
an FV image.

GenFv needs two kinds of information about the target FV:

* The FV attributes
* The list of one or more files that will be placed into this FV.

This information is defined in the FV section of the FDF file.

If the `[FV]` section contains an `FvNameString` entry and it is set to `TRUE`,
the tools will use the `FvUiName` from the section tag to create an
`FvNameString` entry in the FV image's extension header.

The following example is for a FV section named "BiosUpdate."

```ini
[FV.BiosUpdate]
  BlockSize          = 0x10000
  FvAlignment        = 16
  ERASE_POLARITY     = 1
  MEMORY_MAPPED      = TRUE
  STICKY_WRITE       = TRUE
  LOCK_CAP           = TRUE
  LOCK_STATUS        = TRUE
  WRITE_DISABLED_CAP = TRUE
  WRITE_ENABLED_CAP  = TRUE
  WRITE_STATUS       = TRUE
  WRITE_LOCK_CAP     = TRUE
  WRITE_LOCK_STATUS  = TRUE
  READ_DISABLED_CAP  = TRUE
  READ_ENABLED_CAP   = TRUE
  READ_STATUS        = TRUE
  READ_LOCK_CAP      = TRUE
  READ_LOCK_STATUS   = TRUE

  FILE FV_IMAGE = EDBEDF47-6EA3-4512-83C1-70F4769D4BDE {
    SECTION GUIDED {
      SECTION FV_IMAGE = BiosUpdateCargo
    }
  }
```

This FV is very simple; it contains only one `FILE`. But this file contains
an entire FV image named `BiosUpdateCargo` which must be available when
**GenFds** creates the `BiosUpdate` FV.

The **GenFds** tool will process the FDF file and place the FV attributes and
contents in to an INF file (in this example, the `BiosUpdate.inf` file) and
then processing is transferred to **GenFv** tool when creating FV images. The
following example is what this generated, FV-style, INF file looks like:

```ini
[options]
EFI_BLOCK_SIZE = 0x10000

[attributes]
EFI_ERASE_POLARITY     = 1
EFI_WRITE_ENABLED_CAP  = TRUE
EFI_READ_ENABLED_CAP   = TRUE
EFI_READ_LOCK_STATUS   = TRUE
EFI_WRITE_STATUS       = TRUE
EFI_READ_DISABLED_CAP  = TRUE
EFI_WRITE_LOCK_STATUS  = TRUE
EFI_LOCK_CAP           = TRUE
EFI_LOCK_STATUS        = TRUE
EFI_ERASE_POLARITY     = 1
EFI_MEMORY_MAPPED      = TRUE
EFI_READ_LOCK_CAP      = TRUE
EFI_WRITE_DISABLED_CAP = TRUE
EFI_READ_STATUS        = TRUE
EFI_WRITE_LOCK_CAP     = TRUE
EFI_STICKY_WRITE       = TRUE
EFI_FVB2_ALIGNMENT_16  = TRUE

[files]
EFI_FILE_NAME = C:edk2BuildMyPlatformDEBUG_MYTOOLSFVFfsEDBEDF47-6EA34512-83C1-70F4769D4BDEEDBEDF47-6EA3-4512-83C1-70F4769D4BDE.ffs
```
