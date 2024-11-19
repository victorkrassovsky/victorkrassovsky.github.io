---
title: Cryptography
author: Victor Krassovsky
layout: article
mermaid: true
permalink: /cryptography/
---
These were some notes that I took while taking a course on cryptography. 

## Symmetric Encryption

The primary building block of secure communication is symmetric encryption. Symmetric encryption is described with a pair of ciphers $E$ and $D$ a secret key $k$. In other words, given a key space $\mathcal{K}$, a message space $\mathcal{M}$, and a cipher space $\mathcal{C}$, a cipher is a pair of "efficient" algorithms $E:\mathcal{K}\times \mathcal{M}\to \mathcal{C}$ and $D:\mathcal{K}\times\mathcal{C}\to\mathcal{M}$ so that

$$D(k,E(k,m))=m$$

for all $k\in \mathcal{K}$ and $m\in \mathcal{M}$.

## One Time Pad

One time pad is a symmetric encryption scheme in which the key is as long as the plaintext. Both the key and the plaintext are represented by strings of bits, and the encryption and decryption algorithms simply take the XOR of both their arguments. In other words,

$$E(k,m)=k\oplus m, D(c,m)=c\oplus m.$$

This does indeed satisfy the condition of consistency as $D(k,E(k,m))=k\oplus k\oplus m=0\oplus m=m$.

One time pad is a useful concept as it is secure and has fast encryption and decryption algorithms, but it is impractical as the key needs to be as long as the key plaintext. 

To demonstrate the security of OTP, it is necessary to first define what theoretic security is. We say that a cipher $(E,D)$ over $(\mathcal{K},\mathcal{M}, \mathcal{C})$ has perfect secrecy if $\forall m_0,m_1\in \mathcal{M}$ with $len(m_0)=len(m_1)$, $\forall c\in \mathcal{C}$, 

$$P(E(k,m_0)=c) = P(E(k,m_1)=c)$$

where $k$ is uniform on $\mathcal{K}$. In other words, the cipher text does not provide any information about the plain text, as any plain text has an equal probability of being correct, if the key is chosen uniformly at random. Essentially, this means that the encryption scheme is invulnerable to cipher text attacks. 

We can now prove that OTP indeed has perfect secrecy. We first need a lemma:

**Lemma 1.1** If $A,B$ are distributed at random on $(0,1)^n$ with $B$ distributed uniformly, then $A\oplus B$ is also distributed uniformly.  
*Proof:* We use the law of total probability and the fact that $f(b)=b\oplus c$ is bijective on $(0,1)^n$: 

$$
\begin{align*}
P(A\oplus B = c) &= \sum_{b\in (0,1)^n}P(A\oplus B=c | B=b)P(B=b)\\
&= \frac{1}{2^n}\sum_{b\in (0,1)^n}P(A\oplus b = c)\\
&= \frac{1}{2^n}\sum_{b\in(0,1)^n}P(A=b\oplus c)\\
&= \frac{1}{2^n}\sum_{a\in (0,1)^n}P(A=a)\\
&= \frac{1}{2^n}.
\end{align*}
$$

**Theorem 1.2** OTP has perfect secrecy.  
*Proof:* We have that $\mathcal{M}=\mathcal{C}=\mathcal{K}=(0,1)^n$. Let $m_0,m_1\in \mathcal{M}$ and $c\in \mathcal{C}$ be given and let $k\leftarrow \mathcal{K}$ be chosen uniformly at random. Then, from the lemma $E(k,m_0)=k\oplus m_0$ and $E(k,m_1)=k\oplus m_1$ are uniform on $\mathcal{C}$, hence

$$P(E(k,m_0)=c)=P(E(k,m_1)).$$

So, OTP is prefectly secure.

## Stream Ciphers

### Psuedorandom Generators

The issue with OTP is that keys need to be as long as the plaintext, hence encrypting messages is costly. Moreover, if there is a mechanism by which two parties can exchange keys securely, they could just use it to send messages instead. One might try to reuse a key $k$ to send two encrypted messages $m_0$ and $m_1$, which is called two time pad. This is insecure, however, as if an attacked XORs the cipher texts, they get $m_0\oplus m_1$. Then, if an attacker knows anything at all about the content of the plaintext, being english sentences for instance, then they can recover the plaintexts $m_0$ and $m_1$. Therefore, keys should never be used more than once.

To solve this issue, we use devices called psuedorandom generators which effectively generate longer keys from shorter ones in such a way that an attacker cannot distinguish between the generated keys and keys sampled at random. In symbols, a pseudorandom generator is a function $G:K \to (0,1)^n$ so that $G(k)$ is indistinguishable from a random string $r\in (0,1)^n$. To define distinguishability we use statistical tests which are algorithms on $(0,1)^n$ that output 0 or 1. An example of a statistical test could be $A(x)= \text{LSB}(x)$. 

We say that a PRG $G$ is secure if for all efficient statistical tests $A$, 

$$\text{Adv}_{\text{PRG}}[A,G]=|P(A(r)=1) - P(A(G(k))=1)| \le \epsilon$$ 

where $r$ is uniform on $(0,1)^n$, $k$ is uniform on $K$, and $\epsilon$ is negligible. The difference in probabilities is called the advantage of $A$ on $G$. 

The requirement that $A$ is efficient is important, as there are in fact no PRGs that are indistinguishable when subjected to an arbitrary statistical test. To see this, just let $A(x)$ be a test that is 1 when $x$ is in the image of the given PRG $G$. Then $P(A(G(k)))=1$, while $P(A(r))=\frac{\|K\|}{2^n}$, hence their difference should be non-negligible for any $\|K\| < 2^n$. 

### Semantic Security

Ciphers that use PRGs to generate their keys are called stream ciphers. Due to the restriction on the length of keys, it is not possible for stream ciphers to be perfectly secure. Instead, they settle for semantic security which captures the idea of the negligible distinguishability between PRGs and truely randomly sampled keys.

In the definition of semantic security, we have a challenger, who encrypts messages, and an adversary $A$ who is attempting to break the challengers ciphers. First the challenger takes two explicit messages $m_0,m_1$ and samples a key $k$ at random from the key set. Then, they encrypt one of the two messages $m_b$ with the key $k$ and send it to the adversary whose goal is to determine the value of $b$, or the message that was sent. Then, if $W_b$ is the event that the challenger chose message $b$, then $A$'s advantage over the cipher $E$ is 

$$\text{Adv}_{\text{SS}}[A,E] = |P(W_0)-P(W_1)|.$$

If this quantity is negligible for all efficient $A$, then $E$ is said to be semantically secure.It is not difficult to show that OTP is semantically secure, and in fact that perfect secrecy implies semantic secrecy.

All that's left is to show that a stream cipher that uses a secure PRG is semantically secure.

**Theorem 2.1:** Let $G$ be a secure PRG. Then the cipher defined by $E(k,m)=G(k)\oplus m$ and $D(k,m)=G(k)\oplus m$ is semantically secure.  
*Proof:* We will show that for all efficient adversaries $A$, there is an efficient statistical test $B$ so that

$$\text{Adv}_{\text{SS}}[A,E] \le 2\cdot \text{Adv}_{\text{PRG}}[B,G].$$

First, let $A$ be an adversary and define events $W_0$ and $W_1$ as in the definition of semantic security. Furthermore, play out the same situation except instead of using a stream cipher, use one time pad, and call the two resulting events $R_0$ and $R_1$. In other words, keep everything the same except replace $G(k)$ with an actually random $r$. Then, $\|P(R_0)-P(R_1)\| = 0$ since OTP is perfectly secure and $\text{Adv}[A,E]=\|P(W_0)-P(W_1)\|$.

Moreover, there is a statistical test $B$ so that $\|P(W_b)-P(R_b)\|=\text{Adv}_{PRG}[B,G]$, for $b\in\{0,1\}$, as this difference in probabilities describes the distinguishibility between $G$ and a truely random generator. Therefore, using the triangle inequality,

$$
\begin{align*}
  \text{Adv}_{\text{SS}}[A,E] &= |P(W_0)-P(W_1)|\\
  &\le |P(W_0)-P(R_0)| + |P(W_1)-P(R_1)| + |P(R_0)-P(R_1)|\\
  &= 2\cdot\text{Adv}_{\text{PRG}}[B,G]
\end{align*}
$$

Since $G$ is secure, the advantage of $B$ over $G$ must be negligible, hence the advantage of $A$ over $E$ is also negligible. So, we have won.

## Block Ciphers

Block ciphers are symmetric ciphers which map between plaintexts and ciphertexts of fixed lengths. Examples include 3DES and its successor AES. They are typically composed of a key expansion algorithm which expands a short key into several round keys, and a pseudorandom function called a round function, which takes a round key and some string and outputs another string of the same length. The round function is then successively applied to the input. 

### PRFs and PRPs

We begin to formalize the security of block ciphers with two definitions: Psuedorandom functions and psuedorandom permutations.

A psuedorandom function or PRF is defined over three sets $(K,X,Y)$ and is a function $F:K\times X\to Y$ so that $F(k,x)$ is efficient to compute. A pseudorandom permutation or PRP is essentially an invertible PRF; It is defined over two sets $(K,X)$ and is two functions, $E:K\times X\to X$ and $D:K\times X\to X$ so that $D(k,E(k,x))=x$ for all $k\in K$ and $x\in X$, with both $E$ and $D$ efficient to compute.

Furthermore, a PRF $F$ is secure if the set of functions from $X$ to $Y$ determined by $F(k,\cdot)$ is indistinguishable from the entire set of functions from $X$ to $Y$, denoted $Funs[X,Y]$. In other words, we can construct a similar experiment like the one in the previous chapter with an adversary who sends some queries to a challenger who encodes them with either a PRF or a randomly chosen function. The results are sent back to the adversary who then attempts to determine whether a PRF or a truely random function was used. If the adversary cannot do this efficiently, then the PRF is called secure.


### The Data Encryption Standard

DES, and later 3DES, utilizes something called a Feistal network to construct a PRP from a PRF. 

<img src="/assets/Feistel_cipher_diagram.png" alt="diagram" width="400" style="display: block; margin-left: auto; margin-right: auto; max-width: 300px; height: auto;"/>

Starting from the top in the encryption diagram, we break our plaintext into two blocks, $L_0$ and $R_0$, then transform succesively according to the rules $L_n=R_{n-1}$ and $R_n=L_{n-1} \oplus F(k_{n-1},R_{n-1})$. Then, we can invert this entire network very easily which is shown in the decryption diagram. In fact, the network is exactly the same, except we flip $R_{n+1}$ and $L_{n+1}$. Hence, if $F$ is a PRF, then the encryption network $E$ and decryption network $D$ form a PRP. Furthermore, a theorem due to Luby-Rackoff ('88) shows that if the Feistel network has at least three layers, then provided that $F$ is a secure PRF, $E$ and $D$ are also secure. 

### Attacks on DES

There are a few well known (often succesful) attacks on DES which is why it is no longer used. Usually these attacks require at least a few input output pairs in order to work. The most obvious such attack is an exhaustive key search, in which the attacker acquires a few blocks that were encrypted with the same key and tries to find the key that they were encrypted with. Namely, given $c_1, c_2, c_3$ ciphertexts, find $k\in \\{0,1\\}^{56}$ so that DES$(k,m_i)=c_i$. This works because of the following lemma:  

**Lemma 2.2:** Suppose that DES is an ideal cipher i.e. it consists of $2^{56}$ invertible functions $\pi : \\{0,1\\}^{64}\to\\{0,1\\}^64$. Then, for all messages $m$ and ciphertexts $c$ there is at most one key s.t. $c=\text{DES}(k,m)$ with probability $1-\frac{1}{256}$.  
*Proof:* We have that 
$$
\begin{align*}
	P(\exists k' \ne k : c=\text{DES}(k,m) = \text{DES}(k',m)) &\le \sum_{k'\in \\{0,1\\}^{56}} P(\text{DES}(k,m)=\text{DES}(k',m))\\
	&= 2^{56}\cdot\frac{1}{2^{64}}\\
	&= \frac{1}{256}.
\end{align*}
$$

Therefore, given several ciphertexts encrypted with the same key, the probability that there is another key that gives the same ciphertexts quickly becomes very small.  

Since DES's key size is only 56 bits, computers quickly became able to search the entire space in a relatively short amount of time. One idea to fix this was to artificially increase the key size by composing DES multiple times with itself. In particular, the 3DES cipher is defined by $\text{3DES}(m, k) = \text{DES}(\text{DES}^{-1}(\text{DES}(m,k_1),k_2),k_3)$. The key size is now three times as long, so the same exhaustive search attack no longer works. 

One thing to note is that DES composed with itself only once would not provide any additional security. An attacker would simply need to solve the equation $c = \text{DES}(\text{DES}(m,k_1), k_2)$, or $\text{DES}^{-1}(c,k_2) = \text{DES}(m,k_1)$. One could do this by precomputing the left side for all keys $k_2$, and then searching the key space for a $k_1$ that provides a collision. Therefore, the time complexity would only be about $2^{56}\cdot 2=2^{57}$ which is still completely insecure. This type of attack is called a meet-in-the-middle attack. 

There are also some other attacks on DES that are somewhat quicker, but require additional resources, which I will briefly mention. The first is a linear attack, which exploits the fact that one of the S-Boxes in DES was implemented slightly incorrectly. In particular, because one of the S-Boxes is very slightly linear, it introduces a linear relationship between the message and ciphertext. Given many many input ouput pairs, it is possible to compute this relationship which would give you some information about the key, drastically reducing the amount of time it would take to compute it. 

The other attack uses the fact that quantum computers are able to invert functions much quicker than their classical counterparts. In particular, if we are given $m,c=E(k,m)$, and we let $f$ be a function so that $f(k)=1$ if $E(k,m)=c$ and $f(k)=0$ otherwise, a quantum computer would be able to find $k$ so that $f(k)=1$ in time $2^{28}$, in comparison to the classical exhaustive search which takes $2^{56}$. 

### Advanced Encryption Standard (AES)

DES was eventually replaced by the NIST with the Advanced Encryption Standard or AES. It supports key sizes of 128, 192 or 256 bits, therefore somewhat future-proofing it against advances in technology and the development of quantum computers. Unlike DES, AES is not a Feistal network. Rather, it is a substitution-permutation network with key-expansion. 

<img src="/assets/subpermnet.svg" alt="diagram" width="400" style="display: block; margin-left: auto; margin-right: auto; max-width: 300px; height: auto;"/>

First the key is expanded into round keys $k_i$. Then, at each layer, we apply a substitution and a permutation followed by a XOR with the round key to the plaintext. Each layer is fully reversable, so to decrypt, we can simply repeat the key expansion algorithm, and follow the layers in reverse order to recover the plaintext. For AES-128, we need 11 round keys for 10 sub-perm layers. I have a full implementation of AES-128 <a href="https://github.com/victorkrassovsky/cryptography">here.</a>

