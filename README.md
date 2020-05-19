# README #

In this repo I have recreated the Data Encryption Standard (DES) algorithm using python in order to further my own understanding and hopefully help anyone else interested in the implementation of this algorithm. Also worth noting is that DES is outdated and AES is now the accepted algorithm for data encryption.

### IMPORTANT NOTE ###

I am some guy in a room. You should never use the code here for real encryption purposes (not just because since DES is outdated). I hope it will help you understand the principals of the DES but this has not been checked by anyone with any real expertise or training in cryptography.

### Algorithm description ###

The first step is the generation of the subkeys. This is done by taking the provided key and putting it through the key generation scheme shown below in order to generate 16 subkeys.

<p align="center">
<image src='./key_generation_schedule.png' width="450px;"></image>
</p>

In this scheme PC1 and PC2 are permutation blocks. These blocks shuffle the bits of the message according to a pre-defined pattern (see source 2 for details). The key generation starts by applying permutation block PC1, the result is then split into two 32 bit blocks and sent to the rounds structure. Each round involves first applying a leftwise bit rotation of one bit, the results being fed into PC2 in order to generate the rounds subkey. The left and right sides are then swapped and passed to the next round of subkey generation.

These keys are then stored to be used in the F-block of the round structure shown below. 

<p align="center">
<image src='./round_structure.png' width="450px;"></image>
</p>

As the message is fed into the encryption algorithm the first thing that happens is an initial pertubation block, IP. This is a predefined suffling of the bits as described in the key generation step. The inverse of this step is found in FP just before the result is retuned as cyphertext.

The shuffled message is then split in two with the right side being sent through the F-block and the result is XORed with the right side. The left side and the right side are switched and the process is repeated for a total of 16 rounds. The structure of the F-block in this diagram is shown below.

<p align="center">
<image src='./F_block.png' width="450px;"></image>
</p>

This shows the 32 bit segment input being first expanded to 48 bits by using a similar pertubation function used in PC1, PC2, IP and FP except that the mapping reuses some postitions meaning a 32 bit input becomes a 48 bit input. This is then XORed with the subkey for this round and split into 8 6 bit elements. 

These 6 bit elements are passed through substitution boxes, or S-boxes, in order to generate 8 4 bit outputs which are recombined into a 32 bit value which is passed through another pertubation box in order to get the output from the F-box. 

In order to decrypt the message and recover the plaintext the encrypted text is run through the algorithm in exactly the same way but with the keys being used in reverse order.

### Sources ###

* https://en.wikipedia.org/wiki/Data_Encryption_Standard
* https://en.wikipedia.org/wiki/DES_supplementary_material

