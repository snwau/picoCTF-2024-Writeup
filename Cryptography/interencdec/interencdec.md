# interencdec #
 
## Overview ##

50 points

Category: [Cryptography](../)

Tags: `#cryptography #base64 #rot`

## Description ##

Can you get the real meaning from this file.
Download the file here.

## Approach ##

Inspecting the contents of the provided `enc_flag` file:

    $ cat enc_flag 
    YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclgyZzBOMm8yYXpZNWZRPT0nCg==

Looks to be base64 encoded, so let's decode:

    $ cat enc_flag | base64 -d
    b'd3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2g0N2o2azY5fQ=='

Decoded results again look to be base64 encoded:

    $ echo "d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrX2g0N2o2azY5fQ==" | base64 -d
    wpjvJAM{jhlzhy_k3jy9wa3k_h47j6k69}

Given the position of the `{` and `}` symbols with a prefix of `wpjvJAM` that takes the same length and mix of capitalisation of `picoCTF`, some form of rotational substitution cipher is suspected.

## Solution ##

Trying the various rotation distances facilitated by https://rot13.com, it was found to be a `ROT19` substitution cipher, when decrypted:

    picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.

Note that rather than brute force trying incremental rotations until the cipher successfully decrypted the input, having known what the decrypted prefix of the flag should have been, a quick calculation could have been performed.
