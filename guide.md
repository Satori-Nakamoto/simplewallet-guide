# Guide to simplewallet
## Based on Masari v0.3

### Basics

After daemon is synced, open a new terminal tab with Ctrl + Shift + t

`./masari-wallet-cli`

Create a new wallet by entering any name for the wallet. Then input a password and choose your language. Make sure to copy down
and keep your 25 word seed safe- with just those 25 words you can access and restore the entire wallet, including spending all
the funds on it. Now you're ready to begin.

The general structure of simplewallet is:

#### wallet > account > address

meaning that a wallet can contain several (hundreds? thousands?) accounts and each account can contain several 
(hundreds? thousands?) addresses. The wallet starts out with one account with one address on it. Let's make more.

First, a new account within our wallet:

`account new garage sale income`

This will make a new account with the label "garage sale income". Notice that white spaces are allowed in the new account name.
Let's add another account for "everyday cash"

`account new everyday cash`

As you can see, we now have 3 accounts indexed 0,1, and 2. To switch between accounts, use 

`account switch <index>`

Let's say someone wants to buy the TV at our online garage sale. We should switch to account 1- garage sale income, and create 
a new address to receive the funds. Let's add the label "old TV", so we can remember the source of the income when tax season 
inevitably rolls around.

`account switch 1`

`address new old TV`

Now we can give this new address 9pDUb...43yY to the customer. The price for the old TV is 2 MSR. When we want to verify that
the customer has paid, we can to switch to the relevant account and use

`address all`

to show all addresses within that account and whether or not they have been used. To check balance, simply type

`balance`

To see specifically which address received the funds, we can use

`show_transfers in`

where we see (in order) block height, transaction in or out, time, amount, txid, payment id, and index. The index 1
corresponds to our generated address "old TV" index 1, so we know the TV is paid for. For the sake of privacy, we won't reuse 
this address.

### Sending

Now say we want to send 1 MSR to a friend. After we open the wallet and select the account we want to send from, we can
use the transfer command. The most general transfer command is

`transfer <address> <amount>`

Sending 1 MSR to the address our friend gave us would then look like this

`transfer 9jNEbBf4eUUeNvKEJVpeqGa86iavuJzhAV35REsYNh7KEHrFrHFJAMJiA1PPdLZdAvDAN7ps4Jn6iLfobCmMyT9pV17nrRi 1`

If you use a payment ID, it should be inserted after <amount>. Using a new address for each transaction is recommended, though.
If you want to reuse addresses, you can use the address book funcion.

### Address book

To add an entry (white spaces are allowed in description):

`address_book add <address> <description>`

To show all entries in your address book, use

`address_book`

And to delete an entry, use

`address_book delete <index>`

All of these commands will show your address book after executing.

### Proving sending

You will need 3 things to prove that you sent funds: txid, tx key, and the wallet address you sent to. In this case we sent
to the address
9jNEbBf4eUUeNvKEJVpeqGa86iavuJzhAV35REsYNh7KEHrFrHFJAMJiA1PPdLZdAvDAN7ps4Jn6iLfobCmMyT9pV17nrRi
which can be verified when we get the txid and show the outbound transfers. We do that with

`show_transfers out`

The txid is the string after the date and amount sent. In this case it's
13f7529685020eb3e69b401acc81968f90fe30024492f7b599053bf2ec269188
Save this for step 3. Notice that the destination address displayed here.

Next, get the tx key with

`get_tx_key <txid>`

In this case it's
e8b96020ae99162c0d87d35d5d96e960e573189cefea634660c0cb1f01a6d905
Save this for step 3.

Finally, check the tx key with the address you sent to

`check_tx_key <txid> <txkey> <address>`

Possible outcomes are:
 `<address> received <amount> in txid <txid>`
or
 `Error: <address> received nothing in txid <txid>`

###Signing and verifying

Since cryptocurrencies are founded on cryptography, we can also do some pretty neat things with the simplewallet, like
sign and verify files. Let's make a text file with a message, say message.txt, and sign it with:

`sign <file_to_sign>`

for example: 

`sign /home/me/Desktop/message.txt`

This will print out a signature. You need to give that signature and your wallet address to the other party to 
verify the file with the verify command:

`verify <filename> <address> <signature>`

Send them the file, the signature you created, and your wallet address. They can verify that the file has not been 
tampered with by doing:

`verify /home/my_friend/Downloads/message.txt <my address> <signature from previous step>`

The two possible outcomes are:

 `Good signature from <address>`

or

 `Error: Bad signature from <address>`
