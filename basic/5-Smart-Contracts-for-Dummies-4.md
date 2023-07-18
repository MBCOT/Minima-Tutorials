------------------------------------------------------------
### **Tutorial 5**: Learning Smart Contracts for Dummies:
### **- Part 4**: FLASH LOAN CONTRACT - combinated with two EXCHANGE Contracts
------------------------------------------------------------

#### **Flash Loan Contract** combinated with **Generic Exchange Contract**
```
-------------------------------------------------------------------
IF SIGNEDBY( PREVSTATE(1) ) THEN RETURN TRUE ENDIF
ASSERT SAMESTATE ( 1 1 )
ASSERT VERIFYOUT( @INPUT @ADDRESS @AMOUNT*1.01 @TOKENID )
RETURN TRUE
-------------------------------------------------------------------
```

### **Interpretations of script**:

- 1: This contract says, the owner "PREVSTATE(1)" can cancel at any time, by signing with his key OR you can
spend it if you send the same amount of  Minima or other token locked up into the script plus 1% to the same address on the same transaction.

- 2: It's like a present to be return. Say I will borrow 10 minima from script but in the condition that in the same transaction I have to return the same amount borrowed plus 1%, in this case 10.1 minima.

- 3: Let's say Bety is the owner "PREVSTATE(1)" of the contract (the owner of the deposited funds)",
Bety deposit 10 minima into the contract
Alice wants to borrow those 10 minima deposited on Bety's contract, to do some exchanges with it to win at least 10.1 to satisy the script conditions.
So alice has to build a transaction to meet all the criteria and satisy all contract rules of all the coins/tokens used in order to get the 10 tokens deposited on the contract as well as all other exchanges of tokens to win at least 10.1 minima to satisfy the loan contract on the same transaction.
----------------------------------------------------------------

- So in this tutorial we will learn how to use **Flash Loans** contracts in combination with **Exchange contracts** in the same transaction.
- Alice will use a **Flash Loans** to borrow 10 Minima from Bety, and then Alice will exchange thouse 10 minima by one VIP-TOKEN from Bob, then Alice will exchange her VIP-TOKEN by 30 minima from Peter,
then finally Alice will get back the 10 minima borrowed by that flash loan contract to the contract plus 1%

- Alice is the one who will get borrowed  10 Minima from Bety flash loan contract to exchange for 1 VIP-Token from Bob and then will exchange 1 VIP-TOKEN for 30 minimas from Peter and then will return the borrowed 10 Minimas to Bety's contract plus 1% satisying script condition SAMESATE (1 1) that is bety's publickey.

- **In a single transaction, Alice borrowed 10 minima from a contract and win 20 minima from nothing**. !!! cool !!!

------------------------------------------------------------------

### Lets setup a possible real scenario:


------------------------ **STEP BY STEP** ----------------------

### Requirements,four nodes representing **BETY** and **PETER**,  **BOB** and **ALICE**.

- **We have four actors in this scenario and everyone of them has a mision and a steps to do:**

  - **Alice**:
    - Will borrow funds from Bety's contract (10) minima
    - will exchange those 10 minima for VIP-TOKEN from Bob's contract.
    - will exchange the VIP-TOKEN for 30 minima from Peter's contract
    - will get back 10 minimas to Bety's contract plus a 1%
    - **Alice will get 20 minimas from nothing in a single transaction**
    - -> ``` java -jar minima.jar -data ALICE -port 10001 -rpcenable -rpc 10002 ```
  - **Bob**
    - Bob lock funds(VIP-TOKEN) on the the generic exchange contract
    - Bob use the state variables to send the funds on the contract
    - Bob's locked funds are ready to be exchanged
    - Bob will lock up one VIP-TOKEN to be exchanged by 10 minima
    - Bob will get 10 minimas
    - -> ``` java -jar minima.jar -data BOB -port 11001 -rpcenable -rpc 11002 ```
  - **Peter**
    - Peter lock funds(30 minima) on the the generic exchange contract
    - Peter use the state variables to send the funds on the contract
    - Peter's locked funds are ready to be exchanged
    - Peter will lock up 30 Minima to be exchanged by one VIP-TOKEN
    - Peter will get one VIP-TOKEN
    - -> ``` java -jar minima.jar -data PETER -port 12001 -rpcenable -rpc 12002 ```
  - **Bety**
    - Bety lock funds(10 minima) on the flash loan contract
    - Bety use the state variables to send the funds on the contract
    - Bety's locked funds are ready to be exchanged
    - Bety will get her's 10 minima plus 1% from Alice
    - -> ``` java -jar minima.jar -data BETY -port 13001 -rpcenable -rpc 13002 ```

----------------------------------------------------------------

### 1.- Lets prepare the environmet to build this kind of scenario by steps

- We will use the testnet so everyone can watch and track the contracts and the coins, and also becouse setting it locally on test mode failed, maybe my settings of nodes were wrong.

### Addresses information of every actor

- Use the command ` newaddress ` to create one address for every actor, on a single node or multiples nodes as actors.

  - **Alice**
    - "publickey":"0xE2722E195A512E6C2299799157B25EF0DD8BFC683A51CEE9F3EB588BC0C810FD",
    - "address":"0xBE9A305C5FD472FF3CD030377D44709CAE954C866F8D2D5D3DA0FBF724159EDC",
  - **Bob**
    - "publickey":"0x3C9272E2C71C5DA0950679917F4B5A3DF51C8773A5BF3EC8FFA17D7D8E530491",
    - "address":"0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F",
  - **Peter**
    - "publickey":"0x5DC054827DBE67179B02A446A41BFBCFF0DA01690A106452D6E026027A220436",
    - "address":"0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6",
  - **Bety**
    - "publickey":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA",
    - "address":"0xE2272F0D9F4A724C4D992FB1B1C829E02D47FD040D22D611D011B5FDFA2B9DFC",

### Tokens used on scripts must be created before them

   - Bob: needs to create **100 VIP-TOKEN** but for that he needs some Minima,
   so Bety,first need to ask some minima on the discord , so Bety will send 1 minima to Bob so he can create the 100 VIP-TOKEN
   - Bety send 1 minima to Bob
     - ` ->send address:0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F amount:1 `
   - Bob create 100 VIP-TOKEN
     - ` ->tokencreate name:VIP-TOKEN amount:100 decimals:0
    `**"tokenid":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179"**
   - Peter needs 30 minimas to do his mission, so lets  give him 30 minimas from Bety or anyone
     - ` ->send address:0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6 amount:30 `
   - Bety needs 10 minimas to send to the flash loan contract, so lets give her 10 minima
     - ` ->send address:0xE2272F0D9F4A724C4D992FB1B1C829E02D47FD040D22D611D011B5FDFA2B9DFC amount:10`

### Script to be created

  - Every user must execute one script or several scripts represening his role in this scenario, so every node can track it, for instance:
  - Alice need to know about the scripts about Bety, Bob and Peter so 3 scripts needs to be add to get an address of everyone representing an actor, but Bob and Peter use the same script, so only 2 scripts has to been added.
  - So alice must add the scripts **Flash loans** and **Exchange Contract** to get their addresses, the addresses of those contracts will always be the same on every node of the network.
  - Bob needs to add the **Exchange Contract**
  - Peter needs to add the **Exchange Contract**
  - Bety needs to add the **Flash Loan Contract**
  - Every script(When sending funds to them) needs to be set up according to every actor's role using **state variables**.
  - **Write down** all the info and addresses so you will need later.
  - Look over **APENDIX A**, to get some points about tracking coins from @spartacusrex

  - **Bob**:
  He needs to add the **exchange's contract** to his node to kwnow the address of the contract where to send the funds.
    - ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(0) ) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT  PREVSTATE(1) PREVSTATE(2) PREVSTATE(3)) RETURN TRUE" ```
    - Response:
    - "response":{
	  "publickey":"",
	  "script":"IF SIGNEDBY(PREVSTATE(0)) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT PREVSTATE(1)PREVSTATE(2)PREVSTATE(3)) RETURN TRUE",
	  **"address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6"**,
	  "track":true
	  }

  - **Peter**:
  He needs to add the **exchange's contract** to his node to kwnow the address of the contract where to send the funds and track the script
    - ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(0) ) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT  PREVSTATE(1) PREVSTATE(2) PREVSTATE(3)) RETURN TRUE" ```
    - Response is the same address bob's script
    - **"address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6"**

  - **Bety**:
  He needs to add the **flash loan's contract** to his node to kwnow the address of the contract and lock the funds, 10 minima.
    - ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(1) ) THEN RETURN TRUE ENDIF ASSERT SAMESTATE ( 1 1 ) ASSERT VERIFYOUT( @INPUT @ADDRESS @AMOUNT*1.01 @TOKENID ) RETURN TRUE" ```
    - Response:
    - "response":{
    "publickey":"",
    "script":"IF SIGNEDBY(PREVSTATE(1)) THEN  RETURN TRUE ENDIF  ASSERT SAMESTATE(1 1) ASSERT VERIFYOUT(@INPUT @ADDRESS @AMOUNT*1.01 @TOKENID) RETURN TRUE",
    **"address":"0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1"**,
    "track":true

  - **Alice**:
  He needs to add the **flash loan's contract** to his node to kwnow the address of the contract where to borrow the funds and track the script as well as **exchange contract** to let track Bob and Peter coins locked on the exchange's contract

    - ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(1) ) THEN RETURN TRUE ENDIF ASSERT SAMESTATE ( 1 1 ) ASSERT VERIFYOUT( @INPUT @ADDRESS @AMOUNT*1.01 @TOKENID ) RETURN TRUE" ```
    - Response: the same address as Bety's script
    - **"address":"0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1"**

    - ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(0) ) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT  PREVSTATE(1) PREVSTATE(2) PREVSTATE(3)) RETURN TRUE" ```
    - Response is the same address bob's and Peter's script
    - **"address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6"**

### Send the funds to the contracts to lock them up with the state variables for every actor.

  - **NOTE**: The response shown after the send command are just the first lines of the whole response, so if you want to look for it, you will need to scroll back just after the command "send" is executed.

  - **Bob**:
  Will lock up 1 token **VIP-TOKEN** sending them to the Exchange contract address , setting the right state variables that the script needs.
    - ->``` send address:0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6 amount:1 tokenid:0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179 state:{"0":"0x3C9272E2C71C5DA0950679917F4B5A3DF51C8773A5BF3EC8FFA17D7D8E530491","1":"0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F","2":"10","3":"0x00"} ```  

    - Response:
    - params":{
    "address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6",
    "amount":"1",
    "tokenid":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179",
    "state":{
    "0":"0x3C9272E2C71C5DA0950679917F4B5A3DF51C8773A5BF3EC8FFA17D7D8E530491",
    "1":"0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F",
    "2":"10",
    "3":"0x00"
    }
    **"coinid":"0xBD69AFC56B4AF16B17F1DCC5C9E357FE116498A773173F213F74A04251449C5C"**

  - **Peter**,
  will lock up 30 minima sending it to the **Exchange script's address** , setting the right state variables
    - ->``` send address:0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6 amount:30 state:{"0":"0x5DC054827DBE67179B02A446A41BFBCFF0DA01690A106452D6E026027A220436","1":"0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6","2":"1","3":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179"} ```

    - Response:
    - "params":{
    "address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6",
    "amount":"30",
    "state":{
    "0":"0x5DC054827DBE67179B02A446A41BFBCFF0DA01690A106452D6E026027A220436",
    "1":"0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6",
    "2":"1",
    "3":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179"
    }
    **"coinid":"0x4718F34A95627A22503D4F6F60FC5B974BE480DBF969D61D7E7F824DB3B43A6E"**  

  - **Bety**,
  Will lock up 10 minima sending it to the **Flash Loan script's address**, setting the right state variables
    - ->```  send address:0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1 amount:10 state:{"1":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA"} ```

    - Response:
    - "params":{
    "address":"0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1",
    "amount":"10",
    "state":{
    "1":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA"
    }
    **"coinid":"0xB0105BDEF1012ECA3C598E8E65EB7155721B9A453A9A53868686B847D977BAD9"**

###    ----------- The scenario is ready, let's fire it up building the Alice transaction to run all of it ----------------


--------------------------------------------------------------

**Note**:
Before we build the transaction, we are gonna put all info of every actor and scripts together, so it will be easy to follow
the code and check every data.

- **Alice**:
  - "publickey":"0xE2722E195A512E6C2299799157B25EF0DD8BFC683A51CEE9F3EB588BC0C810FD",
  - "address":"0xBE9A305C5FD472FF3CD030377D44709CAE954C866F8D2D5D3DA0FBF724159EDC",
- **Bob**
  - "publickey":"0x3C9272E2C71C5DA0950679917F4B5A3DF51C8773A5BF3EC8FFA17D7D8E530491",
  - "address":"0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F",
  - **"tokenid":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179"**
  - **"coinid":"0xBD69AFC56B4AF16B17F1DCC5C9E357FE116498A773173F213F74A04251449C5C"**
- **Peter**
  - "publickey":"0x5DC054827DBE67179B02A446A41BFBCFF0DA01690A106452D6E026027A220436",
  - "address":"0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6",
  - **"coinid":"0x4718F34A95627A22503D4F6F60FC5B974BE480DBF969D61D7E7F824DB3B43A6E"**
- **Bety**
  - "publickey":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA",
  - "address":"0xE2272F0D9F4A724C4D992FB1B1C829E02D47FD040D22D611D011B5FDFA2B9DFC",
  - **"coinid":"0xB0105BDEF1012ECA3C598E8E65EB7155721B9A453A9A53868686B847D977BAD9"**
- **Echange Contract**:
  - **"address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6"**,
- **Flash Loan contract**:
  - **"address":"0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1"**
---------------------------------

### Template

```
->txncreate id:flash1

->txninput id:flash1 coinid:Bety script's funds 10 minima  // Tokens guarded by Bety's script ----FLASH LOAN contract

->txninput id:flash1 coinid:BOB script's funds  VIP-TOKEN  // Tokens guarded by bob's script  ----EXCHANGE contract

->txninput id:flash1 coinid:PETER script's fund  30 Minima // Tokens guarded by Peter's script  ----EXCHANGE constract

->txnstate id:flash1 port:1 value:"PREVSTATE(1)"           // To satisy Bety's Flash loan contract condition SAMESTATE(1 1)

->txnoutput id:flash1 address:Bety address amount:10*1.01 tokenid:0x00   // Tokens to payback the loan to Bety's wallet

->txnoutput id:flash1 address:Bob address  amount:10                     // Send 10 minimas to BOB's address, as script says

->txnoutput id:flash1 address:Peter address amount:1 tokenid:VIP-TOKEN   // Tokens of Bob's script to Peter's address

->txnoutput id:flash1 address:Alice address amount:19.9                  // **19.9 minimas earned** Tokens earned to Alice

->txnpost id:flash1
```

**!!! Atention !!!**
  - "@INPUT" parameter of the instruction VERIFYOUT of any script must be taken in consideration, as it dictate the order to use as **it is linked with its inputs represented by "coinid"**.

  - What I mean, is that first input using a "coinid"(Contract locked coin) must satisfy that, in the first ouput the script conditions are satisyed to spend the "coind"
  - And the same for the next "coind", if it is on the second input, then on the second output, the script of this coind needs its coditions be satisfyed.
  - **So the order of the input position in a transaction for a coind(script),must require an output at the same position number that the input in a transaction, otherwise the script will fail.**

  - **State Variables** can't overlap script's state variables and you can stop an output to store the state variable with the flag **storestate:false**


### Implementation

-According the former atention note, in this case the input/output relantionship of every coind locked funds, and its **position in the transaction** would be:
  - 1 input Betys contract coinid -> 1 output satisy bety's conctract  (Flash Loan)
  - 2 input Bob's contract coind -> 2 output satisy bob's contract  (Exchange)
  - 3 input Per'ers contract coind -> 3 output satisy bob's contract  (Exchange)
```
 ->txncreate id:flash1

 ->txninput id:flash1 coinid:0xB0105BDEF1012ECA3C598E8E65EB7155721B9A453A9A53868686B847D977BAD9   // Tokens guarded by Bety's  script ----FLASH LOAN contract

 ->txninput id:flash1 coinid:0xBD69AFC56B4AF16B17F1DCC5C9E357FE116498A773173F213F74A04251449C5C   // Tokens guarded by bob's script  ----EXCHANGE contract

 ->txninput id:flash1 coinid:0x4718F34A95627A22503D4F6F60FC5B974BE480DBF969D61D7E7F824DB3B43A6E // Tokens guarded by Peter's script  ----EXCHANGE constract

 ->txnstate id:flash1 port:1 value:0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA  // To satisy Bety's Flash loan contract condition SAMESTATE(1 1)

 ->txnoutput id:flash1 address:0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1 amount:10.1   // Tokens to script address of flash loan contract

->txnoutput id:flash1 address:0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F  amount:10  // 10 minima of ALICE to Bob's address  

->txnoutput id:flash1 address:0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6 amount:1 tokenid:0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179    // Tokens of Bob's script to Peter's address

->txnoutput id:flash1 address:0xBE9A305C5FD472FF3CD030377D44709CAE954C866F8D2D5D3DA0FBF724159EDC amount:19.9  // Tokens earned with the exchanges of items go back to Alice's wallet

->txnpost id:flash1
```

- **ALICE node is prepared, the transaction done and posted.**
- **ALICE should have now 19.9 minima.**
- **BOB should have 10 minima.**
- **PETER should have one VIP-TOKEN**
- **BETY Flash Loan contract should have 10.1 minima with bety's publick key settup.**

------------------------------------------------------------
**RESPONSE:**
Minima @ 07/05/2022 17:55:29 [66.8 MB] : NEW Spent Coin : {"coinid":"0xB0105BDEF1012ECA3C598E8E65EB7155721B9A453A9A53868686B847D977BAD9","amount":"10","address":"0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[{"port":1,"type":1,"data":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA","keeper":true}],"mmrentry":"1114","spent":true,"created":"293974"}
Minima @ 07/05/2022 17:55:29 [66.9 MB] : NEW Spent Coin : {"coinid":"0xBD69AFC56B4AF16B17F1DCC5C9E357FE116498A773173F213F74A04251449C5C","amount":"0.00000000000000000000000000000000000000000001","address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6","tokenid":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179","token":{"name":{"name":"VIP-TOKEN"},"coinid":"0xF1297D7D12DB1E71C360CD36D3388BD3C9B0D26816726C76B53D98FFFEFE70B3","total":"100","decimals":0,"script":"RETURN TRUE","totalamount":"0.000000000000000000000000000000000000000001","scale":44,"tokenid":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179"},"tokenamount":"1","floating":false,"storestate":true,"state":[{"port":0,"type":1,"data":"0x3C9272E2C71C5DA0950679917F4B5A3DF51C8773A5BF3EC8FFA17D7D8E530491","keeper":true},{"port":1,"type":1,"data":"0xD5C8B8A4579F98992A0823E00D93660C1181C7BFA8BB4B451D6017906AC0CE2F","keeper":true},{"port":2,"type":2,"data":"10","keeper":true},{"port":3,"type":1,"data":"0x00","keeper":true}],"mmrentry":"1106","spent":true,"created":"293431"}
Minima @ 07/05/2022 17:55:29 [67.0 MB] : NEW Spent Coin : {"coinid":"0x4718F34A95627A22503D4F6F60FC5B974BE480DBF969D61D7E7F824DB3B43A6E","amount":"30","address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[{"port":0,"type":1,"data":"0x5DC054827DBE67179B02A446A41BFBCFF0DA01690A106452D6E026027A220436","keeper":true},{"port":1,"type":1,"data":"0x944E04A541CFC9A6490E49C4524744AA015642B77BF9085B123DDE31C909E4A6","keeper":true},{"port":2,"type":2,"data":"1","keeper":true},{"port":3,"type":1,"data":"0x1A4F5F0697ABB7BBB0CF442D068A5D3B7C3B112838E63521C01AE705DE6A5179","keeper":true}],"mmrentry":"1108","spent":true,"created":"293439"}
Minima @ 07/05/2022 17:55:29 [67.0 MB] : NEW Unspent Coin : {"coinid":"0xB71524B303AC1295A062C0086D840CC7B3983AF7456549079D9F9E6D0A7FB474","amount":"10.1","address":"0x5C265458A180BB63746B6460310DA7B8757D7ABD062C73638660FE6A1228BEF1","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[{"port":1,"type":1,"data":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA","keeper":true}],"mmrentry":"1115","spent":false,"created":"293991"}
Minima @ 07/05/2022 17:55:29 [67.1 MB] : NEW Unspent Coin : {"coinid":"0x8DB405C91B02639BE3E28E0A200EBE0621648F318C4636B283A9331BF5EC7434","amount":"19.9","address":"0xBE9A305C5FD472FF3CD030377D44709CAE954C866F8D2D5D3DA0FBF724159EDC","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[{"port":1,"type":1,"data":"0xA68C475DB33F79A6D2FAA3ACCB77027F61D5A6CABA6124C8CE41748A6DC5DBFA","keeper":true}],"mmrentry":"1118","spent":false,"created":"293991"}

**APPENDIX A**:
- @spartacusrex points about tracking coins:

![Image text](https://github.com/elledaniels/Minima_Tutorials/blob/main/basic/images/tutorial-5-4/spartacus_points-tracking-coins.png)
------------------------------------------------------------
##### Ask for some tokens on minima discord "#app-chat" channel and someone DM you and ask for your address to send some minima, once finished the tutorial, do the same to help other people to learn, that's the minima community philosophy, help each other.
------------------------------------------------------------

- Go back to tutorials page: <https://github.com/elledaniels/Minima_Tutorials>

by @JOSUA and the help of:
@mbor
@spartacusrex
@elias and the minima team (**without their help this tutorial wouldn't have been possible**) <----
