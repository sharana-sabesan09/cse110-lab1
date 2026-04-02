# Introduction to Sharana

**My name is Sharana Sabesan, a 2nd Year CS student at UCSD.**

<picture>
  <img src="ProfessionalPic%20(2).jpg" alt="Me" width="400">
</picture>

***I am interested in working with different types of data and mastering data handling methods to improve algorithmic optimization.***

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." — Martin Fowler

Here is a link to my [resume](https://docs.google.com/document/d/13pVPTUSsmz0EPJOL6KBzpWxTSickfw0jf7HEu_u2jpE/edit?usp=sharing).

Link to: [Introduction](#introduction-to-sharana)

[ReadME file for this project](README.md)

### My Hobbies
* Reading Sci-Fi books
* Going on runs and swimming
* Watching movies from different countries (Korean, Spanish, Nigerian, etc.)
* Cooking (or trying to cook)
  
### The Hardest CS Classes, in my opinion
1. CSE 101 - Algorithm Design
2. CSE 110 - Software Dev
3. CSE 20 - Intro to Discrete Math

### My Goals for CSE 110
- [x] Lab 1
- [ ] Putting in consistent work, over one big burst of energy
- [ ] Designing a product I care about 

***My Favorite Project of All Time***

Proof of Work Algorithm - The Consensus Algorithm behind Bitcoin

```python
from hashlib import sha256
import json
import time
import names
import random

DIFFICULTY = 4
BLOCK_LIMIT = 3

class Chain:
    def __init__(self):
        self.blockchain = []  # list of blocks
        self.pending = []     # list of transactions yet to be added

    def add_transaction(self, sender, recipient, amount):
        transaction = {
            "sender": sender,
            "recipient": recipient,
            "amount": amount
        }
        self.pending.append(transaction)

    def compute_hash(self, block):
        json_block = json.dumps(block, sort_keys=True).encode()
        return sha256(json_block).hexdigest()

    def mine_block(self, block):
        target = "0" * DIFFICULTY
        while self.compute_hash(block)[:DIFFICULTY] != target:
            block["nonce"] += 1
        print(f"Block {block['index'] + 1} MINED: {self.compute_hash(block)}\n")
        return self.compute_hash(block), block["nonce"]

    def add_block(self):
        prevhash = self.blockchain[-1]["proof"] if self.blockchain else "Genesis"
        block = {
            "index": len(self.blockchain),
            "timestamp": time.time(),
            "transactions": self.pending,
            "prevhash": prevhash,
            "nonce": 0
        }
        self.blockchain.append(block)
        self.pending = []
        block["proof"], block["nonce"] = self.mine_block(block)

    def print_blockchain(self):
        print('-' * 80)
        print("ENTIRE BLOCKCHAIN LEDGER:\n")
        for block in self.blockchain:
            print(f"BLOCK {block['index']+1} : {block}\n")


# --- Run the blockchain ---
total_blocks = int(input("How many blocks should be in the blockchain? "))
chain = Chain()

for _ in range(total_blocks):
    for _ in range(BLOCK_LIMIT):
        chain.add_transaction(
            names.get_first_name(),
            names.get_first_name(),
            random.randrange(0, 2**31 - 1)
        )
    chain.add_block()

chain.print_blockchain()