# packer #
 
## Overview ##

100 points

Category: [Reverse Engineering](../)

Tags: `#reverseengineering #packed #strings`

## Description ##

Reverse this linux executable?

## Approach ##

Firstly using `strings` to try and find anything of interest, we find reference to the use of an executable packer :

    $ strings out
    ...
    $Info: This file is packed with the UPX executable packer http://upx.sf.net $
    $Id: UPX 3.95 Copyright (C) 1996-2018 the UPX Team. All Rights Reserved. $
    ...

Downloading and installing the UPX packer :

    $ wget https://github.com/upx/upx/releases/download/v4.2.2/upx-4.2.2-amd64_linux.tar.xz
    $ xz -d upx-4.2.2-amd64_linux.tar.xz
    $ tar -xvf upx-4.2.2-amd64_linux.tar

Using `upx` with the `-d` option to decompress the challenge binary :

    $ ../bin/upx-4.2.2-amd64_linux/upx -d out
    
                          Ultimate Packer for eXecutables
                              Copyright (C) 1996 - 2024
    UPX 4.2.2       Markus Oberhumer, Laszlo Molnar & John Reiser    Jan 3rd 2024
    
            File size         Ratio      Format      Name
       --------------------   ------   -----------   -----------
    [WARNING] bad b_info at 0x4b718
    
    [WARNING] ... recovery at 0x4b714
    
        877724 <-    336520   38.34%   linux/amd64   out
    
    Unpacked 1 file.

## Solution ##

Repeating the `strings` check again on the unpacked binary, narrowing the search to references that include `"flag"` :

    $ strings out_unpacked | grep flag
    Password correct, please see flag: 7069636f4354467b5539585f556e5034636b314e365f42316e34526933535f31613561336633397d
    (mode_flags & PRINTF_FORTIFY) != 0
    WARNING: Unsupported flag value(s) of 0x%x in DT_FLAGS_1.
    version == NULL || !(flags & DL_LOOKUP_RETURN_NEWEST)
    flag.c
    _dl_x86_hwcap_flags
    _dl_stack_flags

The string representaton of a series of hexadecimal numbers is identified via the familiar starting sequence `\x70\x69\x63\x6f\x43\x54\x46 = "picoCTF"`, so lets run a quick conversion :

    $ python3
    Python 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> bytearray.fromhex("7069636f4354467b5539585f556e5034636b314e365f42316e34526933535f31613561336633397d").decode()
    'picoCTF{...........redacted.............}'

Where the actual flag value has been redacted for the purposes of this write up.
