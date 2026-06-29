             Random number (???? bits)
                    |
                    |   ?????????
             Seed phrase (12/24 words, BIP-39)
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


## questions for Claude:
- is BIP24 a universal standard? Will my seed phrase work with every wallet which uses that standard?
- Does BIP24 refer to the entire process from random number to addresses, or just the last part (the tree derivation)

## Bigger topic for later:
- Transactions: why should private/public keys be used only once for receiving and sending?
- UTXO's, chain analysis/chain tracking and privacy
