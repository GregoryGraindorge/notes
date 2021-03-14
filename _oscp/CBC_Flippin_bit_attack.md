CBC - Flippin bit attack

https://stackhound.me/hackthebox-flippin-bank/

https://dr3dd.gitlab.io/cryptography/2019/01/10/simple-AES-CBC-bit-flipping-attack/

https://medium.com/better-programming/strings-unicode-and-bytes-in-python-3-everything-you-always-wanted-to-know-27dc02ff2686

file:///home/greg/Downloads/[Crypto]%20Flippin%20Bank.pdf

Use python 2.7 and not python3 because it is easier to work with for cryptogrpahy sturff.

CBC divide the data to encrypt into different blocks. Each block is encrypted with the XOR and an encryption key.

Each block uses the previous block for the XOR operation. So if we change one byte of the previous block, then we might be able to change the value of the next block.

Let's say, we want to  change the value of the second block, then we have to  modify the first block.

In CBC encryption and decryption for first block there is no previous block so at that time iv is used.Let us review how the CBC decryption process work:The image above illustrates the decryption process, notice that if we changed a byte in the cipher text then the entire plain text block generated from that cipher block will be corrupted.But still in the next block of plain text, the only thing that will change is the exact byte corresponding to the byte we changed before.

![image.png](../_resources/c1ade3bba0324705a4c4b0306fc66134.png)

XOR compares each bits of two values and output 0 if bits are the same and output 1 if bits are different.