# Blame Game #
 
## Overview ##

75 points

Category: [General Skills](../)

Tags: `#generalskills #git`

## Description ##

Someone's commits seems to be preventing the program from working. Who is it?
You can download the challenge files here:
challenge.zip

## Approach ##

An initial inspection of the commit history, shows the flag as the Author of the commit :

    $ git log
    .....
    commit 0351e0474493168ca76441c24630c17554fd09ca
    Author: picoCTF{...........redacted.............} <ops@picoctf.com>
    Date:   Tue Mar 12 00:07:01 2024 +0000

        optimize file size of prod code

## Solution ##

Alternatively using the `blame` command in the spirit of the challenges name to show who was the last person to modify each line of the current revision file, we only have a single file in the challenge git repository :

    $ git blame message.py
    0351e047 (picoCTF{...........redacted.............} 2024-03-12 00:07:01 +0000 1) print("Hello, World!"
    

Where the actual flag value has been redacted for the purposes of this write up.
