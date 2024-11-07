# Binary Search #
 
## Overview ##

100 points

Category: [General Skills](../)

Tags: `#generalskills #algorithm`

## Description ##

Want to play a game? As you use more of the shell, you might be interested in how they work! Binary search is a classic algorithm used to quickly find an item in a sorted list. Can you find the flag? You'll have 1000 possibilities and only 10 guesses.

Cyber security often has a huge amount of data to look through - from logs, vulnerability reports, and forensics. Practicing the fundamentals manually might help you in the future when you have to write your own tools!

You can download the challenge files here:
- challenge.zip

## Solution ##

Unzipping the provided `challenge.zip` we can inspect the `guessing_game.sh` shell script source.

Looks like we have 10 gueses to guess the correct random number between 1 and 1000, to drop the flag. As the name and description of the challenge suggests, using the binary search algorithm should get us quickly there. Halving the distance each guess to narrow in on the random number. 

Opening a session to the challenge server :

    $ ssh -p 55504 ctf-player@atlas.picoctf.net
    The authenticity of host '[atlas.picoctf.net]:55504 ([18.217.83.136]:55504)' can't be established.
    ED25519 key fingerprint is SHA256:M8hXanE8l/Yzfs8iuxNsuFL4vCzCKEIlM/3hpO13tfQ.
    This key is not known by any other names
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added '[atlas.picoctf.net]:55504' (ED25519) to the list of known hosts.
    ctf-player@atlas.picoctf.net's password: 
    Welcome to the Binary Search Game!
    I'm thinking of a number between 1 and 1000.
    Enter your guess: 500
    Higher! Try again.
    Enter your guess: 750
    Lower! Try again.
    Enter your guess: 625
    Lower! Try again.
    Enter your guess: 562
    Higher! Try again.
    Enter your guess: 593
    Higher! Try again.
    Enter your guess: 609
    Lower! Try again.
    Enter your guess: 601
    Higher! Try again.
    Enter your guess: 605
    Higher! Try again.
    Enter your guess: 607
    Congratulations! You guessed the correct number: 607
    Here's your flag: picoCTF{...........redacted.............}
    Connection to atlas.picoctf.net closed.

Where the actual flag value has been redacted for the purposes of this write up.
