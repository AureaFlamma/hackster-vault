# BASIC STRUCTURE



                            Random number (128/256 bits)                                 // Derived from quasi-random data (CPU temp fluctuations, microphone static noise etc.) via *CSPRNG*
                                    |
                                    |  BIP-39                                            // BIP_39_dictionary(Random number + SHA-256(Random_number).[0-4]) 
                                    |                                                    // random number is hashed, and the first 4/8 bits of the hash are appended to the number as a checksum.
                                    |                                                    // That gives us 132/263 bits, which are then divided into 12/24 11-bit packets. Each packet correspons to a  word
                                    |                                                    // from the BIP-39 wordlist, whose length is 2^11 = 2048
                                    |                                                    // PURPOSE: -  checksum is there to protect from mistyping/data corruption via avalanche effect.
                                    |                                                    //          -  the wordlist is there to make it human-readable/memorable. (words as chosen in such a way that the first 3 letters of a word can identify a word)
                            Seed phrase (12/24 words)
                                    |
"mnemonic" + passphrase (salt) -->  |  BIP-39 (PBKDF2)                                   // concatenated seed phrase (with string "mnemonic" is concatenated with optional passphrase and used as salt) is hashed with SHA-512 256 times.
                                    |                                                    // PURPOSE: 
                                    |                                                    // - The repeated hashing is now thought obsolete. It is meant to make *brute-forcing (guessing)* seeds more difficult. In practice, it makes a difference
                                    |                                                    //    of minutes vs days, rather than feasible to crack vs non-feasible.  
                                    |                                                    // - passphrase is similarly controversial. See below.
                                    v
                            Binary seed (512 bits)
                                    |
   "Bitcoin seed" --------------->  |  BIP-32                                            // BIP-32 describes how the tree is derived from the binary seed:
                                    |                                                    // - both the mechanics (HMAC-SHA512, chain codes, the whole math)
                                    |                                                    // - and the basic tree structure it: that there are nodes with parents and children, chains, how much depth is possible (hint: a lot)
                                    |
                                    v
                            Master private key
                                    |
                                    |  BIP-32 (mechanics)
                                    |  BIP-43 (meta-rule)                                // BIP-43 is the convention which introduces purposes. It says  that the node below the master is the purpose node. 
                                    |                                                    // Without it, each wallet would be free to construct the tree in its own way, and assign nodes its own functions,
                                    |                                                    // within the basic structure parameters described by BIP-32 
                                    |
                                    +--------------------+--------- ...
                                    |                    |                    
                               purpose (44')        purpose (84')        
                                    |                    
                                    |  BIP-44/49/84... (rules)                           // BIP-44 describes the semantics of the 5 levels of the tree (purpose/type/account/chain/address). BIP-49 and others changed the format of addresses.
                                    |                    
                                    +-----------------------------+---- ...
                                    |                             |
                               coin type (0' = BTC)           coin type (60' = ETH)
                                    |
                                    +--------------------+--- ...
                                    |                    |
                               account (0')         account (1')
                                    |
                                    +--------------------+
                                    |                    |
                              chain 0 (receive)    chain 1 (change)
                                    |                    |
                                    +---+---+            +---+---+
                                    |   |   |            |   |   |
                                  addr addr addr       addr addr addr
                                   0    1   ...         0    1   ...


## Hierarchical Deterministic derivation in detail

                                                                Binary seed (512 bits)
                                                                        |
                                                                        | HMAC-SHA512 ("Bitcoin seed")
                                                                        |
                                                                        |
                                                                        V 
                                                      (left 256 bits)   +   (right 256 bits)                    
                                                    master private key      master chain code
                                                            |_______________________|                                         // HARDENED  DERIVATION: private + chain code
                                                               ________|_____________________                      
                                                              |        |                    |
                                                              |        |                    |
                                                  HMAC-SHA512 |        | HMAC-SHA512        |
                                                 (+ index 0)  |        | (+ index 1)        |
                                                              |        V                    V
                                                              |     child 1 private key    ...
                                                              |         +
                                                              |     chain code           
         SHA256                                               |
         + RIPEMD160                                          |
         + encoding              secp256k1                    V
  child <------------ child <----------------  child private + child chain 
  address 0          public key 0                  key 0          code 0          
                        |                                            |
                        |                                            |
                        |                                            |
                        |____________________________________________|                                                        // NORMAL DERIVATION: public key + chain code
                                    |
                                 ___|_______________
                                 |                 |
                                 V                 V
                            grandchild 0          ....
                             private key
                                 +
                             chain code



                               
## Bitcoin Improvement Proposals:
- BIP-32 - specifies how Hierarchical Deterministic tree is to be derived from Binary seed
- BIP-39 - specifies how Binary seed is to be derived from the random number, including seed phrase derivation
- BIP-43 - specifies that the first node in a HD tree is the purpose node - this allows for different semantics/shapes of the tree. 
- BIP-44/49/84/86 - the specific purposes/shapes of the tree // <?> 

## Passphrases
The passphrase is  meant to add extra protection if the seed phrase is leaked. But the only way it can do that, is if it is cryptographically strong - with around 128 bits of entrophy - at which point it is the length of the seed phrase istself and non-human readable. 
This means that it has to be stored in a paper backup (rather than memorised)... but since its meant to protect against leaked seed phrases, it has to be stored separately to the seed phrase... this increases the risk of losing
coins due to lost phrases - since now there are twice as many things you mustn't lose.
Another use case is to add another node at which the tree can diverge - seed phrase + passphrase A yields a different binary seed than same seed phrase with passphrase B, allowing for more than one wallet from the same seed phrase. 
This may be useful against the 5-pound spanner attack (duress) in that it allows for creation of decoy wallets. However, for normal use cases (different wallet for spending, rainy day, investment etc.), BIP-44+ ensures we can have many accounts on each wallet.

## Attack vectors
- Brute-forcing (guessing)

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


