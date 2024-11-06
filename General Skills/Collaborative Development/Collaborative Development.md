# Collaborative Development #
 
## Overview ##

75 points

Category: [General Skills](../)

Tags: `#generalskills #git`

## Description ##

My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?
You can download the challenge files here:
challenge.zip

## Approach ##

Unzipping the `challenge.zip` archive we have a git local repository.

Listing the contents of the unzipped folder reveals a single file.

    $ ls
    flag.py
    
    $ cat flag.py 
    print("Printing the flag...")

Nothing interesting found in `git` `log`, `blame` or `status` commands.

Listing the branches of the repository, we see three separate feature branches as well as our current `main` branch we have checked out currently :

    $ git branch
      feature/part-1
      feature/part-2
      feature/part-3
    * main

## Solution ##

Lets progressively checkout the feature branches and check the contents of the `flag.py` file :

    $ git checkout feature/part-1
    Switched to branch 'feature/part-1'
    
    $ ls
    flag.py
    
    $ cat flag.py 
    print("Printing the flag...")
    print("picoCTF{...........", end='')
    
    $ git checkout feature/part-2
    Switched to branch 'feature/part-2'
    
    $ cat flag.py 
    print("Printing the flag...")
    
    print("redacted", end='')
    
    $ git checkout feature/part-3
    Switched to branch 'feature/part-3'
    
    $ cat flag.py 
    print("Printing the flag...")
    
    print("...........}")

Putting the pieces of the flag together we have :

    picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.
    