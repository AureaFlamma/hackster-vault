             Random number (128/256 bits)
                    |
                    |   *BIP-39*
                    |
             Seed phrase (12/24 words)
                    |
                    | PBKDF2 → 512-bit seed → BIP-32 [1:1]
                    v
           Master private key (1 per wallet)
                    |
                    | BIP-32 derivation path  [1 : ~4 billion]
    ________________|________________________________
    |               |                               |
    v               v                               v
Private key 0   Private key 1                 Private key n
(m/.../0/0)     (m/.../0/1)                    (m/.../0/n)
    |               |                               |
    | secp256k1     | secp256k1                     | 
    | [1:1]         | [1:1]                         | 
    v               v                               v
Public key 0    Public key 1    ...           Public key n
(33 bytes)      (33 bytes)                          |            
    |               |                               |
    | SHA256+       | SHA256+                       |
    | RIPEMD160     | RIPEMD160                     | 
    | [1:many]      | [1:many]                      | 
   / \             / \                             / \
  v   v           v   v                           v   v
Addr  Addr      Addr  Addr     ...            Addr  Addr
0a    0b        1a    1b                       na    nb

(same public key → Legacy 1..., P2SH 3..., or SegWit bc1... addresses)

                     Random number (128/256 bits)
                            |
                            |   *BIP-39*
                            |
                     Seed phrase (12/24 words)
                            |
"mnemonic" + salt --------> | PBKDF2 → 512-bit seed → BIP-32 [1:1]
                            |
                            v
                      Binary seed (512 bits) 
                            |
"Bitcoin seed" -----------> | BIP-32 derivation path  [1 : ~4 billion]
                            |
                            V
                    Master private key
                            | 
                            |_______________ account 0
                            |                   |
                            |                   |_____ branch 0 (receiving)
                            |--- account 1      |         |-- address 0
                            |--- ...            |         |-- address 1
                            |--- account n      |         |-- ...
                                                |         |__ address n
                                                |
                                                |        
                                                |_____ branch 1 (change)
                                                          |-- address 0
                                                          |-- ...
                                                         



                     Random number (128/256 bits)
                            |
                            |   *BIP-39*
                            |
                     Seed phrase (12/24 words)
                            |
"mnemonic" + salt --------> | PBKDF2 → 512-bit seed → BIP-32 [1:1]
                            |
                            v
                      Binary seed (512 bits) 
                            |
"Bitcoin seed" -----------> | BIP-32 derivation path  [1 : ~4 billion]
                            |
                            V
                    Master private key
                            | 
                            |------------------- purpose (84')
                            |--- ...                |_____________ coin type
                            |--- purpose (n)        |--- ....          |---------- account
                                                                       |--- ...        |---- branch/chain (0 - receiving) 
                                                                                                 |--- address 0
                                                                                                 |--- address 1
                                                                                                 |--- 






## Random number
Has 128/256 bits. Obtained through *CSPRNG*.
> The randomness is the most crucial security feature. All other safeguards are only additions. 

## Random number -> seed phrase
Random number is **hashed** with **SHA-256** and the first 4/8 bits (length/32) of the hash are appended. This yields 132/264 bits which are divided into 12/24 11-bit chunks. Each chunk corresponds to one of the 2^11 (2048) words in a BIP-39 dictionary/map.
Rationale: 
- checksum: If you mistype a word whilst entering the seed phrase into a new wallet, the checksum almost definitely won't match, due to the **avalanche effect** of hashing.
- dictionary: To make it human readable


## Seed phrase -> master private key
**PBKDF2**, a key-stretching function, is applied to the seed phrase:
- the words are concatenated
- a **salt** is added (salt = "mnemonic"+user passphrase)     
- the resulting string is hashed with **SHA-512** 2048 times.
The result has 512 bits.

Twofold rationale: 
- Passphrase:
    - allows you to create multiple wallets from the same seed phrase, by using different passphrases for each.
    - If strong, protects you against brute forcing your wallet when your seed is leaked. The attacker has to have your seed phrase and then brute force the seedphrase + passphrase hash... however, your passphrase would have to be have some entropy.
      Brain passphrases have low entropy and are easily brute-forced. Though, it does protect you against a scenario where a non-technical person (your cleaner) discovers your paper backup and doesn't know how to get around cracking the passphrase.
      In other words, the attacker would have to both be a hacker and have physical access to your paper wallet, reducing the attack surface. 
- SHA-512 * 2048: In theory, it makes brute forcing 2048 times slower. For every seed (+ passphrase) the attacker guesses, they have to hash it 2048 times. Though that's the difference between cracking something in minutes versus days.
  This element of PBKDF2 is now considered redundant, as the security provided by it is overshadowed by the entropy of the seed phrase. A non-random seed phrase is crackable with or without it, and a strong one is non-crackable with or without it.


## questions for Claude:
- is BIP24 a universal standard? Will my seed phrase work with every wallet which uses that standard?
- Does BIP24 refer to the entire process from random number to addresses, or just the last part (the tree derivation)
- What is CSPRNG and are there other randomness manipulators?
- What precisely is BIP-39? Is it a process that only affects the master -> seed phrase step or the whole derivation? Is it an algorithm? 
## Bigger topic for later:
- Transactions: why should private/public keys be used only once for receiving and sending?
- UTXO's, chain analysis/chain tracking and privacy
- Who maintains bitcoin? Who approves PRs? What are BIPs?

## Other
- what are bits?


