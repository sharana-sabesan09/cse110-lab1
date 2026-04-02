# Introduction to Sharana

**My name is Sharana Sabesan, a 2nd Year CS student at UCSD.**

<picture>
  <img src="ProfessionalPic%20(2).jpg" alt="Me" width="400">
</picture>

***I am interested in working with different types of data and mastering data handling methods to improve algorithmic optimization.***

> "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." — Martin Fowler



### My Hobbies

**My Favorite Project of All Time**

Proof of Work Algorithm - The Consensus Algorithm behind Bitcoin

'from hashlib import sha256 # computing hashes
import json # parsing data
import time # timestamps
import names # making transactions
import random # making transactions

DIFFICULTY = 4
BLOCK_LIMIT = 3

class Chain:
    def __init__(self):
        self.blockchain = [] # list of blocks
        self.pending    = [] # list of transactions yet to be added

    def add_transaction(self, sender, recipent, amount):
        transaction = {
            "sender" : sender,
            "recipent" : recipent,
            "amount" : amount
        }
        # ^ creates transaction with sender, recipent and amount being sent
        
        self.pending.append(transaction)
        # ^ transaction with sender, recipent and amount
        # is added to pending transactions list

    def compute_hash(self, block):
        json_block = json.dumps(block, sort_keys=True).encode()
        # ^ json.dumps converts python object into JSON/string form
        # ^ encode() converts to string into bytes
        # ^ to be acceptable by hash function.
        # ^ sort_keys specifies whether your values will be in sorted format

        curhash = sha256(json_block).hexdigest()
        # ^ sha256 creates hash for the block
        # ^ hexdigest() returns encoded data in hexadecimal format

        return curhash

    def mine_block(self, block):
        zero_list = ""
        for i in range(DIFFICULTY):
            zero_list = zero_list + "0"
        # ^ gives us target hash
        
        while self.compute_hash(block)[:DIFFICULTY] != zero_list:
            block["nonce"] = block["nonce"] + 1
        # ^ computes proof of work of hash by adding to nonce value
        # ^ guessing game until hash equals target hash
            
        print("Block " + str(block["index"]+ 1) + " MINED: " + self.compute_hash(block) + "\n")
        return self.compute_hash(block),block["nonce"]
    

    def add_block(self):
        if(len(self.blockchain) == 0):
            prevhash = "Genesis"
        else:
            prevhash = self.blockchain[len(self.blockchain) - 1]["proof"]
        # ^ sets prevhash if the block being created is genesis block. Otherwise, prevhash is calculated normally.
        # proof of work of previous block = previous hash
        
        block = {
            "index": len(self.blockchain),
            "timestamp": time.time(),
            "transactions": self.pending,
            "prevhash": prevhash,
            "nonce": 0
        }
        # creates block

        self.blockchain.append(block)
        # ^ block is added to chain
        
        self.pending = []
        # ^ list of transactions is cleared. We don't want old transactions from previous block in new block
    
        block["proof"], block["nonce"] = self.mine_block(block)
        # ^ calculates proof of work and updates nonce value; both variables are added to block
        
    def print_blockchain(self):
        print('-' * 80)
        print("ENTIRE BLOCKCHAIN LEDGER:\n")
        for block in self.blockchain:
            print("BLOCK " + str(block["index"]+1) + " : " + str(block) + "\n")
        # ^ prints blockchain in neat format

total_blocks = int(input("How many blocks should be in the blockchain? "))
# ^ finds how many blocks to create in blockchain based on user

chain = Chain()
# a chain object is created

for block in range(total_blocks):
    for transaction in range(BLOCK_LIMIT):
        chain.add_transaction(names.get_first_name(), names.get_first_name(), random.randrange(0, 2**31-1))
        # ^ each transaction has random sender, reciever, and amount
     chain.add_block()
     # block added to Chain object
# ^ the total blocks, said by user are created, with BLOCK_LIMIT transactions in each block


chain.print_blockchain()'