# Scan Surprise #
 
## Overview ##

50 points

Category: [Forensics](../)

Tags: `#forensics #image`

## Description ##

I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?
You can download the challenge files here:
`challenge.zip`

## Approach ##

Unzipping the provided `challenge.zip`, a single `png` file is extracted.

    $ unzip challenge.zip 
    Archive:  challenge.zip
       creating: home/ctf-player/drop-in/
     extracting: home/ctf-player/drop-in/flag.png  

## Solution ##

Viewing the `flag.png` file presents a QR code. Scanning the QR code reveals the flag.
