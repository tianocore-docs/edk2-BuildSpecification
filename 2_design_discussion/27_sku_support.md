<!--- @file
  2.7 SKU Support

  Copyright (c) 2008-2018, Intel Corporation. All rights reserved.<BR>

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
a platform driver. The build system also supports building a specific SKU.

The SKU enabled PCD sections (defined by a PCD section tag in the DSC file that
contains a SKUID modifier that is not DEFAULT) are a sparsely populated set of
configuration settings. The platform developer may specify one or more PCDs that
will have different values than the PCD values specified by the default SKU.
Additional PCDs not listed in a default PCD section may also be specified under
a section with a SKUID modifier.

During runtime, the PCD drivers will automatically return a default SKU value
if no specific SKU value was specified after a platform driver calls SetSku().
The configuration elements, PCDs, must be accessed using either Dynamic or
DynamicEx PCD access methods. When building and image that supports multiple
SKUs, the Feature Flag, Fixed At Build and Patchable In Module PCDs will only
use the default SKU configuration settings. The default configuration settings
are identified by PCD section tags that have either a Default SKUID modifier
or have not SKUID modifier. The set of SKUs that can be included is configurable
either through setting the list in the DSC file `[Defines]` section's 
`SKU_IDENTIFIER` element or through setting one or more -x SKUID command-line
options to the build command.

When building a single SKU, it is possible to use SKU specific Feature Flag, Fixed
At Build and Patchable In Module configuration elements along with the Dynamic and
DynamicEx PCD for the specific SKU. The build tools will automatically adjust the
SKU specific Dynamic and DynamicEx PCD values overriding the default values. This
is equivalent of running SetSku immediately on reset.

**********
**Note:** If there are no PcdsDynamic or PcdsDynamicEx section tags that use a
SkuId modifier, then only the DEFAULT values will be placed into the PCD Database.
The platform drivers must not call SetSku() for this single SKU, as the 'DEFAULT'
SKU will be the only SKU available when built by this method.
**********

The following examples show the three types of builds.

**DEFAULT SKUID Build**
One or more SKUID | SKUIDENTIFIER entries may appear in the DSC file's [SkuIds]
section as in the following example:

```
[SkuIds]
  0 | DEFAULT
  1 | ScsiSku
  2 | SataSku
  3 | iScsiSku
```

Only the `DEFAULT` SKU will be built as identified by a single entry in the
`SKU_IDENTIFIER` in the DSC file's `[Defines]` section as in the following
example:

  `SKU_IDENTIFIER = DEFAULT`

The user is not required to specify:

  `-x DEFAULT`

FeatureFlag, PatchableInModule and FixedAtBuild PCD values from the values in
the default PCD sections. 

Dynamic and DynamicEx PCD values from the default PCD sections (sections that
do not contain a SkuId modifier in the section tag or that contain a SkuId
modifier of DEFAULT) must be used. These values will then be placed in the
DEFAULT table of the PCD Database.

**********
**WARNING:** The platform drivers must not call SetSku() for this single SKU, as
the 'DEFAULT' SKU will be the only SKU available when built by this method.
**********


**Single SKUID Build**
This method is useful for debugging as well as for size-optimization.

More than one `SKUID | SKUIDENTIFIER` entry must appear in the DSC file's `[SkuIds]`
section as in the following example:

```
[SkuIds]
  0 | DEFAULT
  1 | ScsiSku
  2 | SataSku
  3 | iScsiSku
```

Only one of the possible SKUs will be built as identified by a single entry in
the `SKU_IDENTIFIER` in the DSC file's `[Defines]` section as in the following
example:

  `SKU_IDENTIFIER = ScsiSku`

**********
**Note:** A `SKU_IDENTIFIER = DEFAULT | ScsiSku` must be treated by tools as exactly
the same as if just `SKU_IDENTIFIER = ScsiSku` had been specified.
**********

If the users specifies the following option on the build command-line, only SataSku
SKUID will be included:

  `-x SataSku`

The `-x SKUIDENTIFIER` command-line option overrides the `SKU_IDENTIFIER` statement in
the `[Defines]` section.

FeatureFlag, PatchableInModule and FixedAtBuild PCD values from the PCD section
that contains the matching SKUID modifier will override the values in the default
PCD sections.

Dynamic and DynamicEx PCD values from PCD sections that contain the matching SKUID
modifier will override the values from the default PCD section. These values will
then be placed in the DEFAULT table of the PCD Database. This is equivalent of
executing a SetSku() immediately on reset/power-on.

**********
**WARNING:** The platform drivers must not call SetSku() for this single SKU build,
as the 'DEFAULT' SKU will be the only SKU available when built by this method.
**********


**Multiple SKUID Build**
The DEFAULT SKU is always included in a multi-SKU platform build as these are the
default values returned by PcdGet statements until such time as a platform driver
executes a SetSku() call.

More than two `SKUID | SKUIDENTIFIER` entries must appear in the DSC file's `[SkuIds]`
section as in the following example:

```
[SkuIds]
  0 | DEFAULT
  1 | ScsiSku
  2 | SataSku
  3 | iScsiSku
```

Only two of the possible SKUs will be built as identified by a list of `SKU_IDENTIFIER`
in the DSC file's `[Defines]` section as in the following example:

  `SKU_IDENTIFIER = ScsiSku | SataSku`

**********
**Note:** A `SKU_IDENTIFIER = DEFAULT | ScsiSku | SataSku` must be treated by tools as
exactly the same as if this `SKU_IDENTIFIER = ScsiSku | SataSku`  statement had been
specified.
**********

If the users specifies the following options on the build command-line, all of the
SKUIDs will be included (DEFAULT is always included):

  `-x ScsiSku -x SataSku -x iScsiSku`

FeatureFlag, PatchableInModule and FixedAtBuild PCD values must come from the default
PCD sections; PCD sections for these access types that have a SKUID modifier must be
ignored by the Build Tools. If a PCD listed in a PCD section with a SKUID modifier
is NOT listed in the default PCD section, the PCD cannot be used by any module
included in the build.

Dynamic and DynamicEx PCD values from the DEFAULT SKU as well as values from the
specified SKUs will be put into tables in the PCD Database. Prior to a platform driver
is calling SetSku(), drivers accessing the PCD database will get values from the DEFAULT
SKU. Once a platform driver calls SetSku(), the values for the specific SKU will be
returned (unless there is no entry for the PCD in the specific SKU table, in which case,
the value will come from the DEFAULT SKU table).
