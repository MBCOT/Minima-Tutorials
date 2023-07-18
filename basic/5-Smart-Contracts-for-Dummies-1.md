------------------------------------------------------------
### **Tutorial 5**: Learning Smart Contracts for Dummies
### **- Part 1**: Transactions and Scripts
------------------------------------------------------------

So in this tutorial will learn how to do a transaction, automatically with the "send" command and manually using minima commands about transactions, and also we will learn what is a script and focus on concepts that will be neccesary to understand in orther to succeed with Smart Contracts.

- Before learn about Smart contracts(scripts) we need to understand Transactions, and understand that an **address** is in reality a script(the hash of it).


- 1: **First** we should read :  <https://docs.minima.global/docs/learn/transactions/>  from here we extract some important information and clarify some terms, and do the simplest tests possible with scripts. Being successful with **Smart Contracts (Scripts)** involves a lot of playing around, reading about them is easy, but you need to play with them to really understand how they work. Lets begin to extract some concepts from the link about coins, transactions, scripts, addresses.

  - a: **What is a coin ?** A coin (UTxO) is generated as a result of a transaction.
  So, to understand that, we have to go back to the origin, "the genesis", where the total supply is created.
  Now, after genesis, we have the first coin with an amount of 1 Billion Minima and an address where the coin is. That address, in reality is a script (the hash of script) that dictates the conditions of how the coin can be spent.
  **So we can assert that the coin is deposited into an script (address)**

  - b: **Nodes, Addresses vs Scripts** To understand how scripts work, we have to clarify one very important thing: **Any amount of Minima can be deposited into a script** by sending an existing coin to the address of the script (the hash of the script), but at the same time evrey coin in into an script that protects it.
  So what ` send ` command does before sending the coin, is to build a automatically a transaction where it must satifsy  the script conditions where the coin is locked by the script to unlock the coin only signing the transaction with the publickey of the address where the coin is, but if the coin is locked into some specific script, then the send command could not be usefull, you would have to build a specfic transaction to meet the script conditions in order to spend(send) the coin.

  - c: **Nodes and how an address is generated**, there is a minima command "newaddress" that generates a new "address" where anyone can send coins/tokens and only the owner can spend it.
  --> But what does this command really do?
    - Using the private key of the node, a derived public key is generated and a default script is created with that content: **"RETURN SIGNEDBY(publickey generated)"**, the hash of this script is the "address" and this "address" is unique, because it has been generated from a private key to get a public key that nobody else can generate and therefore cannot be created by anyone else.

  - d: **Conclusion 1:** When we send coins/tokens to an address, what we are doing is sending coins/tokens to an script.

- 2: **Second** To understand what we talked before and identify every element we mentioned (script, address, coin, UTxO, transaction) we are going to do a couple of tests.
  - **TEST 1**: We will trigger an automatically generated transaction using ` send ` command to watch how the transaction is built, looking specially at the input and output parameteres of this transaction and when the transaction be confirmed by the network the console will prompt that a one or more **coin** have been spent and two new unspent **coins** have been created.
    - **----> So lets prepare for the test 1:**
      - We are going to do the test on the same node, so we are going to send 10 minimas to one address of the node from another address of the same node(this other address is supposed to have at least more than 10 minimas)
      - So lets prepare the node for the test, we need two addresses so we execute two times the command ` newaddress ` and write it down the public key and the address of the output of the command, so in my case I have the next values:
        - **Address 1**:
          - "publickey":"0x2852EE11215574EDD2105E85EBF6B0F84AF86C748BFBF4C9D1083754F4CDF390"
          - "address":"0x27B791F4AD477551C5B305187FD8AFA2EEB73441EE82816171E15278C9040945"
        - **Address 2**:
          - "publickey":"0x4C64D239A115A774872AC3474ECDB4EA4FA203F1B5087EF75050AACFF1D3FA98"
          - "address":"0xC7308B3ABB2D0CED8DA5A7EDFB522E8BEEC88607EE83108338FCB68BCC1A54AA"
        - Send to "Address 1" 100 minima from the same node or other node or if you don't have any of them, ask for them in the minima discord channel **#app-chat** to @jazzmine
    - **----> Lets start de test 1:**
      - Send 10 minima from "address 1" to **"address 2"** ` :
        - **Note:** here we can't assure that "address 1" will be picked as the origin of the funds, the command ` send ` will pick an address were at least there are 10 minima or more or if there are not any addresses with that amount, the command will pick several of them if they all at least sum 10 minima and will use them as input coins, so in that particular case, your transaction can have more than one input coins.

      **send address:0xC7308B3ABB2D0CED8DA5A7EDFB522E8BEEC88607EE83108338FCB68BCC1A54AA amount:10**

      - Response from command executed (parts of it):
```
"txn":{
        "inputs":[{
          "coinid":"0x07B3D2D4E30B40AD737527A58E7B5812281780A0E46672C1E6C70750475A6FD5",
          "amount":"100",
          "address":"0x27B791F4AD477551C5B305187FD8AFA2EEB73441EE82816171E15278C9040945",
        }],
        "outputs":[{
          "coinid":"0x8EB06577B6438BE41EA1AE010CA011E7222BF65A36A6115ED1944D1E9882E004",
          "amount":"10",
          "address":"0xC7308B3ABB2D0CED8DA5A7EDFB522E8BEEC88607EE83108338FCB68BCC1A54AA",
        },
        {
          "coinid":"0x6633F1627BE1C3E44BEBED7F2952C4F08472BE5218E9445C2587308D51B2F3C5",
          "amount":"90",
          "address":"0xA9FA9B9037FAF7522AB2284E32508E3B0CC349D70BC39C4F29260B6E1CCA38C8",
        }],
        ...
        ....
        ....
        "scripts":[{
          "script":"RETURN SIGNEDBY(0x2852EE11215574EDD2105E85EBF6B0F84AF86C748BFBF4C9D1083754F4CDF390)",
          "address":"0x27B791F4AD477551C5B305187FD8AFA2EEB73441EE82816171E15278C9040945",
          "proof":{
            "blocktime":"0",
            "proof":[],
            "prooflength":0
          }
        }]
```
      - After the transaction has been taken out of the mempool, put in a block and propagated over the network and confirmed, the console will prompt something like this:

```
: NEW Spent Coin : {"coinid":"0x07B3D2D4E30B40AD737527A58E7B5812281780A0E46672C1E6C70750475A6FD5","amount":"100","address":"0x27B791F4AD477551C5B305187FD8AFA2EEB73441EE82816171E15278C9040945","miniaddress":"Mx17MU8V9BA7EY8SBCZ531VTHBT2TQRJ8GFEGA0M2SF1A9SCW1098MCEAPWR","tokenid":"0x00","token":null,"storestate":true,"state":[],"spent":true,"mmrentry":"2","created":"112"}
: NEW Unspent Coin : {"coinid":"0x8EB06577B6438BE41EA1AE010CA011E7222BF65A36A6115ED1944D1E9882E004","amount":"10","address":"0xC7308B3ABB2D0CED8DA5A7EDFB522E8BEEC88607EE83108338FCB68BCC1A54AA","miniaddress":"Mx67625JYEPD1JMZR9D7TNTY4BKBTR48C1VEGC886E7SMQ5SZ6WKYA44R9NT","tokenid":"0x00","token":null,"storestate":true,"state":[],"spent":false,"mmrentry":"4","created":"578"}
: NEW Unspent Coin : {"coinid":"0x6633F1627BE1C3E44BEBED7F2952C4F08472BE5218E9445C2587308D51B2F3C5","amount":"90","address":"0xA9FA9B9037FAF7522AB2284E32508E3B0CC349D70BC39C4F29260B6E1CCA38C8","miniaddress":"Mx59VADP0DVQUT92YCH89ZP513HR1J1KJYZBZEE4UA961DN1PWHZP3JV55VB","tokenid":"0x00","token":null,"storestate":false,"state":[],"spent":false,"mmrentry":"5","created":"578"}
```


  - Lets go to analyse all of this generated data while we are checking the documentation and the facts and conclusions we gathered on the previous parts of this tutorial.

    - **Input parameters of the transaction**:
      - The UTxO (Unspent coin) to be spent in this transaction (all amount will be spent) identified by unique CoinID.
          - **Remember** A coin is always generated as a result of a transaction and sent/locked into a script (the hash of the script is the address) and the script controls in which conditions the coin can be spent.
        - Amount:100, when we were preparing the things to do the test, we sent 100 minima to an addresses, "Address 1"
        - Address, you can check that is the same "address 1" where we sent 100 minima before.
        - As you can see, when the transaction is executed and confirmed the console prompts: "NEW Spent Coin" and indicates the ammount:100 (All amount has been spent) and the "Address 1" that is just the address of the CoinID in the inputs.
          - **Remember** for spending a coin, the script where the coin is "(address)" must satisfy the conditions so the transaction must execute the script, the transaction also shows the scripts and his address, so we can verify that the transaction must execute:
           "script":"RETURN SIGNEDBY(0x2852EE11215574EDD2105E85EBF6B0F84AF86C748BFBF4C9D1083754F4CDF390)", and as this is a publickey generated from the private keys of the node, the result will be true, and the coin could be spent becouse the script returned TRUE.

    - **Output parameters of the transaction**:
      - As you can see, there are two outputs on the transaction, that means that two new unspent coins will be created.
      - The first output, is just the "address 2" that we created to receive 10 minimas just executing the comand we did before: **send address:0xC7308B3ABB2D0CED8DA5A7EDFB522E8BEEC88607EE83108338FCB68BCC1A54AA amount:10**
        And you can check that once the transaction is confirmed the console outputs ": NEW Unspent Coin :" with the information related to this new coin that has been sent to the script represented by the "address 2"
      - The second output, is the "change" as you can find on the documentation, it is automatically calculated from the ` send ` command. **Remember** every input will be spent, so in our case, we had 100 minima as an input on "address 1" and wanted to spent 10 minima sending them to "address 2" to we need to spent the difference 100-10= 90 Minima and send it back to the origin, that origin can be any "address" from your node, in our example this: "address":"0xA9FA9B9037FAF7522AB2284E32508E3B0CC349D70BC39C4F29260B6E1CCA38C8", and when the transaction has been confirmed you can check on the console that ": NEW Unspent Coin :" has been generated of 90 minima.

      - Now we can check then following:
        - 100 minima has been spent, if you check "address 1" there is no more the 100 minima.
        - 1 new UTxO of 10 minima has been created and sent to "address 2"
        - 1 new UTxO of 90 minima has been created as a change and sent back to one address of the origin Node where the input coin came from.

    - **Lets summarize** the important things we have learnt that later will be useful to understand how to build scripts.

      - 1: When we send any coin/token to an address, **we are sending the coin/token to an script**, the address is simply the hash of the script, so , the coin/token sent to an address is locked under the conditions of the script.
      - 2: Any of the input coins on a transaction will be spent if the execution of the script, where every coin is locked in, return TRUE, if one of the scripts of the input coins return FALSE, then the transaction will fail.
      - 3: Any transaction will create one or more new UtxO coins.
      - 4: The sum of all input coins that is less than the sum of the outputs will be burnt, so if we used an input coin that has 100 minima and only one output that spend 10 minima, then 90 minima will be burnt and lost.

  - **TEST 2**: We will do the transaction manually, ourselves, with the help of minima commands about transactions.
  We are gonna use the same setup as we did on the "TEST 1" , the same "address 1", "address 2", and the same amounts 100 and 10.
  We are going to use the following minima transaction commands:
    - txncreate
    - txninput
    - txncheck
    - txnlist
    - txnoutput
    - txnsign
    - txnpost
    - You can find what every command does typing "help" on the terminal of your mobile or in the console of your server.

    - **----> Lets start de test 2** Commands to execute to do the transaction manually, only relevant information of the ouput generated by the command will be shown:
      - txncreate id:test2   (id must be unique, so usually is a random number generated)
        - Response: ` "transactionid":"0xAB728B0D4A215A520DAA3DB2A618CBD24AB3A3A36293B07FA2EF55CBBD765613" `
      - txninput id:test2 coinid:0x2AC4047C0DD28F2910D109F8399C278BFD029BAD0E6E1F4E07ED0ACB9540C07A
        - use ` coins relevant:true ` to find the **coinid** of the coind deposited on "address 1"
      - **Using** ` txncheck id:tes2 ` and ` txnlist ` commands you can watch the content of the transaction while it is being built.
      - txnoutput id:test2 address:0xC7308B3ABB2D0CED8DA5A7EDFB522E8BEEC88607EE83108338FCB68BCC1A54AA amount:10
        - We used "address 2" as an output
        - **hint** If you use the command ` txncheck id:test2 ` you won't have to calculate the change back, it will prompt several parameters and one of them is "difference":"90" that is your change back you have to use in the next output.
      - txnoutput id:test2 address:0x27B791F4AD477551C5B305187FD8AFA2EEB73441EE82816171E15278C9040945 amount:90
        - We used "address 1" as an output that is the same input, remember that in test 1 another address was choosen automatically by the "send" command.
      - txnsign id:test2 publickey:0x2852EE11215574EDD2105E85EBF6B0F84AF86C748BFBF4C9D1083754F4CDF390
      - txnpost id:test2

------------------------------------------------------------
##### Ask for some tokens on minima discord "#app-chat" channel and someone DM you and ask for your address to send 10 minima, once finished the tutorial, do the same to help other people to learn, that's the minima community philosophy, help each other.
------------------------------------------------------------

- Go back to tutorials page: <https://github.com/elledaniels/Minima_Tutorials>

by @JOSUA
