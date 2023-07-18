# Minima-tutorials
A series of Minima blockchain tutorials, from basic to complex

### **Tutorial 1: Understand UTXO model**
  - This tutorial will show you how the UTXO model works on minima, you will be able to create a wallet address and send minima coins between two nodes on their wallet addresses using terminal minima command line console.
  - It can be done using one computer running two nodes, two different comupters running one node each one or two mobiles.
  - You need 1 minima to do the tutorial, so ask someone to send 1 minima to you on the channel minima discord channel "#app-chat", to your wallet address.
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/1-UTXO-create-send-Tutorial.md>

### **Tutorial 2: Send Tokens**
  - This tutorial will show you how to send tokens, you will be able to locate the _tokenid_  ,needed to send tokens between two nodes on their wallet addresses using terminal minima command line console.
  - It can be done using one computer running two nodes, two different comupters running one node each one or two mobiles.
  - You need some tokens to do the tutorial, so ask someone to send some tokens to you on the channel minima discord channel "#app-chat", to your wallet address.
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/2-Send-Tokens-Tutorial.md>

### **Tutorial 3: Create Token  part 1**
  - This is the first part of two on how to use minima terminal commands to teach and explain how to create Tokens using minima coins, and also the first part to introduce what transaction are and how to check and understand the console output response to the create token command.
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/3-Create_token-tutorial-1.md>

### **Tutorial 4: Create Token  part 2**
  - This is the second part on how to use **tokencreate** minima terminal command to teach and explain how to create Tokens using minima coins, but using extra parameters to add more info to the Token using the JSON structure to give more info than a simple name.
  - We are going to create 1 token that will represent power token in a very simple game, a game made with **GODOT engine** that will react when the node where the game is running receive one of those power tokens in the wallet address of the node, making the game react and changing the color and the size of the player with the properties of the token's power received.
  - We will use a new terminal command: **coins relevant:true**
  - we will use a new termimal command on a mobile devices: **rpc enable:true**
  - We will user transactions but not directly to avoid UTXO "problems" that we saw on the tutorial-1.
  - The app-game we are going to use has:
    - **A Game** where we will see how to change the player's color and size when a token it is send to the minima node, where a player can collect gems, the game steals you a 0.01 minima or 0.1 minima depending on the collected gem.
    - **A Terminal** like minima terminal where you can execute any minima command like(tokencreate) and copy the results of the execution of the commands.
    - **A Splitter** function that will split 1 minima into small units of 0.1 or 0.01 units with only one transaction to avoid UTXO "problems", as every time you collect a gem a terminal send command is executed with an amount of 0.1 or 0.01 minimas.
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/4-Create_token-tutorial-2.md>

### Tutorial 5-1: Smart Contract for dummies
### **- Part 1**: Transactions and Scripts
  - This is the part 1 of several where we try to explain what are smart contracts and how do deal with them step by step, what is called tutorials for dummies :-)
  - So in this tutorial will learn how to do a transaction, automatically with the "send" command and manually using minima commands about transactions, and also we will learn what is a script and focus on concepts that will be neccesary to understand in order to succeed with Smart Contracts.
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/5-Smart-Contracts-for-Dummies-1.md>

### Tutorial 5-2: Smart Contract for dummies
### **- Part 2**: EXCHANGE CONTRACT
  - This is the part 2 of several where we try to explain what are smart contracts and how do deal with them step by step.
  - So in this tutorial will learn how to do our first script, step by step and pointing out all important things and concepts needed to do it successfully, as there are a lot of things that can fail when dealing with scripts.
  We will build an Exchange Contract, someone will give us 5 minimas and we in return will give him 10 Tokens.

  #### **Exchange Contract**
  ```
  --------------------------------------------------------------
  IF SIGNEDBY( TheOwnerPublicKey ) THEN RETURN TRUE ENDIF
  ASSERT VERIFYOUT( @INPUT 0xYouAddress 5 0x00 )
  RETURN TRUE
  --------------------------------------------------------------
  ```
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/5-Smart-Contracts-for-Dummies-2.md>

### Tutorial 5-3: Smart Contract for dummies
### **- Part 3**: GENERIC EXCHANGE CONTRACT - State Variables
  - This is the part 3 of several where we try to explain what are smart contracts and how do deal with them step by step.
  - So in this tutorial will learn how to modify our specific echange contract done in the previous tutorial and modify it to be generic so everyone will be able to use with the help of **State variables**.
  - **1.** We will learn how set the **State Variables** using the ` send ` commands to lock funds into the script.
  - **2.** We will learn how to set the **State Variables** manually when building a transaction using ` txnstate ` command to lock funds into the script manually (send + state variables done manually)
  - **3.** We will learn howto build a transaction to **cancel the contract** so the owner can withdraw his locked funds.
  - **4.** We will learn howto ALICE can build a transaction to execute this generic exchange contract.


#### **Generic Exchange Contract**
```
--------------------------------------------------------------
IF SIGNEDBY( PREVSTATE(0) THEN RETURN TRUE ENDIF
ASSERT VERIFYOUT( @INPUT PREVSTATE (1) PREVSTATE(2) PREVSTATE(3) )
RETURN TRUE
--------------------------------------------------------------
```
- <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/5-Smart-Contracts-for-Dummies-3.md>

### Tutorial 5-4: Smart Contract for dummies
### **- Part 4**: FLASH LOAN CONTRACT - combinated with two EXCHANGE Contracts
  - So in this tutorial we will learn how to use **Flash Loans** contracts in combination with **Exchange contracts** in the same transaction.
  - Alice will use a **Flash Loans** to borrow 10 Minima from Bety, and then Alice will exchange those 10 minima by one VIP-TOKEN from Bob, then Alice will exchange her VIP-TOKEN by 30 minima from Peter,
  then finally Alice will get back the 10 minima borrowed by that flash loan contract to the contract plus 1%
  - **In a single transaction, Alice borrowed 10 minima from a contract and win 20 minima from nothing**.
  - !!! cool !!! contract.

  #### **Generic Flash Loan Contract**
  ```
  -------------------------------------------------------------------
  IF SIGNEDBY( PREVSTATE(1) ) THEN RETURN TRUE ENDIF
  ASSERT SAMESTATE ( 1 1 )
  ASSERT VERIFYOUT( @INPUT @ADDRESS @AMOUNT*1.01 @TOKENID )
  RETURN TRUE
  -------------------------------------------------------------------
  ```
  - <https://github.com/MBCOT/Minima_Tutorials/blob/main/basic/5-Smart-Contracts-for-Dummies-4.md>

### **Tutorial 6: Create Simple DAO**
  - This is the first part of several where we try to build a simpleDAO (People with a governance token) will have the power to vote a proposal (First part will be a fixed proposal).
  - We will use Tokens, smart contracts, some html, javascript or GODOT prefixed environment to test it.
    - **--------->  UNDER CONSTRUCTION  <-------------**

- by @JOSUA  -
