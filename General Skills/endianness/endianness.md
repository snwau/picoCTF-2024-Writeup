# endianness #
 
## Overview ##

200 points

Category: [General Skills](../)

Tags: `#generalskills #endian`

## Description ##

Know of little and big endian?

## Solution ##

Running the challenge we are presented with a randomised word, in this instance `"vipql"` represented in string form.

    $ nc titan.picoctf.net 49868
    Welcome to the Endian CTF!
    You need to find both the little endian and big endian representations of a word.
    If you get both correct, you will receive the flag.
    Word: vipql
    Enter the Little Endian representation:

Converting the characters of the string to hexadecimal byte values we get :

    vipql = \x76 \x69 \x70 \x71 \x6c

Entering the requested Little Endian representation we enter these hexadecimal values (without the escaping) from right to left (little or least significant end first).

Subsequently when requested to enter the Big Endian representation, do the same by from left to right (big or most significant end first).

    $ nc titan.picoctf.net 49868
    Welcome to the Endian CTF!
    You need to find both the little endian and big endian representations of a word.
    If you get both correct, you will receive the flag.
    Word: vipql
    Enter the Little Endian representation: 6C71706976
    Correct Little Endian representation!
    Enter the Big Endian representation: 766970716C
    Correct Big Endian representation!
    Congratulations! You found both endian representations correctly!
    Your Flag is: picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.
