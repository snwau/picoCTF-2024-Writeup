# Custom encryption #
 
## Overview ##

100 points

Category: [Cryptography](../)

Tags: `#cryptography`

## Description ##

Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here flag_info and code file might be good to analyze and get the flag.

## Approach ##

Inspecting the provided `enc_flag` encrypted flag file we see what appears to be two parameters or coefficients (`a` and `b`), as well as an array of integers (the cipher text).  

    $ cat enc_flag 
    a = 89
    b = 27
    cipher is: [33588, 276168, 261240, 302292, 343344, 328416, 242580, 85836, 82104, 156744, 0, 309756, 78372, 18660, 253776, 0, 82104, 320952, 3732, 231384, 89568, 100764, 22392, 22392, 63444, 22392, 97032, 190332, 119424, 182868, 97032, 26124, 44784, 63444]

Analysing the provided encryption source code `custom_encryption.py` to understand the encryption algorithm.

    from random import randint
    import sys


    def generator(g, x, p):
        return pow(g, x) % p

    def encrypt(plaintext, key):
        cipher = []
        for char in plaintext:
            cipher.append(((ord(char) * key*311)))
        return cipher

    def is_prime(p):
        v = 0
        for i in range(2, p + 1):
            if p % i == 0:
                v = v + 1
        if v > 1:
            return False
        else:
            return True

    def dynamic_xor_encrypt(plaintext, text_key):
        cipher_text = ""
        key_length = len(text_key)
        for i, char in enumerate(plaintext[::-1]):
            key_char = text_key[i % key_length]
            encrypted_char = chr(ord(char) ^ ord(key_char))
            cipher_text += encrypted_char
        return cipher_text

    def test(plain_text, text_key):
        p = 97
        g = 31
        if not is_prime(p) and not is_prime(g):
            print("Enter prime numbers")
            return
        a = randint(p-10, p)
        b = randint(g-10, g)
        print(f"a = {a}")
        print(f"b = {b}")
        u = generator(g, a, p)
        v = generator(g, b, p)
        key = generator(v, a, p)
        b_key = generator(u, b, p)
        shared_key = None
        if key == b_key:
            shared_key = key
        else:
            print("Invalid key")
            return
        semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
        cipher = encrypt(semi_cipher, shared_key)
        print(f'cipher is: {cipher}')

    if __name__ == "__main__":
        message = sys.argv[1]
        test(message, "trudeau")

We see argument one to the python script `message` is the plain text to encrypt which is passed to `test()` function that performs the encryption with a static key `"trudeau"`.

`test()` defines two more coefficients `p` and `q` with fixed values, that must be prime numbers. The `a` and `b` values we saw from the encrypted flag file we can see are generated randomly, hence why they were persisted in the encrypted flag file.

The `generator()` function is used to generate a number of values and keys, but ultimately a `shared_key` that is used by the `encrypt()` function.

Before this `shared_key` and the `encrypt()` function are used, the plain text firstly goes through an `XOR` based encryption via the `dynamic_xor_encrypt()` function. 'XOR' being reversible, we can reuse this function in our `custom_decrypt.py` code directly to decrypt this stage, having the cipher text and knowing the static `text_key` (`"trudeau"`) used in the `XOR` encryption.

`encrypt()` converts each character of the "semi" cipher string to the number representing the unicode code of the character via the `ord()` function, multiplying this by the `shared_key` and a fixed value of `311`.

    t = ((ord(char) * key*311)) 

To be able to decrypt the cipher text encrypted by the second stage `encrypt()` function we must generate the `shared_key`, which we can do as we know `p` and `q` are fixed values, and `a` and `b` are stored in the encrypted flag file.

To calculate the `shared_key` we reuse what is done from `custom_encryption.py`, generating `v` then `key` which is our returned `shared_key`.

    def calculate_key(a, b, g, p):
      v = pow(g,b) % p
      return (pow(v,a) % p)

Now we can create a `decrypt()` function that operates in an identical fashion to the original `encrypt()`, except divides the cipher text value by the `shared_key` reversing the multiplication from `encrypt()`. This will decrypt our encrypted flag file cipher text to the "semi" cipher text to be further decrypted as previously mentioned by the `XOR` stage, leaving us then with the decrypted plain text.

    def decrypt(cipher_text, key):
      semi_cipher_text = []
      for char in cipher_text:
        decrypted_char = chr(int(int(char) / 311 / key))
        semi_cipher_text.append(decrypted_char)
      return semi_cipher_text

After reversing the multiplication we are left with the plain text character but the unicode number representation, hence we can convert this to a character via `chr()`.

## Solution ##

Bringing all of this together into an accompanying `custom_decryption.py` source file, we have:

    #!/usr/bin/env python3

    import sys
    import re

    # parse the 'a' or 'b' coefficient from the provided line of text, 
    # read from the encrypted flag file, line of text
    def parse_coeff(coeff, string):
      m = re.search(r'(\w) = (\d+)$', string)
      if m is not None:
        if m.group(1) == coeff:
          return int(m.group(2))
      return None

    # calculate the `shared_key` to use in decrypt()
    def calculate_key(a, b, g, p):
      v = pow(g,b) % p
      return (pow(v,a) % p)

    # extract the cipher text as a list of numbers for later conversion
    def extract_cipher_text(string):
      m = re.search(r'cipher is: \[([\d\s,]+)\]', string)
      char_list = m.group(1).split(', ')
      return char_list

    # reverse the encryption and convert back from unicode character number
    # representation to characters
    def decrypt(cipher_text, key):
      semi_cipher_text = []
      for char in cipher_text:
        decrypted_char = chr(int(int(char) / 311 / key))
        semi_cipher_text.append(decrypted_char)
      return semi_cipher_text

    def xor_decrypt(semi_cipher, text_key):
      plain_text = ""
      key_length = len(text_key)
      for i, char in enumerate(semi_cipher):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
      return plain_text[::-1]

    if __name__ == "__main__":
      # an argument must be provided that specifies the encrypted file to decrypt
      if (len(sys.argv) < 2):
        print("usage: custom_decryption <encrypted_file>")
        exit(1)

    # define the fixed coefficients (from custom_encryption.py)
    p = 97
    g = 31

    # open the encrypted file and parse the `a` and `b` coefficients
    with open(sys.argv[1], "r") as enc_f:
      a = parse_coeff("a", enc_f.readline())
      b = parse_coeff("b", enc_f.readline())

      # generate the `shared_key` using the read `a` and `b` coefficients
      key = calculate_key(a, b, g, p)

      # extract the cipher text as a list of numbers
      cipher_text = extract_cipher_text(enc_f.readline())

      # perform the two stage decryption
      semi_cipher_text = decrypt(cipher_text, key)
      plain_text = xor_decrypt(semi_cipher_text, "trudeau")

      print(plain_text)

Testing

    $ python3 custom_encryption.py "Plain as plain can be." > test_enc

    $ cat test_enc
    a = 87
    b = 29
    cipher is: [2379150, 608005, 608005, 1797580, 290785, 0, 581570, 2220540, 740180, 740180, 132175, 237915, 449395, 2246975, 185045, 502265, 2246975, 264350, 317220, 0, 660875, 951660]

    $ python3 custom_decryption.py test_enc
    Plain as plain can be.

Confirmed the custom decryptor is working with known data, so lets decrypt the provided challenge flag file:

    $ python3 custom_decryption.py enc_flag
    picoCTF{...........redacted.............}

Where the actual flag value has been redacted for the purposes of this write up.
