<!--- @file
  5.1 Build Meta-Data File Formats

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

## 5.1 Build Meta-Data File Formats

The following subsections describe the different parts of the build system's
meta-data files. These files are specific to the build process. Other EDK II
meta-data file formats are specified in their corresponding documents (see
Related Information in the Introduction.)

### 5.1.1 Comments

Within a meta-data file, comments are encouraged, with the hash "#" character
identifying a comment. In line comments terminate the processing of a line. In
line comments must be placed at the end of the line, and may not be placed
within the section ("[", "]", "<" or ">") tags. Comment characters can
be at the start of a line, or after a data element (there must be one or more
white space characters between the data element and the comment character.
Examples:

```
# this is a comment line
[Unicode-Text-File] # This is also a valid comment.

[Unicode-Text-File # This is not valid]
```

The last example is not valid, as the section header data element format is
`[text]` with the square brackets included as part of the data element.

Hash characters within a quoted string are permitted, and do not signify a
comment.

### 5.1.2 Valid Entries

All entries must appear on a single line, with entries terminated by either a
new line, or a comment.

