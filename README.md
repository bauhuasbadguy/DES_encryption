# README #

In this repo I have recreated the Data Encryption Standard (DES) algorithm using python in order to further my own understanding and hopefully help anyone else interested in the implementation of this algorithm. Also worth noting is that DES is outdated and AES is now the accepted algorithm for data encryption.

### IMPORTANT NOTE ###

I am some guy in a room. You should never use the code here for real encryption purposes (and not just because since DES is outdated). I hope it will help you understand the principals of the DES but this has not been checked by anyone with any real expertise or training in cryptography.

### Algorithm description ###

#### Subkey generation ####

The first step is the generation of the subkeys. This is done by taking the provided key and putting it through the key generation scheme shown below in order to generate 16 subkeys.

<p align="center">
<image src='./images/key_generation_schedule.png' width="450px;"></image>
</p>

In this scheme PC1 and PC2 are permutation blocks. These blocks shuffle the bits of the message according to a pre-defined pattern (see source 2 for details), PC1 and PC2 also reduce the size of the message when performing this shuffle dropping certain bits, PC1 goes from 64 bits to 56 and PC2 goes from 56 bits to 48. The key generation starts by applying permutation block PC1, the result is then split into two 28 bit blocks and sent to the rounds structure. Each round involves first applying a leftwise bit rotation of one bit to the left and right sides of the input. The results are fed into PC2 in order to generate the 48 bit rounds subkey for this round. The left and right sides are then passed to the next round of subkey generation.

#### Encryption ####

The DES algorithm encrypts text in 64 bit blocks using a round structure of 16 rounds. This structure is shown below. 

<p align="center">
<image src='./images/round_structure.png' width="450px;"></image>
</p>

As the block is fed into the encryption algorithm the first thing that happens is an initial perturbation block, IP. This is a predefined shuffling of the bits as described in the key generation step except in this case the length of the message is retained as 64 bits total. The inverse of this step is found in FP just before the result being returned as cyphertext.

The shuffled message is then split in two. The right side is sent through the F-block and the result is XORed with the left side. The processed left side and the raw right side are then switched and the process is repeated for a total of 16 rounds. The subkeys we generated in the first part of the encryption process are used in the F-block, the structure of which is shown below.

<p align="center">
<image src='./images/F_block.png' width="450px;"></image>
</p>

This shows a 32 bit segment input being first expanded to 48 bits by using a similar perturbation function to that used in PC1, PC2, IP and FP except that this mapping reuses some positions meaning a 32 bit input becomes a 48 bit output. This is then XORed with the subkey for this round and split into 8, 6 bit elements. 

These 6 bit elements are passed through substitution boxes, or S-boxes, in order to generate 8, 4 bit outputs which are recombined into a 32 bit value which is passed through another perturbation box, P, in order to get the output from the F-box. 

In order to decrypt the message and recover the plaintext the encrypted text is run through the algorithm in exactly the same way but with the sub keys being used in reverse order.

### Sources ###

* https://en.wikipedia.org/wiki/Data_Encryption_Standard
* https://en.wikipedia.org/wiki/DES_supplementary_material
