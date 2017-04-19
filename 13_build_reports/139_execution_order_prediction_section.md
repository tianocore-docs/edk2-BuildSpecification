<!--- @file
  13.8 Execution Order Prediction Section

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

## 13.8 Execution Order Prediction Section

This section contains platform level prediction for the execution flow. Each
phase list the following triple in their predicted order:

`(Type, Name, Module INF Path)`

%The entry point or notification function name%

#### Example

```
>======================================================================<
Execution Order Prediction
*P PEI phase
*D DXE phase
*E Module INF entry point name
*N Module notification function name
Type Symbol          Module INF Path
========================================================================
*PE   PeiCore        s:\edk2\MdeModulePkg\Core\Pei\PeiMain.inf
*PE   PcdPeimInit    s:\edk2\MdeModulePkg\Universal\Pcd\Pei\Pcd.inf
...
*PN EndOfPeiCallback s:\edk2\MyPlatform\PlatformPei\PlatformPei.inf
*DE DxeMain          s:\edk2\MdeModulePkg\Core\Dxe\DxeMain.inf
*DE PcdDxeInit       s:\edk2\MdeModulePkg\Universal\Pcd\Dxe\Pcd.inf
...
<======================================================================>
```

**********
**Note:** This section is present when **EXECUTIONORDER** is specified in
**-Y** option.
**********

The following figure shows the HTML format with an entry expanded.

![](../media/image24.png)

###### Figure 24 Report.html
