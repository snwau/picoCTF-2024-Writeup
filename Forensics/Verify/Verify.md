# Verify #
 
## Overview ##

50 points

Category: [Forensics](../)

Tags: `#forensics`

## Description ##

People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.
You can download the challenge files here:
`challenge.zip`

## Approach ##

Unzipping the provided `challenge.zip`, a lot of files are extracted with seeminly random filename, but also interestingly there is a `decrypt.sh` shell script.

    $ unzip challenge.zip 
    Archive:  challenge.zip
       creating: home/ctf-player/drop-in/
       creating: home/ctf-player/drop-in/files/
     extracting: home/ctf-player/drop-in/files/cAehfago  
     extracting: home/ctf-player/drop-in/files/87590c24  
     extracting: home/ctf-player/drop-in/files/YoOZ5qVn  
     extracting: home/ctf-player/drop-in/files/7ZEsnWZx  
     extracting: home/ctf-player/drop-in/files/rse22gSC  
     extracting: home/ctf-player/drop-in/files/bJquvh3E  
     extracting: home/ctf-player/drop-in/files/EtkxnTjE  
     extracting: home/ctf-player/drop-in/files/YmQKA23p  
     extracting: home/ctf-player/drop-in/files/bSkjXxrA  
     extracting: home/ctf-player/drop-in/files/uF2fheP1  
     extracting: home/ctf-player/drop-in/files/raIgZC8T  
     extracting: home/ctf-player/drop-in/files/HVhGYkYw  
     extracting: home/ctf-player/drop-in/files/JHLXVG98  
     extracting: home/ctf-player/drop-in/files/KaGA7sxA  
     extracting: home/ctf-player/drop-in/files/ZS7jkTUt  
     extracting: home/ctf-player/drop-in/files/hIbQdK4d  
     extracting: home/ctf-player/drop-in/files/iyKEK5tY  
     extracting: home/ctf-player/drop-in/files/fzjMwVlq  
     extracting: home/ctf-player/drop-in/files/jGGKUWQ9  
     extracting: home/ctf-player/drop-in/files/CfZp4f2N  
     extracting: home/ctf-player/drop-in/files/j21jebUD  
     extracting: home/ctf-player/drop-in/files/oX1HkX8C  
     extracting: home/ctf-player/drop-in/files/CLcdCN8f  
     extracting: home/ctf-player/drop-in/files/GTH1LeLw  
     extracting: home/ctf-player/drop-in/files/7S05biBB  
     extracting: home/ctf-player/drop-in/files/7dnW8EpS  
     extracting: home/ctf-player/drop-in/files/ia64o1VA  
     extracting: home/ctf-player/drop-in/files/7x5Qa3WK  
     extracting: home/ctf-player/drop-in/files/3FS3M7tr  
     extracting: home/ctf-player/drop-in/files/QUesl6sJ  
     extracting: home/ctf-player/drop-in/files/p0yUpsYe  
     extracting: home/ctf-player/drop-in/files/ZTxbKkJG  
     extracting: home/ctf-player/drop-in/files/3XFTF7vi  
     extracting: home/ctf-player/drop-in/files/Tz1xgD5A  
     extracting: home/ctf-player/drop-in/files/3niXrkT4  
     extracting: home/ctf-player/drop-in/files/jg2ztGlp  
     extracting: home/ctf-player/drop-in/files/vy3RDHxy  
     extracting: home/ctf-player/drop-in/files/t3cJ6Gfp  
     extracting: home/ctf-player/drop-in/files/RGc1b7Dz  
     extracting: home/ctf-player/drop-in/files/vMdtOisa  
     extracting: home/ctf-player/drop-in/files/b9BDTW02  
     extracting: home/ctf-player/drop-in/files/I7ySGmF3  
     extracting: home/ctf-player/drop-in/files/AHwWjpbL  
     extracting: home/ctf-player/drop-in/files/3KQWpDpt  
     extracting: home/ctf-player/drop-in/files/GGpU931s  
     extracting: home/ctf-player/drop-in/files/cbKOHVgV  
     extracting: home/ctf-player/drop-in/files/bOxuzUXf  
     extracting: home/ctf-player/drop-in/files/TJect2dg  
     extracting: home/ctf-player/drop-in/files/0QBK0M73  
     extracting: home/ctf-player/drop-in/files/2ypuxhr6  
     extracting: home/ctf-player/drop-in/files/uHK0HPxg  
     extracting: home/ctf-player/drop-in/files/yCXEcn1e  
     extracting: home/ctf-player/drop-in/files/XmYppXhk  
     extracting: home/ctf-player/drop-in/files/9E2Ci7r7  
     extracting: home/ctf-player/drop-in/files/ybQ7iIWc  
     extracting: home/ctf-player/drop-in/files/g33i1myO  
     extracting: home/ctf-player/drop-in/files/OvH6KubA  
     extracting: home/ctf-player/drop-in/files/caVHWehf  
     extracting: home/ctf-player/drop-in/files/02kLdPvr  
     extracting: home/ctf-player/drop-in/files/PucU9ljB  
     extracting: home/ctf-player/drop-in/files/AXwbPGqz  
     extracting: home/ctf-player/drop-in/files/KRxkl4ki  
     extracting: home/ctf-player/drop-in/files/Bb0A4ZUF  
     extracting: home/ctf-player/drop-in/files/eqMPkSW0  
     extracting: home/ctf-player/drop-in/files/4RWg3C81  
     extracting: home/ctf-player/drop-in/files/RD85xyJi  
     extracting: home/ctf-player/drop-in/files/2SLddvKm  
     extracting: home/ctf-player/drop-in/files/aAv6C6d8  
     extracting: home/ctf-player/drop-in/files/ZS6vNpit  
     extracting: home/ctf-player/drop-in/files/PyzlgxBl  
     extracting: home/ctf-player/drop-in/files/niAMHayw  
     extracting: home/ctf-player/drop-in/files/XKypMMiJ  
     extracting: home/ctf-player/drop-in/files/0pYtGDyN  
     extracting: home/ctf-player/drop-in/files/aCxWJytK  
     extracting: home/ctf-player/drop-in/files/PbCHfm8l  
     extracting: home/ctf-player/drop-in/files/fFbQAlD7  
     extracting: home/ctf-player/drop-in/files/AfqV5hAX  
     extracting: home/ctf-player/drop-in/files/MCFq06DD  
     extracting: home/ctf-player/drop-in/files/q2mcPs3c  
     extracting: home/ctf-player/drop-in/files/HUGRY61E  
     extracting: home/ctf-player/drop-in/files/SOpZbONn  
     extracting: home/ctf-player/drop-in/files/zfFqIJrW  
     extracting: home/ctf-player/drop-in/files/7qVcsxIn  
     extracting: home/ctf-player/drop-in/files/z8QLAIjY  
     extracting: home/ctf-player/drop-in/files/OHWOluJj  
     extracting: home/ctf-player/drop-in/files/7SXF5dDA  
     extracting: home/ctf-player/drop-in/files/sHWDCVFR  
     extracting: home/ctf-player/drop-in/files/uiBoxr24  
     extracting: home/ctf-player/drop-in/files/OFSlnCKK  
     extracting: home/ctf-player/drop-in/files/0AEMwSHP  
     extracting: home/ctf-player/drop-in/files/pGy7EVYy  
     extracting: home/ctf-player/drop-in/files/inbE9V4Z  
     extracting: home/ctf-player/drop-in/files/vZ9Qc5Cg  
     extracting: home/ctf-player/drop-in/files/IihMQr2S  
     extracting: home/ctf-player/drop-in/files/x0THPKVQ  
     extracting: home/ctf-player/drop-in/files/srli5tAs  
     extracting: home/ctf-player/drop-in/files/t1UlIMzc  
     extracting: home/ctf-player/drop-in/files/Mrl2cMHD  
     extracting: home/ctf-player/drop-in/files/WY59WpIg  
     extracting: home/ctf-player/drop-in/files/0ReLqo7l  
     extracting: home/ctf-player/drop-in/files/4E42FQrx  
     extracting: home/ctf-player/drop-in/files/WjjIvWrX  
     extracting: home/ctf-player/drop-in/files/Ms5Mvz12  
     extracting: home/ctf-player/drop-in/files/TIC1KArZ  
     extracting: home/ctf-player/drop-in/files/d5EQVHHc  
     extracting: home/ctf-player/drop-in/files/3llQZxg9  
     extracting: home/ctf-player/drop-in/files/W2hZvcID  
     extracting: home/ctf-player/drop-in/files/6JGob933  
     extracting: home/ctf-player/drop-in/files/ANd9SGuS  
     extracting: home/ctf-player/drop-in/files/WpLyF4xC  
     extracting: home/ctf-player/drop-in/files/TAh3haNF  
     extracting: home/ctf-player/drop-in/files/5guDO1cS  
     extracting: home/ctf-player/drop-in/files/i9nzJ3r6  
     extracting: home/ctf-player/drop-in/files/miUtTQRm  
     extracting: home/ctf-player/drop-in/files/Ds4StPpk  
     extracting: home/ctf-player/drop-in/files/YrKlF95C  
     extracting: home/ctf-player/drop-in/files/WOHDiZ4q  
     extracting: home/ctf-player/drop-in/files/7enMn1rk  
     extracting: home/ctf-player/drop-in/files/1iUcIRwR  
     extracting: home/ctf-player/drop-in/files/iBekBdEx  
     extracting: home/ctf-player/drop-in/files/a2OU257J  
     extracting: home/ctf-player/drop-in/files/jFn9chPx  
     extracting: home/ctf-player/drop-in/files/34asQJAj  
     extracting: home/ctf-player/drop-in/files/Gyn3Qful  
     extracting: home/ctf-player/drop-in/files/xPX9SADx  
     extracting: home/ctf-player/drop-in/files/QU8jejZq  
     extracting: home/ctf-player/drop-in/files/JZuYuOle  
     extracting: home/ctf-player/drop-in/files/a5BwxEPT  
     extracting: home/ctf-player/drop-in/files/PisLwk6C  
     extracting: home/ctf-player/drop-in/files/DQlu1oVQ  
     extracting: home/ctf-player/drop-in/files/MoKJFjOi  
     extracting: home/ctf-player/drop-in/files/DyULYdws  
     extracting: home/ctf-player/drop-in/files/Y9apvJeb  
     extracting: home/ctf-player/drop-in/files/sbuTFkuC  
     extracting: home/ctf-player/drop-in/files/3HWGffFE  
     extracting: home/ctf-player/drop-in/files/dzxHhn9S  
     extracting: home/ctf-player/drop-in/files/oNq64S7j  
     extracting: home/ctf-player/drop-in/files/7M36N0ZG  
     extracting: home/ctf-player/drop-in/files/5aE4Zjfg  
     extracting: home/ctf-player/drop-in/files/opYpNJ2C  
     extracting: home/ctf-player/drop-in/files/4lpXJyrZ  
     extracting: home/ctf-player/drop-in/files/y2TwAQsl  
     extracting: home/ctf-player/drop-in/files/vJ3qPWEu  
     extracting: home/ctf-player/drop-in/files/tqcK3Nrs  
     extracting: home/ctf-player/drop-in/files/YXeLAbOj  
     extracting: home/ctf-player/drop-in/files/lY9YUKSk  
     extracting: home/ctf-player/drop-in/files/NTsSRBdP  
     extracting: home/ctf-player/drop-in/files/O5vpRSXk  
     extracting: home/ctf-player/drop-in/files/Ae01ONEX  
     extracting: home/ctf-player/drop-in/files/JPzzPv2R  
     extracting: home/ctf-player/drop-in/files/jS1LPICH  
     extracting: home/ctf-player/drop-in/files/scPPxxi7  
     extracting: home/ctf-player/drop-in/files/7GdIoG6L  
     extracting: home/ctf-player/drop-in/files/mAZwT81W  
     extracting: home/ctf-player/drop-in/files/3UI1Q3Im  
     extracting: home/ctf-player/drop-in/files/5oxXh5Er  
     extracting: home/ctf-player/drop-in/files/v7gF2ZIR  
     extracting: home/ctf-player/drop-in/files/cBdFMzZa  
     extracting: home/ctf-player/drop-in/files/wR9DoLPS  
     extracting: home/ctf-player/drop-in/files/fM4Y8XlN  
     extracting: home/ctf-player/drop-in/files/cAgx0hQ8  
     extracting: home/ctf-player/drop-in/files/7e0ugnNi  
     extracting: home/ctf-player/drop-in/files/dmc1Epts  
     extracting: home/ctf-player/drop-in/files/zKjekGtU  
     extracting: home/ctf-player/drop-in/files/lty8luMw  
     extracting: home/ctf-player/drop-in/files/PK3nRKAh  
     extracting: home/ctf-player/drop-in/files/WZcRSCOi  
     extracting: home/ctf-player/drop-in/files/k1uy8MT2  
     extracting: home/ctf-player/drop-in/files/pbXSC543  
     extracting: home/ctf-player/drop-in/files/eyrV73UR  
     extracting: home/ctf-player/drop-in/files/XZocCx02  
     extracting: home/ctf-player/drop-in/files/Kopd5uAS  
     extracting: home/ctf-player/drop-in/files/vlXYHLR4  
     extracting: home/ctf-player/drop-in/files/hueXtQsv  
     extracting: home/ctf-player/drop-in/files/S7zBjDOu  
     extracting: home/ctf-player/drop-in/files/lpgH0BeK  
     extracting: home/ctf-player/drop-in/files/mL2zl8ME  
     extracting: home/ctf-player/drop-in/files/Kqq1CiRK  
     extracting: home/ctf-player/drop-in/files/jQrw8bQO  
     extracting: home/ctf-player/drop-in/files/DUx15XmG  
     extracting: home/ctf-player/drop-in/files/WLCnFHkD  
     extracting: home/ctf-player/drop-in/files/1WzFkIC5  
     extracting: home/ctf-player/drop-in/files/AuXnIN8I  
     extracting: home/ctf-player/drop-in/files/PVAhaXhI  
     extracting: home/ctf-player/drop-in/files/uVGzmmri  
     extracting: home/ctf-player/drop-in/files/GBMBtUcG  
     extracting: home/ctf-player/drop-in/files/byaxGEzl  
     extracting: home/ctf-player/drop-in/files/nYMkLfqq  
     extracting: home/ctf-player/drop-in/files/ngS5ThOq  
     extracting: home/ctf-player/drop-in/files/c5K8wpvT  
     extracting: home/ctf-player/drop-in/files/aR087rW1  
     extracting: home/ctf-player/drop-in/files/5uH65mOX  
     extracting: home/ctf-player/drop-in/files/K4w4LSpK  
     extracting: home/ctf-player/drop-in/files/6GarVCiO  
     extracting: home/ctf-player/drop-in/files/9LgFu97C  
     extracting: home/ctf-player/drop-in/files/Xkrt40pJ  
     extracting: home/ctf-player/drop-in/files/ze9Qihpg  
     extracting: home/ctf-player/drop-in/files/BVQMsUMN  
     extracting: home/ctf-player/drop-in/files/2y6eiPnY  
     extracting: home/ctf-player/drop-in/files/U6Za1XXb  
     extracting: home/ctf-player/drop-in/files/EwczAgxU  
     extracting: home/ctf-player/drop-in/files/BjOuTdvt  
     extracting: home/ctf-player/drop-in/files/QkF8ZdDf  
     extracting: home/ctf-player/drop-in/files/s1T1hXGi  
     extracting: home/ctf-player/drop-in/files/9twFwHiq  
     extracting: home/ctf-player/drop-in/files/MkEdjeVZ  
     extracting: home/ctf-player/drop-in/files/5L6bCEOe  
     extracting: home/ctf-player/drop-in/files/Hw4z4pGA  
     extracting: home/ctf-player/drop-in/files/ePMDz1YQ  
     extracting: home/ctf-player/drop-in/files/3bwissRA  
     extracting: home/ctf-player/drop-in/files/sTNxbYIX  
     extracting: home/ctf-player/drop-in/files/15s8svF3  
     extracting: home/ctf-player/drop-in/files/vZXXKL7g  
     extracting: home/ctf-player/drop-in/files/P0pMREh2  
     extracting: home/ctf-player/drop-in/files/1QyIAyVK  
     extracting: home/ctf-player/drop-in/files/F7pJKgt3  
     extracting: home/ctf-player/drop-in/files/5VDBcjMu  
     extracting: home/ctf-player/drop-in/files/1BX0cysv  
     extracting: home/ctf-player/drop-in/files/bXmTLFnK  
     extracting: home/ctf-player/drop-in/files/vT94qEUU  
     extracting: home/ctf-player/drop-in/files/FaSiQOUR  
     extracting: home/ctf-player/drop-in/files/NbwekSHV  
     extracting: home/ctf-player/drop-in/files/SXfbixMa  
     extracting: home/ctf-player/drop-in/files/frJix6dy  
     extracting: home/ctf-player/drop-in/files/FA1DL0lH  
     extracting: home/ctf-player/drop-in/files/k3Je1oZv  
     extracting: home/ctf-player/drop-in/files/r0zVMZkQ  
     extracting: home/ctf-player/drop-in/files/tYVdC1Z0  
     extracting: home/ctf-player/drop-in/files/KM1rOvYX  
     extracting: home/ctf-player/drop-in/files/4YmX8P1z  
     extracting: home/ctf-player/drop-in/files/YJZwL09m  
     extracting: home/ctf-player/drop-in/files/SibjLvWM  
     extracting: home/ctf-player/drop-in/files/Giagg0HI  
     extracting: home/ctf-player/drop-in/files/TdeDBXMf  
     extracting: home/ctf-player/drop-in/files/JKlfmBiL  
     extracting: home/ctf-player/drop-in/files/13eD2Klj  
     extracting: home/ctf-player/drop-in/files/RH4yp5nD  
     extracting: home/ctf-player/drop-in/files/lCWOa2m6  
     extracting: home/ctf-player/drop-in/files/4WsDciyV  
     extracting: home/ctf-player/drop-in/files/o4p7FB5l  
     extracting: home/ctf-player/drop-in/files/TSGfXer8  
     extracting: home/ctf-player/drop-in/files/MRW8xKE7  
     extracting: home/ctf-player/drop-in/files/Av9IEmci  
     extracting: home/ctf-player/drop-in/files/OgbJmSsl  
     extracting: home/ctf-player/drop-in/files/qhTEo0LL  
     extracting: home/ctf-player/drop-in/files/7iM83rpT  
     extracting: home/ctf-player/drop-in/files/yRSmctoR  
     extracting: home/ctf-player/drop-in/files/QjfpcKtL  
     extracting: home/ctf-player/drop-in/files/qxtRkCQq  
     extracting: home/ctf-player/drop-in/files/RHX0dhSU  
     extracting: home/ctf-player/drop-in/files/EI5Rua6h  
     extracting: home/ctf-player/drop-in/files/urhbGMnd  
     extracting: home/ctf-player/drop-in/files/wp0CW0xc  
     extracting: home/ctf-player/drop-in/files/Ijh6n9yp  
     extracting: home/ctf-player/drop-in/files/x0ODxbKK  
     extracting: home/ctf-player/drop-in/files/jpTG7wle  
     extracting: home/ctf-player/drop-in/files/SuTmtwHT  
     extracting: home/ctf-player/drop-in/files/l4aPZsIN  
     extracting: home/ctf-player/drop-in/files/fG3pxqYw  
     extracting: home/ctf-player/drop-in/files/gNe3d0tj  
     extracting: home/ctf-player/drop-in/files/wH52POYK  
     extracting: home/ctf-player/drop-in/files/NULNnaKd  
     extracting: home/ctf-player/drop-in/files/3azP1Vr5  
     extracting: home/ctf-player/drop-in/files/BYWVMQ8G  
     extracting: home/ctf-player/drop-in/files/Iqbw9pQz  
     extracting: home/ctf-player/drop-in/files/fANUtdte  
     extracting: home/ctf-player/drop-in/files/CDVB02i6  
     extracting: home/ctf-player/drop-in/files/whsqK6Xi  
     extracting: home/ctf-player/drop-in/files/ziobqxT9  
     extracting: home/ctf-player/drop-in/files/2HLMOyNv  
     extracting: home/ctf-player/drop-in/files/3RwrWxAh  
     extracting: home/ctf-player/drop-in/files/D3FjiyXm  
     extracting: home/ctf-player/drop-in/files/HUqcWryv  
     extracting: home/ctf-player/drop-in/files/DxLqi3sV  
     extracting: home/ctf-player/drop-in/files/Yqx2G9d1  
     extracting: home/ctf-player/drop-in/files/7XpL7kA2  
     extracting: home/ctf-player/drop-in/files/WruPiHQS  
     extracting: home/ctf-player/drop-in/files/8pMqC98d  
     extracting: home/ctf-player/drop-in/files/ZXWIGnQR  
     extracting: home/ctf-player/drop-in/files/O8mpr58x  
     extracting: home/ctf-player/drop-in/files/HkwhoPkp  
     extracting: home/ctf-player/drop-in/files/uFoxxMAD  
     extracting: home/ctf-player/drop-in/files/xa0pHpo3  
     extracting: home/ctf-player/drop-in/files/cxjAxbcB  
     extracting: home/ctf-player/drop-in/files/bJ8uPssG  
     extracting: home/ctf-player/drop-in/files/1kzeT7Bq  
     extracting: home/ctf-player/drop-in/files/SliwhkVP  
     extracting: home/ctf-player/drop-in/files/2EHy0UNu  
     extracting: home/ctf-player/drop-in/files/np9TOAIR  
     extracting: home/ctf-player/drop-in/files/h6P0bHfg  
     extracting: home/ctf-player/drop-in/files/p8McbjLN  
     extracting: home/ctf-player/drop-in/files/oNQJCg54  
     extracting: home/ctf-player/drop-in/files/diVxZXPr  
     extracting: home/ctf-player/drop-in/files/dGbl67gA  
     extracting: home/ctf-player/drop-in/files/sODASTm7  
     extracting: home/ctf-player/drop-in/files/WJFLBATt  
     extracting: home/ctf-player/drop-in/files/Gup9YNYb  
     extracting: home/ctf-player/drop-in/files/1s9fb01L  
     extracting: home/ctf-player/drop-in/files/N36WmSck  
     extracting: home/ctf-player/drop-in/files/1fPO7Wz1  
     extracting: home/ctf-player/drop-in/files/9hbn5BeA  
      inflating: home/ctf-player/drop-in/checksum.txt  
      inflating: home/ctf-player/drop-in/decrypt.sh 

Having a look at the contents of `decrypt.sh` we have:

    $ cat decrypt.sh 

    #!/bin/bash

    # Check if the user provided a file name as an argument
    if [ $# -eq 0 ]; then
        echo "Expected usage: decrypt.sh <filename>"
        exit 1
    fi

    # Store the provided filename in a variable
    file_name="$1"

    # Check if the provided argument is a file and not a folder
    if [ ! -f "/home/ctf-player/drop-in/$file_name" ]; then
        echo "Error: '$file_name' is not a valid file. Look inside the 'files' folder with 'ls -R'!"
        exit 1
    fi

    # If there's an error reading the file, print an error message
    if ! openssl enc -d -aes-256-cbc -pbkdf2 -iter 100000 -salt -in "/home/ctf-player/drop-in/$file_name" -k picoCTF; then
        echo "Error: Failed to decrypt '$file_name'. This flag is fake! Keep looking!"
    fi

I duplicated this script and modified it slightly, replacing the absolute file paths `/home/ctf-player/drop-in/$file_name` with simply `$file_name`, such that the script can be run locally on the current folder or pass an absolute file path as the argument to the script.

I then wrote a new bash script `pwn.sh` to loop through all files in a specfied folder (provided via argument), running the modified descrpt script `decrypt_local.sh` on each file to automate the decrption and hopefully locate the flag. 

    #!/bin/bash

    # check if user provided directory as an argument
    if [ $# -eq 0 ]; then
      echo "Usage: pwn.sh <directory>"
      exit 1
    fi

    # check if the supplied target is a directory
    directory_path="$1"
    if [ ! -d "$directory_path" ]; then
      echo "ERROR: '$directory_path' is not a directory"
      exit 1
    fi

    for file in "$directory_path"/*; do
      if [ -f "$file" ]; then
        echo "Testing file... $file"
        ./decrypt_local.sh $file
      fi
    done

I could have improved the script to interogate the output of the script and stop once the flag was found, but instead let the commandline do the hard work.

## Solution ##

Running the `pwn.sh` script on the challenge `files` folder:

    $ ./pwn.sh files 2>&1| grep picoCTF
    picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.
