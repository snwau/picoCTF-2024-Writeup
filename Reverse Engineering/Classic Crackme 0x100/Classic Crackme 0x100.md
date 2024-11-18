# Classic Crackme 0x100 #
 
## Overview ##

300 points

Category: [Reverse Engineering](../)

Tags: `#reverseengineering #bruteforce #hash`

## Description ##

A classic Crackme. Find the password, get the flag!
Binary can be downloaded here.
Crack the Binary file locally and recover the password. Use the same password on the server to get the flag!

## Approach ##

Disassembling the `crackme100` challenge binary in [Ghidra](https://ghidra-sre.org) and analysing `main()`.

    undefined8 main(void)
    {
      int iVar1;
      size_t sVar2;
      char local_a8 [64];
      undefined8 local_68;
      undefined8 local_60;
      undefined8 local_58;
      undefined8 local_50;
      undefined8 local_48;
      undefined7 local_40;
      undefined4 uStack_39;
      uint local_2c;
      uint local_28;
      char local_21;
      uint local_20;
      uint local_1c;
      uint local_18;
      int local_14;
      int local_10;
      int local_c;
      
      local_68 = 0x676d76727970786c;
      local_60 = 0x7672657270697564;
      local_58 = 0x727166766b716f6d;
      local_50 = 0x6575717670716c62;
      local_48 = 0x796771706d7a7565;
      local_40 = 0x73687478726963;
      uStack_39 = 0x77616a;
      setvbuf(stdout,(char *)0x0,2,0);
      printf("Enter the secret password: ");
      __isoc99_scanf(&DAT_00402024,local_a8);
      local_c = 0;
      sVar2 = strlen((char *)&local_68);
      local_14 = (int)sVar2;
      local_18 = 0x55;
      local_1c = 0x33;
      local_20 = 0xf;
      local_21 = 'a';
      for (; local_c < 3; local_c = local_c + 1) {
        for (local_10 = 0; local_10 < local_14; local_10 = local_10 + 1) {
          local_28 = (local_10 % 0xff >> 1 & local_18) + (local_10 % 0xff & local_18);
          local_2c = ((int)local_28 >> 2 & local_1c) + (local_1c & local_28);
          iVar1 = ((int)local_2c >> 4 & local_20) +
                  ((int)local_a8[local_10] - (int)local_21) + (local_20 & local_2c);
          local_a8[local_10] = local_21 + (char)iVar1 + (char)(iVar1 / 0x1a) * -0x1a;
        }
      }
      iVar1 = memcmp(local_a8,&local_68,(long)local_14);
      if (iVar1 == 0) {
        printf("SUCCESS! Here is your flag: %s\n","picoCTF{sample_flag}");
      }
      else {
        puts("FAILED!");
      }
      return 0;
    }

We can see that the challenge binary performs the follow;
- requests an input string from the user (the password) via `scanf()`
- performs some form of hashing of this input transforming the string read into `local_a8`
- compares the calculated hash via `memcmp()` with data stored in `local_68`, using the length of the input string for the comparison length.

The [Ghidra](https://ghidra-sre.org) disassembly has the target hash stored in `local_68` represented a series of words, but this is better represented in a string buffer. This translation could be performed manually, or we can fire up `gdb` and set a breakpoint on the call to `memcmp()` at address `0x40136a` to dump the data pointed to by register `RCX` (or `RSI`) (from the disassembly).

    00401353 8b 45 f4        MOV        EAX,dword ptr [RBP + local_14]
    00401356 48 63 d0        MOVSXD     RDX,EAX
    00401359 48 8d 4d a0     LEA        RCX=>local_68,[RBP + -0x60]
    0040135d 48 8d 85        LEA        RAX=>local_a8,[RBP + -0xa0]
             60 ff ff ff
    00401364 48 89 ce        MOV        RSI,RCX
    00401367 48 89 c7        MOV        RDI,RAX
    0040136a e8 f1 fc        CALL       <EXTERNAL>::memcmp
             ff ff

Debugging in `gdb` we have :

    $ gbd ./crackme100
    gef➤  br *0x40136a
    Breakpoint 1 at 0x40136a: file main_sample.c, line 32.
    gef➤  r
    Starting program: /home/scottw/picoCTF/Classic-Crackme-0x100/crackme100 
    [*] Failed to find objfile or not a valid file format: [Errno 2] No such file or directory: 'system-supplied DSO at 0x7ffff7fc1000'
    [Thread debugging using libthread_db enabled]
    Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
    Enter the secret password: 1234
    
    Breakpoint 1, 0x000000000040136a in main () at main_sample.c:32
    32  main_sample.c: No such file or directory.
    
    ───────────────────────────────────────────────────────────────── registers ────
    $rax   : 0x007fffffffdef0  →  "KOPTQTTWQTTWTWWZQTTWTWWZNQUQVqZ]^TTWTWWZUWWZWZZ]UW"
    $rbx   : 0x0               
    $rcx   : 0x007fffffffdf30  →  "lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw"
    $rdx   : 0x32 

With the way the hashing algorithm iterates over the input string, working on each character from the start of the string to the end, repeating this over 3 iterations, I decided to brute force the password by iteratively solving each character until I had found the full password.

The quickest way to achieve this was to take the disassembled `main()` from the challenge binary, and use directly from our own `main()` with loops to generate and progressively build up the input password until the hash matches.

    #include <stdio.h>
    #include <string.h>
    #include <stdlib.h>

    char input[64];
    char hash[64];
    char password[64];
    char solved[64];

    // The disassembled main() from the crackme100 binary, modified to take
    // an input string rather than read from standard I/O
    int main_orig(char *input)
    {
      int iVar1;
      size_t sVar2;
      char user_input[64];
      // unsigned long long local_68;
      // unsigned long long local_60;
      // unsigned long long local_58;
      // unsigned long long local_50;
      // unsigned long long local_48;
      // unsigned long long local_40;
      // unsigned long long uStack_39;
      unsigned int local_2c;
      unsigned int local_28;
      char local_21;
      unsigned int local_20;
      unsigned int local_1c;
      unsigned int local_18;
      int local_14;
      int local_10;
      int local_c;
      
    #if 0
      // Original target hash during picoCTF challenge
      /*
      local_68 = 0x6a68706e6e6b706d;
      local_60 = 0x64797a676862676e;
      local_58 = 0x707068616b767474;
      local_50 = 0x6777706d6b687665;
      local_48 = 0x6f6b6b7973787a64;
      local_40 = 0x6e66706569726b;
      uStack_39 = 0x6d6472;
      */
      char local_68[] = "mpknnphjngbhgzydttvkahppevhkmpwgdzxsykkokriepfnrdm";
    #else
      // Revised target hash from challenge binary after coming back to 
      // complete the write up
      /*
      local_68 = 0x676d76727970786c;
      local_60 = 0x7672657270697564;
      local_58 = 0x727166766b716f6d;
      local_50 = 0x6575717670716c62;
      local_48 = 0x796771706d7a7565;
      local_40 = 0x73687478726963;
      uStack_39 = 0x77616a;
      */
      char local_68[] = "lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw";
    #endif  

      setvbuf(stdout,(char *)0x0,2,0);
      //printf("Enter the secret password: ");
      //scanf("%s",user_input);
      strcpy(user_input, input);  
      local_c = 0;
      sVar2 = strlen((char *)&local_68);
      local_14 = (int)sVar2;
      local_18 = 0x55;
      local_1c = 0x33;
      local_20 = 0xf;
      local_21 = 'a';
      for (; local_c < 3; local_c = local_c + 1) 
      {
        for (local_10 = 0; local_10 < local_14; local_10 = local_10 + 1) 
        {
          local_28 = (local_10 % 0xff >> 1 & local_18) + (local_10 % 0xff & local_18);
          local_2c = ((int)local_28 >> 2 & local_1c) + (local_1c & local_28);
          iVar1 = ((int)local_2c >> 4 & local_20) +
                  ((int)user_input[local_10] - (int)local_21) + (local_20 & local_2c);
          user_input[local_10] = local_21 + (char)iVar1 + (char)(iVar1 / 0x1a) * -0x1a;
        }
      }
      // We don't need the generated vs target hash comparison logic from the
      // original main()
      /*
      printf("in: %s\nout: %s\n", user_input, (char *)&local_68);
      iVar1 = memcmp(user_input,&local_68,(long)local_14);
      if (iVar1 == 0) {
        printf("SUCCESS! Here is your flag: %s\n","picoCTF{sample_flag}");
      }
      else {
        puts("FAILED!");
      }
      */

      // copy the generated hash from `user_input` buffer to our global `hash` buffer
      memcpy(hash, user_input, sizeof(user_input));
      // first time through, store the target hash within the `password` buffer
      // so we can compare how much of the target hash we have discovered
      if (strlen(password) == 0)
      {
        memcpy(password, (char *)&local_68, sizeof(password));
      }

      return 0;
    }

    int main(void)
    {
      password[0] = '\0';
      solved[0] = '\0';

      for (int i = 0; i < 50; ++i)
      {
        // copy the solved portion of the password to our input buffer
        strcpy(input, solved);
        // iterate through the printable ASCII character range for the next
        // character in the password, appending to the solved portion
        for (unsigned char cur_ch = 0x20; cur_ch < 0x7F; ++cur_ch)
        {
          input[i] = cur_ch;
          input[i+1] = '\0';
         
          hash[0] = '\0';

          // invoke the original challenge binary `main()` to generate the 
          // hash for the current input
          main_orig(input);
          // If the character at the current character index being solved is
          // equal to the target hash, we have solved another character
          if (hash[i] == password[i])
          {
            solved[i] = cur_ch;
            solved[i+1] = '\0';

            printf("solved: %s {hash:%s password:%s}\n", solved, hash, password);
      
            // have we solved the entire hash?
            if (memcmp(hash, password, strlen(password)) == 0)
            {
              printf("********************SOLVED******************\n");
              // write out the solved password to file for easy input with the
              // challenge binary and server
              FILE *fh = fopen ("solved_input.bin", "wb");
              if (fh != NULL) 
              {
                fwrite (solved, strlen(solved), 1, fh);
                fclose (fh);
              }
              exit(0);
            }
            break;
          }
        }
      }

      return 0;
    }

Running this yields the following output :

    # ./crack
    solved: l {hash:lQebebbyfbbybyyvqfmusryvjkvsqgvsoibybyyvadvcusvsbwo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lu {hash:lxQxcxxsdxxsxssnobioolsnfepkkynimexsxssnwxpuoknixqo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lum {hash:lxpTcxxsdxxsxssnobioolsnfepkkynimexsxssnwxpuoknixqo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lums {hash:lxpyQhhuvhhuhuuhglsqynuhpgremshueohuhuuhgzroqehuhso� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumso {hash:lxpyrTtmbttmtmmfmxeikfmfbyjceqfykatmtmmfsrjmicfytko� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsop {hash:lxpyrvTnkllnlnnpvpwjcgnptzkmfaprtslnlnnpkskwjmprllo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg {hash:lxpyrvmWsbblbllvdfmhselvjxisdgvfbiblbllvaqichsvfbjo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg* {hash:lxpyrvmgQppgpggxktacgzgxxsduyixoiwpgpggxoldecuxopeo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*a {hash:lxpyrvmgdTvcvccjazgymvcjdozguujqycvcvccjuhzqygjqvao� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*ao {hash:lxpyrvmgduTuhuuhglsqynuhpgremshueohuhuuhgzroqehuhso� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aoc {hash:lxpyrvmgduiWziircdkeqbirhufoacraagziziirynfyeorazgo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocg {hash:lxpyrvmgduipTggxktacgzgxxsduyixoiwpgpggxoldecuxopeo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl {hash:lxpyrvmgduiprWxlhnutaqxlrjuipwlzfqjxjxxlicustilzjvo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl( {hash:lxpyrvmgduipreWpipwwctaptmxmsapegslalaapkfxwwmpelyo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(i {hash:lxpyrvmgduiprerZwrymejqtvcnqietwuunqnqqtmvnamqtwnoo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ij {hash:lxpyrvmgduiprervQzglmipjdbmghujdlcvpvppjuumqlgjdvno� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijj {hash:lxpyrvmgduiprervmTokuhozlalwgkzkckdodoozctlgkwzkdmo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijji {hash:lxpyrvmgduiprervmoTawxednqbawodcqmfefeedejbkaadcfco� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjik {hash:lxpyrvmgduiprervmoqWkfmfbyjceqfykatmtmmfsrjmicfytko� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikb {hash:lxpyrvmgduiprervmoqkTkrdndoajodpdmfrfrrdewoknadpfpo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp {hash:lxpyrvmgduiprervmoqkvWsnfepkkynimexsxssnwxpuoknixqo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp) {hash:lxpyrvmgduiprervmoqkvfWdnqbawodcqmfefeedejbkaadcfco� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)h {hash:lxpyrvmgduiprervmoqkvfqZjxisdgvfbiblbllvaqichsvfbjo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf {hash:lxpyrvmgduiprervmoqkvfqrTmxmsapegslalaapkfxwwmpelyo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf( {hash:lxpyrvmgduiprervmoqkvfqrbWaqvetjhundnddtmiaazqtjnbo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(c {hash:lxpyrvmgduiprervmoqkvfqrblWajodpdmfrfrrdewoknadpfpo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(ch {hash:lxpyrvmgduiprervmoqkvfqrblqZqgvsoibybyyvadvcusvsbwo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chd {hash:lxpyrvmgduiprervmoqkvfqrblqpWcrnngzvzvvryasyrornzto� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdm {hash:lxpyrvmgduiprervmoqkvfqrblqpvZrnngzvzvvryasyrornzto� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdme {hash:lxpyrvmgduiprervmoqkvfqrblqpvqZxpkdbdbbzcgygxwzxdzo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei {hash:lxpyrvmgduiprervmoqkvfqrblqpvqu]dmfrfrrdewoknadpfpo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei" {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueQexsxssnwxpuoknixqo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"b {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeTjxjxxlicustilzjvo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"bo {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuTpvppjuumqlgjdvno� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"bot {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzWlaapkfxwwmpelyo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botd {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmTxxlicustilzjvo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdj {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpWwbqbtisybgruo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqWtmiaazqtjnbo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh* {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgZadvcusvsbwo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgyTdvcusvsbwo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m) {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycWhsgilmjio� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)` {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgyciWstilzjvo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`i {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirZjmprllo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`il {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxWadcfco� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`ilk {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxtZfytko� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`ilk( {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthZdvno� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`ilk(g {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxths]llo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`ilk(g' {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjTfo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`ilk(g'' {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaWo� password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    solved: lumsopg*aocgl(ijjikbp)hf(chdmei"botdjh*m)`ilk(g''n {hash:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw password:lxpyrvmgduiprervmoqkvfqrblqpvqueeuzmpqgycirxthsjaw}
    ********************SOLVED******************

Now we can test the solved password with the challenge binary to verify :

    $ ./crackme100 < solved_input.bin 
    Enter the secret password: SUCCESS! Here is your flag: picoCTF{sample_flag}

## Solution ##

Repeating the above solved input on the challenge server to get our flag :

    $ cat solved_input.bin | nc titan.picoctf.net 63110
    Enter the secret password: SUCCESS! Here is your flag: picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.

## Notes ##

Note that when I completed this challenge during the CTF event the target hash was found to be `"mpknnphjngbhgzydttvkahppevhkmpwgdzxsykkokriepfnrdm"`. When preparing this write-up I found the target hash had changed and the following warning accompanied the challenge information :

    "Binaries downloaded prior to March 13th, 2024 may not match the intended solution. Please redownload the updated binary if needed."
