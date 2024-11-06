# Super SSH #
 
## Overview ##

25 points

Category: [General Skills](../)

Tags: `#generalskills #ssh`

## Description ##

Using a Secure Shell (SSH) is going to be pretty important.
Can you ssh as ctf-player to titan.picoctf.net at port 63095 to get the flag?
You'll also need the password f3b61b38. If asked, accept the fingerprint with yes.
If your device doesn't have a shell, you can use: https://webshell.picoctf.org
If you're not sure what a shell is, check out our Primer: https://primer.picoctf.com/#_the_shell

## Solution ##

Use the `<user>@<server>` syntax to specify the user for ssh session, otherwise it defaults to your current user account, non standard port numbers can be specified with the `-p <portnum>` argument.

    $ ssh ctf-player@titan.picoctf.net -p 63095
    The authenticity of host '[titan.picoctf.net]:63095 ([3.139.174.234]:63095)' can't be established.
    ED25519 key fingerprint is SHA256:4S9EbTSSRZm32I+cdM5TyzthpQryv5kudRP9PIKT7XQ.
    This key is not known by any other names
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '[titan.picoctf.net]:63095' (ED25519) to the list of known hosts.
    ctf-player@titan.picoctf.net's password: 
    Welcome ctf-player, here's your flag: picoCTF{...........redacted.............}
    Connection to titan.picoctf.net closed.

Where the actual flag value has been redacted for the purposes of this write up.
