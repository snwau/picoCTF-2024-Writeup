# Secret of the Polyglot #
 
## Overview ##

100 points

Category: [Forensics](../)

Tags: `#forensics`

## Description ##

The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?
Download the suspicious file here.

## Approach ##

Inspecting the challenge file `flag2of2-final.pdf ` for type, shows despite the `.pdf` file extension, `png` image data:

    $ file flag2of2-final.pdf 
    flag2of2-final.pdf: PNG image data, 50 x 50, 8-bit/color RGBA, non-interlaced

## Solution ##

Opening the `PDF` file yields the second half of the flag.

Copying the file with the `.png` extension and opening yields the first half of the flag.
