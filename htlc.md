##  HTLC in Lighting Network

Before we dive into the HTLC lets get a brief introduction about Lighting Network

### Lighting Network(LN)
Lightning network is a decentralized network, allowing thousands of transactions per seconds. It is second layer added to bitcoin blockchain which performs transactions between parties of bitcoin blockchain. 
The main aim of this network is to speed up or extends the limit transactions in bitcoin. This network allows users to create payment channels and made transactions away from the main blockchain,
and give benefits of blockchain's security.

![Architecture](C:/Users/HP/Desktop/LN.jpg)

### HTLC
HTLC stands for Hash Time Lock Contracts, and it doesn't require trusted third parties. In these type of contract payments are generated with "time lock". The structure of HTLC contains two parts
- Hash verification
- Time expire verification. 

Let's start with the hash, To make HTLC payment the secret message "M" should be created at first, and then its hash should be calculated using hash function.

``H = Hashing(M) ``

This hash H will be included in the locking script. Therefore, only the person who knows the secret that was hashed to get H will be able to use the payment. The secret M is known as ``preimage``.

The second part of the HTLC contract is the verification of the lock time for the payment. If the secret was not revealed in time and the payment was not used, the sender can retrieve all the funds.

To open a hash lock and claim a payment, the receiver needs to reveal the `preimage` to a hash digest encoded in the contract. 
To open a time lock and receive a refund, the spender needs to wait until after a certain time encoded in the contract. 

### LN Example
Now we have understanding of basic components of HTLC, let's understand the complete process through scenario. 
- Three persons alice, bob and caroline. 
- Alice has open channel to bob and bob has open payment channel to caroline.

![](C:/Users/HP/Desktop/person.png)

Alice wants to buy something from Caroline for 100 mBTC, Alice open a payment channel to Bob, and Bob opens a payment channel to Caroline, but Alice doesn't have direct open channel with Caroline.

To make a succesfully payment between Alice and Caroline they use a HTLC in the following manner.
- Caroline generates a random hash "H" with help the of any message "M". Caroline gives that hash to Alice.

![](C:/Users/HP/Desktop/caroin.png)
- Whereas Alice added hash given by caroline and make payment of 100 mBTC to bob (In order for bob to claim the payment, he needs the message which was used to produce that hash).

![](C:/Users/HP/Desktop/alicebob.png)
- Bob uses his payment channel to pay 100 mBTC to caroline, and he also adds the copy of conditions that alice put on when she gave payment to bob.(as similar to above step)
- Caroline has the original message that was used to produce the hash(also called a pre-image). At last Caroline uses message to finalize payment and fully receive the payment from bob, By doing this Caroline need to make the pre-image available to Bob. 

![](C:/Users/HP/Desktop/preimage.png)
- Bob uses the pre-image to finalize his payment from Alice.

On this way, Alice paid 100 mBTC without establishing a new channel that would link them directly. No one of the chain participants was obliged to trust others, and they earned 0.001 BTC as a fee for their role as middlemen.
inside the case of every body from the chain failing to perform there a part of work, nobody would have lost the cash that would honestly stay frozen until the expiration of the lock time.
