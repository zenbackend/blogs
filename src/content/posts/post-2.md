---
title: What is AES algorithm
description: "meta description"
date: 2022-04-02T05:00:00Z
image: "/images/posts/02.jpg"
categories: ["Security"]
authors: ["Durga Chikkala"]
tags: ["security", "algorithms"]
draft: false
---

Recently I have been building one of my personal projects and I want the data to be stored in a very secure way I am designing an application that will take care of the data security.

When it comes to data security we always refer to encryption and decryption so I am researching on which algorithms we have and what are advantages and disadvantages of it.

In this Journey, I learned about this powerful algo AES and sharing the details of it 



Let's not waste our time 

Get Started :
The Advanced Encryption Standard (AES) is a symmetric encryption algorithm established by the U.S. National Institute of Standards and Technology (NIST) in 2001. It is widely used for securing data in various applications due to its efficiency and strong security.

AES operates on fixed block sizes of 128 bits (16 bytes) and supports key sizes of 128, 192, and 256 bits. The different key sizes provide varying levels of security and performance.

Key Sizes and Their Implications
AES-128
Key Size: 128 bits (16 bytes)
Number of Rounds: 10
Security Level: Provides a good balance between security and performance. It is considered secure for most applications.
Performance: Faster compared to AES-192 and AES-256 due to fewer rounds.

AES-128 is often used in applications where performance is critical, and the level of security provided by 128-bit keys is deemed sufficient.

AES-192
Key Size: 192 bits (24 bytes)
Number of Rounds: 12
Security Level: Higher security compared to AES-128. Less commonly used but provides an additional security margin.
Performance: Slightly slower than AES-128 due to more rounds.

AES-192 is a middle-ground choice, offering better security than AES-128 but with a modest impact on performance.

AES-256
Key Size: 256 bits (32 bytes)
Number of Rounds: 14
Security Level: The highest security among the three. It is resistant to all known practical attacks.
Performance: Slowest of the three due to the highest number of rounds.

AES-256 is chosen for applications where maximum security is paramount, such as military, government, and high-security environments.



Security Considerations
Brute-Force Attack Resistance: The primary strength of AES lies in its key size. AES-128 requires 2^128 attempts to break, which is computationally infeasible with current technology. AES-256, with 2^256 possible keys, offers even stronger resistance.
Known Attacks: While theoretical attacks like related-key attacks and biclique attacks have been discovered, none of them are practical with current computing power. AES remains secure for practical purposes.
Future-Proofing: AES-256 is recommended for future-proofing against advancements in computational power and potential developments in quantum computing.

Use Cases
AES-128: Often used in mobile applications, VPNs, and other environments where performance is critical.
AES-192: Used in environments where an additional security margin is desired without significant performance trade-offs.
AES-256: Preferred in highly secure environments like financial institutions, government agencies, and military applications.

Structure of AES
AES encryption involves several key steps, repeated in a number of rounds. The number of rounds varies depending on the key size.

Key Expansion:

The original key (of size 128, 192, or 256 bits) is expanded into multiple round keys using a key schedule algorithm.
These round keys are derived from the initial key and are used in each round of encryption.
Initial Round: AddRoundKey: The plaintext block is XORed with the first round key.
Main Rounds (9 for AES-128, 11 for AES-192, and 13 for AES-256):

SubBytes: Each byte of the state is replaced with a corresponding byte from a fixed substitution table (S-box).
ShiftRows: Rows of the state are cyclically shifted by a certain number of bytes.
MixColumns: Columns of the state are mixed to provide diffusion. This step is omitted in the final round.
AddRoundKey: The state is XORed with the current round key.
Final Round: SubBytes, ShiftRows, AddRoundKey.



Let's walk through the AES encryption and decryption process step-by-step with an example. We'll use AES-256 for this demonstration.






Step-by-Step Process
1. Key and Plaintext Preparation
Key: "12345678901234567890123456789012"
Plaintext: "Hello, World!" (in bytes)

2. Key Expansion
AES-256 requires 14 rounds of processing, so the original key is expanded into 15 round keys (one for the initial AddRoundKey and 14 for the main and final rounds). This process involves complex mathematical operations but can be summarized as:

Original Key: 256 bits (32 bytes)
Round Keys: Derived from the original key using a key schedule algorithm

3. Initial Round
AddRoundKey: The plaintext is XORed with the first round key.

Let's visualize the process with simple hexadecimal representations :

Plaintext (in hex): 48656c6c6f2c20576f726c6421 (hex representation of "Hello, World!")
Round Key 0: aabbccddeeff00112233445566778899 (example round key)
AddRoundKey Result: 0xaabbccddeeff00112233445566778899 ^ 0x48656c6c6f2c20576f726c6421 = <result>

4. Main Rounds (repeated 13 times for AES-256)
Each main round consists of:

SubBytes: Each byte of the state is replaced using an S-box (substitution box).
ShiftRows: Rows of the state are shifted cyclically.
MixColumns: Columns of the state are mixed to provide diffusion.
AddRoundKey: The state is XORed with the current round key.

5. Final Round (14th round for AES-256)
The final round is similar to the main rounds but without the MixColumns step:

SubBytes
ShiftRows
AddRoundKey

6. Resulting Ciphertext
The output of the final round is the ciphertext. For our example:

Ciphertext: <final_ciphertext>



Let's explore some more encryption algorithms to use it in our projects 

see you guys in next article until then bye bye 

Durga Chikkala 

signing off.