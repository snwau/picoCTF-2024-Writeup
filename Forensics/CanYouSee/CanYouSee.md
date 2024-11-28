# CanYouSee #
 
## Overview ##

100 points

Category: [Forensics](../)

Tags: `#forensics #exif #metadata #base64`

## Description ##

How about some hide and seek? Download this file here.

## Approach ##

Unzipping the `unknown.zip` challenge file, we extract a single `jpg` file:

    $ unzip unknown.zip 
    Archive:  unknown.zip
      inflating: ukn_reality.jpg      

Inspeting the EXIF data for the `jpg` file using `exiftool` we find:

    $ exiftool ukn_reality.jpg 
    ExifTool Version Number         : 12.40
    File Name                       : ukn_reality.jpg
    Directory                       : .
    File Size                       : 2.2 MiB
    File Modification Date/Time     : 2024:03:12 10:35:51+10:30
    File Access Date/Time           : 2024:03:16 06:44:52+10:30
    File Inode Change Date/Time     : 2024:03:16 06:36:45+10:30
    File Permissions                : -rw-r--r--
    File Type                       : JPEG
    File Type Extension             : jpg
    MIME Type                       : image/jpeg
    JFIF Version                    : 1.01
    Resolution Unit                 : inches
    X Resolution                    : 72
    Y Resolution                    : 72
    XMP Toolkit                     : Image::ExifTool 11.88
    Attribution URL                 : cGljb0NURntNRTc0RDQ3QV9ISUREM05fM2I5MjA5YTJ9Cg==
    Image Width                     : 4308
    Image Height                    : 2875
    Encoding Process                : Baseline DCT, Huffman coding
    Bits Per Sample                 : 8
    Color Components                : 3
    Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
    Image Size                      : 4308x2875
    Megapixels                      : 12.4

The value for the `Attribution URL` field looks suspiciously like a base64 encoded value. 

## Solution ##

Decoding the obtained `Attribution URL` value we find :

    $ echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fM2I5MjA5YTJ9Cg==" | base64 -d
    picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.
