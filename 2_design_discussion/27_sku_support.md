<!--- @file
  2.7 SKU Support

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

## 2.7 SKU Support

The EDK II build system provides the capability of supporting multiple SKUs in
a single firmware image. SKU selection is a runtime option that can be set from
a platform driver. During runtime, the configuration elements, PCDs, must be
accessed using either Dynamic or DynamicEx PCD access methods. When building
for multiple SKU support, the FeatureFlag, FixedAtBuild and PatchableInModule
access methods will only use the default SKU configuration settings. The set of
SKUs that can be included is configurable either through setting the list in
the DSC file or through setting commandline options to the build command.

The SKU enabled PCD sections are a sparsely populated set of configuration
settings; the PCD drivers will automatically return a default SKU value if no
specific SKU value was specified.

The build system also supports building a specific SKU. In this mode, it is
possible to use SKU specific FeatureFlag, FixedAtBuild and PatchableInModule
access method configuration elements along with the Dynamic and DynamicEx PCD
for the specific SKU. The build tools will automatically adjust the SKU
specific Dynamic and DynamicEx PCD values overriding the default values. This
is equivalent of running SetSku() immediately on reset.
