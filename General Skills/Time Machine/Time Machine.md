# Time Machine #
 
## Overview ##

50 points

Category: [General Skills](../)

Tags: `#generalskills #git`

## Description ##

What was I last working on? I remember writing a note to help me remember...
You can download the challenge files here:
challenge.zip

## Approach ##

Unzipping the provided `challenge.zip` archive, we immediately see its a git repository with the presence of the `.git` folder.

    $ unzip challenge.zip 
    Archive:  challenge.zip
       creating: drop-in/
      inflating: drop-in/message.txt     
       creating: drop-in/.git/
       creating: drop-in/.git/branches/
      inflating: drop-in/.git/description  
       creating: drop-in/.git/hooks/
      inflating: drop-in/.git/hooks/applypatch-msg.sample  
      inflating: drop-in/.git/hooks/commit-msg.sample  
      inflating: drop-in/.git/hooks/fsmonitor-watchman.sample  
      inflating: drop-in/.git/hooks/post-update.sample  
      inflating: drop-in/.git/hooks/pre-applypatch.sample  
      inflating: drop-in/.git/hooks/pre-commit.sample  
      inflating: drop-in/.git/hooks/pre-merge-commit.sample  
      inflating: drop-in/.git/hooks/pre-push.sample  
      inflating: drop-in/.git/hooks/pre-rebase.sample  
      inflating: drop-in/.git/hooks/pre-receive.sample  
      inflating: drop-in/.git/hooks/prepare-commit-msg.sample  
      inflating: drop-in/.git/hooks/update.sample  
       creating: drop-in/.git/info/
      inflating: drop-in/.git/info/exclude  
       creating: drop-in/.git/refs/
       creating: drop-in/.git/refs/heads/
     extracting: drop-in/.git/refs/heads/master  
       creating: drop-in/.git/refs/tags/
     extracting: drop-in/.git/HEAD       
      inflating: drop-in/.git/config     
       creating: drop-in/.git/objects/
       creating: drop-in/.git/objects/pack/
       creating: drop-in/.git/objects/info/
       creating: drop-in/.git/objects/43/
     extracting: drop-in/.git/objects/43/246218ab4fc7b30e9a9dff073e012316851469  
       creating: drop-in/.git/objects/25/
     extracting: drop-in/.git/objects/25/16effb8d70e33bdd0023629b164a77225e1ec2  
       creating: drop-in/.git/objects/89/
     extracting: drop-in/.git/objects/89/d296ef533525a1378529be66b22d6a2c01e530  
      inflating: drop-in/.git/index      
     extracting: drop-in/.git/COMMIT_EDITMSG  
       creating: drop-in/.git/logs/
      inflating: drop-in/.git/logs/HEAD  
       creating: drop-in/.git/logs/refs/
       creating: drop-in/.git/logs/refs/heads/
      inflating: drop-in/.git/logs/refs/heads/master  

Listing the contents of the unzipped folder and displaying the contents of the single `message.txt` file :

    $ ls
    message.txt
    
    $ cat message.txt 
    This is what I was working on, but I'd need to look at my commit history to know why...

## Solution ##

As the contents of `message.txt` suggests, lets look at the commit history :

    $ git log
    commit 89d296ef533525a1378529be66b22d6a2c01e530 (HEAD -> master)
    Author: picoCTF <ops@picoctf.com>
    Date:   Tue Mar 12 00:07:22 2024 +0000
    
        picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.
