# chapter 4 - keys and addresses
--------------------------------

## cryptographic hash functions
any function that can be used to map data of arbitrary size to data of fixed size.

Ethereum’s Cryptographic Hash Function: Keccak-256

How can you tell if the software library you are using implements FIPS-202 SHA-3 or Keccak-256, if both might be called "SHA-3"?

An easy way to tell is to use a test vector, an expected output for a given input. The test most commonly used for a hash function is the empty input. If you run the hash function with an empty string as input you should see the following results:

Keccak256("") =
  c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470

SHA3("") =
  a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a


## eth addresses

Ethereum addresses are hexadecimal numbers, identifiers derived from the last 20 bytes of the Keccak-256 hash of the public key.

