---
title: Cryptography
author: Victor Krassovsky
date: 2024-07-12
category: Jekyll
layout: post
mermaid: true
---

## Symmetric Encryption

The primary building block of secure communication is symmetric encryption. Symmetric encryption is described with a pair of ciphers $E$ and $D$ a secret key $k$. In other words, given a key space $\mathcal{K}$, a message space $\mathcal{M}$, and a cipher space $\mathcal{C}$, a cipher is a pair of "efficient" algorithms $E:\mathcal{K}\times \mathcal{M}\to \mathcal{C}$ and $D:\mathcal{K}\times\mathcal{C}\to\mathcal{M}$ so that

$$D(k,E(k,m))=m$$

for all $k\in \mathcal{K}$ and $m\in \mathcal{M}$.

## One Time Pad

One time pad is a symmetric encryption scheme in which the key is as long as the plaintext. Both the key and the plaintext are represented by strings of bits, and the encryption and decryption algorithms simply take the XOR of both their arguments. In other words,

$$E(k,m)=k\oplus m, D(c,m)=c\oplus m.$$

This does indeed satisfy the condition of consistency as $D(k,E(k,m))=k\oplus k\oplus m=0\oplus m=m$.

One time pad is a useful concept as it is secure and has fast encryption and decryption algorithms, but it is impractical as the key needs to be as long as the key plaintext. 
