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
Copyright (c) 2007 - 2017, Intel Corporation All rights reserved.

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -a TARGETARCH, --arch=TARGETARCH
                        ARCHS is one of list: IA32, X64, IPF, ARM, AARCH64 or
                        EBC, which overrides target.txt's TARGET_ARCH
                        definition. To specify more archs, please repeat this
                        option.
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
                        MAX_CONCURRENT_THREAD_NUMBER. When value is set to 0,
                        tool automatically detect number of processor threads,
                        set value to 1 means disable multi-thread build, and
                        set value to more than 1 means user specify the threads
                        number to build.
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
  --pcd=OPTIONPCD       Set PCD value by command line. Format: "PcdName=Value"
  -l COMMANDLENGTH, --cmd-len=COMMANDLENGTH
                        Specify the maximum line length of build command.
                        Default is 4096.
  --hash                Enable hash-based caching during build process.
  --binary-destination=BINCACHEDEST
                        Generate a cache of binary files in the specified
                        directory.
  --binary-source=BINCACHESOURCE
                        Consume a cache of binary files from the specified
                        directory.
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

### D.4.3 PCD Option Definition

This section provides the EBNF for the `--pcd` option, which allows users to
specify PCD values on the command-line. PCD values on the command-line take
precedence over PCD provided in DSC, FDF, INF, and DEC files.

#### Prototype

```c
<PcdOption>       ::= "--pcd" <PcdName> ["=" <PcdValue>] <MTS>
<SP>              ::= 0x20
<MTS>             ::= <SP>+
<TS>              ::= <SP>*
<CommaSpace>      ::= "," <SP>*
<HexDigit>        ::= (a-fA-F0-9)
<CName>           ::= A valid C variable name.
<PcdName>         ::= [<TokenSpaceCName> "."] <PcdCName> ["." <Field>]
<TokenSpaceCName> ::= C Variable Name of the Token Space GUID
<PcdCName>        ::= C Variable Name of the PCD
<Field>           ::= C Variable Name of the Structure PCD field
<PcdValue>        ::= {<Boolean>} {<Number>} {<String>} {<Array>}
<Number>          ::= {<Integer>} {<HexNumber>}
<Integer>         ::= {(0-9)} {(1-9)(0-9)+}
<HexNumber>       ::= {"0x"} {"0X"} (a-fA-F0-9){1,16}
<Boolean>         ::= {<True>} {<False>}
<True>            ::= {"TRUE"} {"True"} {"true"} {"1"} {"0x1"} {"0x01"}
<False>           ::= {"FALSE"} {"False"} {"false"} {"0"} {"0x0"} {"0x00"}
<String>          ::= {<QuotedStr>} {<SglQuotedStr>}
<QuotedStr>       ::= ["L"] <DblQuote> <PrintChars>+ <DblQuote>
<SglQuotedStr>    ::= ["L"] <DblQuote> "\" <SglQuote> <PrintChars>+
                      "\" <SglQuote> <DblQuote>
<PrintChars>      ::= {<TS>} {<CChars>}
<DblQuote>        ::= 0x22
<SglQuote>        ::= 0x27
<CChars>          ::= {0x21} {(0x23 - 0x26)} {(0x28 - 0x5B)} {(0x5D - 0x7E)}
                      {<EscapeSequence>}
<EscapeSequence>  ::= "\" {"n"} {"t"} {"f"} {"r"} {"b"} {"0"} {"\"}
                      {<DblQuote>} {<SglQuote>}
<Array>           ::= "H" <DblQuote> "{"[<Lable>] <ArrayVal>
                      [<CommaSpace> [<Lable>] <ArrayVal>]*"}" <DblQuote>
<ArrayVal>        ::= {<Num8Array>} {<GuidStr>} {<DevicePath>}
<ShortNum>        ::= (0-255)
<IntNum>          ::= (0-65535)
<LongNum>         ::= (0-4294967295)
<LongLongNum>     ::= (0-18446744073709551615)
<UINT8>           ::= {"0x"} {"0X"} (a-fA-F0-9){1,2}
<UINT16>          ::= {"0x"} {"0X"} (a-fA-F0-9){1,4}
<UINT32>          ::= {"0x"} {"0X"} (a-fA-F0-9){1,8}
<UINT64>          ::= <HexNumber>
<ArrayString>     ::= {<ArrayQuotedStr>} {<ArraySglQuotedStr>}
<ArrayQuotedStr>  ::= ["L"] "\" <DblQuote> <PrintChars>* "\" <DblQuote>
<ArraySglQuotedStr>::= ["L"] "\" <SglQuote> <PrintChars>* "\" <SglQuote>
<NonNumType>      ::= {<Boolean>} {<ArrayString>} {<Offset>} {<UintMac>}
<Num8Array>       ::= {<NonNumType>} {<ShortNum>} {<UINT8>}
<Num16Array>      ::= {<NonNumType>} {<IntNum>} {<UINT16>}
<Num32Array>      ::= {<NonNumType>} {<LongNum>} {<UINT32>}
<Num64Array>      ::= {<NonNumType>} {<LongLongNum>} {<UINT64>}
<GuidStr>         ::= "GUID(" <GuidVal> ")"
<GuidVal>         ::= {"\"<DblQuote> <RegistryFormatGUID> "\"<DblQuote>}
                      {<CFormatGUID>} {<CName>}
<RegistryFormatGUID>::= <RHex8> "-" <RHex4> "-" <RHex4> "-" <RHex4> "-"
                      <RHex12>
<RHex4>           ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit>
<RHex8>           ::= <RHex4> <RHex4>
<RHex12>          ::= <RHex4> <RHex4> <RHex4>
<RawH2>           ::= <HexDigit>? <HexDigit>
<RawH4>           ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>
<OptRawH4>        ::= <HexDigit>? <HexDigit>? <HexDigit>? <HexDigit>?
<Hex2>            ::= {"0x"} {"0X"} <RawH2>
<Hex4>            ::= {"0x"} {"0X"} <RawH4>
<Hex8>            ::= {"0x"} {"0X"} <OptRawH4> <RawH4>
<Hex12>           ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4> <RawH4>
<Hex16>           ::= {"0x"} {"0X"} <OptRawH4> <OptRawH4> <OptRawH4>
                      <RawH4>
<CFormatGUID>     ::= "{" <Hex8> <CommaSpace> <Hex4> <CommaSpace>
                      <Hex4> <CommaSpace> "{"
                      <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                      <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                      <Hex2> <CommaSpace> <Hex2> <CommaSpace>
                      <Hex2> <CommaSpace> <Hex2> "}" "}"
<DevicePath>      ::= "DEVICE_PATH(" <DevicePathStr> ")"
<DevicePathStr>   ::= A double quoted string that follow the device path
                      as string format defined in UEFI Specification 2.6
                      Section 9.6
<UintMac>         ::= {<Uint8Mac>} {<Uint16Mac>} {<Uint32Mac>} {<Uint64Mac>}
<Uint8Mac>        ::= "UINT8(" <Num8Array> ")"
<Uint16Mac>       ::= "UINT16(" <Num16Array> ")"
<Uint32Mac>       ::= "UINT32(" <Num32Array> ")"
<Uint64Mac>       ::= "UINT64(" <Num64Array> ")"
<Lable>           ::= "LABEL(" <CName> ")"
<Offset>          ::= "OFFSET_OF(" <CName> ")"

**********
**Note:** The " and ' inside the string, must use escape character format (\", \').
**********

```
