<!--- @file
  D.4 Usage

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

## D.4 Usage

```ini
Usage: build.exe [options]
[all|fds|genc|genmake|clean|cleanall|cleanlib|modules|libraries|run]
Copyright (c) 2007 - 2014, Intel Corporation All rights reserved.

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -a TARGETARCH, --arch=TARGETARCH
                        ARCHS is one of list: IA32, X64, IPF, ARM, or EBC,
                        which overrides target.txt's TARGET_ARCH definition.
                        To specify more archs, please repeat this option.
  -p PLATFORMFILE, --platform=PLATFORMFILE
                        Build the platform specified by the DSC file name
                        argument, overriding target.txt's ACTIVE_PLATFORM
                        definition.
  -m MODULEFILE, --module=MODULEFILE
                        Build the module specified by the INF file name
                        argument.
  -b BUILDTARGET, --buildtarget=BUILDTARGET
                        Using the TARGET to build the platform, overriding
                        target.txt's TARGET definition.
  -t TOOLCHAIN, --tagname=TOOLCHAIN
                        Using the Tool Chain Tagname to build the platform,
                        overriding target.txt's TOOL_CHAIN_TAG definition.
  -x SKUID, --sku-id=SKUID
                        Using this name of SKU ID to build the platform,
                        overriding SKUID_IDENTIFIER in DSC file.
  -n THREADNUMBER       Build the platform using multi-threaded compiler. The
                        value overrides target.txt's
                        MAX_CONCURRENT_THREAD_NUMBER. Less than 2 will disable
                        multi-thread builds.
  -f FDFFILE, --fdf=FDFFILE
                        The name of the FDF file to use, which overrides the
                        setting in the DSC file.
  -r ROMIMAGE, --rom-image=ROMIMAGE
                        The name of FD to be generated. The name must be from
                        [FD] section in FDF file.
  -i FVIMAGE, --fv-image=FVIMAGE
                        The name of FV to be generated. The name must be from
                        [FV] section in FDF file.
  -C CAPNAME, --capsule-image=CAPNAME
                        The name of Capsule to be generated. The name must be
                        from [Capsule] section in FDF file.
  -u, --skip-autogen    Skip AutoGen step.
  -e, --re-parse        Re-parse all meta-data files.
  -c, --case-insensitive
                        Don't check case of file name.
  -w, --warning-as-error
                        Treat warning in tools as error.
  -j LOGFILE, --log=LOGFILE
                        Put log in specified file as well as on console.
  -s, --silent          Make use of silent mode of (n)make.
  -q, --quiet           Disable all messages except FATAL ERRORS.
  -v, --verbose         Turn on verbose output with informational messages
                        printed, including library instances selected, final
                        dependency expression, and warning messages, etc.
  -d DEBUG, --debug=DEBUG
                        Enable debug messages at specified level.
  -D MACROS, --define=MACROS
                        Macro: "Name [= Value]".
  -y REPORTFILE, --report-file=REPORTFILE
                        Create/overwrite the report to the specified filename.
  -Y REPORTTYPE, --report-type=REPORTTYPE
                        Flags that control the type of build report to
                        generate. Must be one of: [PCD, LIBRARY, FLASH, DEPEX,
                        HASH, BUILD_FLAGS, FIXED_ADDRESS, EXECUTION_ORDER].
                        To specify more than one flag, repeat this option on
                        the command line and the default flag set is [PCD,
                        LIBRARY, FLASH, DEPEX, HASH, BUILD_FLAGS,
                        FIXED_ADDRESS]
  -F FLAG, --flag=FLAG  Specify the specific option to parse EDK UNI file.
                        Must be one of: [-c, -s]. -c is for EDK framework UNI
                        file, and -s is for EDK UEFI UNI file. This option can
                        also be specified by setting *_*_*_BUILD_FLAGS in
                        [BuildOptions] section of platform DSC. If they are
                        both specified, this value will override the setting
                        in [BuildOptions] section of platform DSC.
  -N, --no-cache        Disable build cache mechanism
  --conf=CONFDIRECTORY  Specify the customized Conf directory.
  --check-usage         Check usage content of entries listed in INF file.
  --ignore-sources      Focus to a binary build and ignore all source files
```

### D.4.1 Debug Levels

The numeric debug levels are defined as integer values 0-9.

Level 0 will provide a few extra messages that might, under certain
environments, cause a build to break, during later stages of the build.

Level 1 provides messages from level 0, along with information related to PCDs.

Level 2 provides messages from levels 1 and 0, along with information related
to Macros.

Level 3 provides all messages from levels 0 - 2, along with information related
to Library Classes as well as generating code for PCDs during AutoGen.

Level 4 provides all previous level messages - no new information is added

Level 5 provides all previous level information as well as information
regarding the database that is used by the build system tools to decrease
incremental build times as well as HII information.

Levels 6 and 7 provides all previous messages - no new information is added

Level 8 provides all previous messages as well as adding build process
information, such as queues and threads running.

Level 9 provides the most details, displaying all previous messages and adding
information about what is happening at each step during the build.

### D.4.2 MACRO Option Definition

This section provides the EBNF for the `-D` option, which allows users to
specify macro values on the command-line. Macro values on the command-line take
precedence over Macros defined in the DSC and FDF files.

#### Prototype

```c
<MacroOption>    ::= {<ShortOpt>} {<LongOpt>}
<SP>             ::= 0x20
<MTS>            ::= <SP>+
<ShortOpt>       ::= "-D" <SP> <MACRO> ["=" <Value>] <MTS>
<LongOpt>        ::= "--define" "=" <MACRO> ["=" <Value>] <MTS>
<MACRO>          ::= (A-Z)(a-zA-Z0-9_)*
<Value>          ::= {<Number>} {<CString>} {<TrueFalse>} {<RegFmtGUID>}
<Number>         ::= {"0x" (a-fA-F0-9)+} {(0-9)+
<CString>        ::= ["L"] <QuotedString>
<QuotedString>   ::= <DblQuote> <CChars>* <DblQuote>
<DblQuote>       ::= 0x22
<CChars>         ::= {0x21} {(0x23 - 0x5B)} {(0x5D - 0x7E)} {<EscapeSequence>}
<EscapeSequence> ::= "\" {"n"} {"t"} {"f"} {"r"} {"b"} {"0"} {"\"} {0x22}
<TrueFalse>      ::= {"TRUE"} {"True"} {"true"} {"FALSE"} {"False"} {"false"}
<H4>             ::= (a-fA-F0-9) (a-fA-F0-9) (a-fA-F0-9) (a-fA-F0-9)
<H8>             ::= <H4> <H4>
<H12>            ::= <H4> <H4> <H4>
<RegFmtGUID>     ::= <H8> "-" <H4> "-" <H4> "-" <H4> "-" <H12>
```
