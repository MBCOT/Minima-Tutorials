------------------------------------------------------------
### **Tutorial 5**: Learning Smart Contracts for Dummies:
### **- Part 3**: GENERIC EXCHANGE CONTRACT - State Variables
------------------------------------------------------------

#### **Generic Exchange Contract**
```
--------------------------------------------------------------
IF SIGNEDBY( PREVSTATE(0) THEN RETURN TRUE ENDIF
ASSERT VERIFYOUT( @INPUT PREVSTATE (1) PREVSTATE(2) PREVSTATE(3) )
RETURN TRUE
--------------------------------------------------------------
```

### **Interpretations of script**:

- 1: This contract says, the owner "PREVSTATE(0)" can cancel at any time, by signing with his key OR you can
spend it if you send an amount of "PREVSTATE(2)" of Minima or other token indicated by tokenid "PREVSTATE(3)" to 0xYourAddress indicated by "PREVSTATE (1)" at the opposite output in
the transaction.

- 2: It's an exchange. Say I have 10 tokens , and I want a quantity of "PREVSTATE(2)" minimas or other token "PREVSTATE(3)" for it.
I send the 10 tokens to the script which has my address "PREVSTATE(1)" in it .
The script requires you to send me a quantity of "PREVSTATE(2)" minimas or other token "PREVSTATE(3)" to spend the 10 tokens I have put in the script.
At the beginning I had 10 tokens and you had 5 minimas.
After the exchange I have 5 minimas and you have 10 tokens.
So it is an exchange. And nothing is left in the script.

- 3: Let's say Bob is the owner "PREVSTATE(0)" of the contract and put his address on it "PREVSTATE(1)",
Bob deposit 10 Tokens on the contract.
Alice wants those 10 tokens deposited on Bob's contract, so Alice has to send a quantity (PREVSTATE(2)) to the address "PREVSTATE(1)" of minimas or other token indicated by "PREVSTATE(3)" to the Bob's contract where Bob specified his address "PREVSTATE(1)".
So alice has to build a transaction to meet all the criteria and satisy contract rules in order to get the 10 tokens deposited on the contract.
----------------------------------------------------------------


------------------------ **STEP BY STEP** ----------------------

### Requirements, two nodes representing **BOB** and **ALICE**.

----------------------------------------------------------------
### BOB node:

**0.- Two BOB addresses, one will represent the owner of the script and the other address will represent where to receive the exchanged funds of the script.**

 - **Owner address**
    - publickey":"0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93",
    - "address":"0x8C907169F08A4B7CF2BA9A5B8B2CC18E459213B267E2645B52C9F90EA08CA8C7"


 - **Bob address** to receive the exchanged funds from the contract
    - "publickey":"0x358EF3D9AF146EBF30EC576CDDD68D6AE64F9FD190BEF0434711A768ABECBB37",
    - "address":"0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A",


**1.- Create the script and write down the response.**

- ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(0) ) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT  PREVSTATE(1) PREVSTATE(2) PREVSTATE(3)) RETURN TRUE" ```

  - **Response**:
      - "response":{
      "publickey":"",
      "script":"IF SIGNEDBY(PREVSTATE(0)) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT PREVSTATE(1)PREVSTATE(2)PREVSTATE(3)) RETURN TRUE",
      **"address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6",**
      "track":true
      }
      }

  - **Note**:
    - We got the "coinid" after sending funds to the script address, once done, whe use the command ``` coins relevant:true ``` to look for the "coinid" related to script address, which is done in the next steps.
    **"coinid":"0xA7A7A32D8EE63C867F784E2A864A618A149349A07C2EA7521EE03D17694157D2"**


**2.- Create 200 MyTOKEN.**

- ->``` tokencreate name:"MyTOKEN" amount:200 decimals:0 ```

  - **Response**:
  **"tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2"**

**3.- Send 10 MyTOKEN to the script address using State Variables.**

  - **What are state variables ?**
  State variables are information that you can store in the transaction as a variables, a variable is stored in a "position", the positions can go from 0 to 255.
  That information stored as a variables will be available to be used on the next transaction by scritps, and also you can watch those variables if you execute the command: ` coins relevant:true ` and look for the address where you sent the coin/token with the state variables setted.

  - State variables on the send command uses the JSON format {"position 0":"information", "position 1":"information"}
  - If you look for ` send ` command sintaxis it tells you that you need to use the parameter **state:** and the JSON format of state variables as you can see in the next command:

  - ->``` send address:0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6 amount:10 tokenid:0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2 state:{"0":"0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93","1":"0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A","2":"10","3":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2"} ```

  - **Response**
    - "command":"send",
    - params":{
    - "address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6",
    - "amount":"10",
    - "tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2",
    - "state":{
      - "0":"0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93",
      - "1":"0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A",
      - "2":"10",
      - "3":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2"
    - }

**4.- Send coins/tokens manually with state variables building a transaction. (optional)**
  - If you followed step 3, you don't need to do this, it is just an example in case there is a situation that you cannnot set state variables using ` send ` command and you have to set the state variables when building a transaction manually.

  - **Transaction send manual state variables:**
```  
  ->txncreate id:send
  ->txninput id:send coinid: the coind that holds the funds to send to the script
  ->txnoutput id:send address: "the script address" amount:10 tokenid: "the token id that represent the tokens to send"
  ->txnoutput id:send address: "coinid address" amount: "change back if the coinid holds more than 10 tokens" tokenid: "tokenid"

  ->txnstate id:send port:0 value:"owner public key"
  ->txnstate id:send port:1 value:"owner address"
  ->txnstate id:send port:2 value:5   "// Alice amount to send to get the 10 tokens"
  ->txnstate id:send port:3 value:"the token id  // Alice tokenid that will use when sending the funds"

  ->txnsign id:send publickey:auto  "the publickey of the coind that holds the funds to be spent sending to the script"
  ->txnpost id:send
```

**4.- Cancel the contract building a transaction and getting back the funds locked into the script.**
  - In case you don't want the contract to be executed anymore and want your funds back we need to build a custom transaction to cancel the contract:
```  
  ->txncreate id:return2
  ->txninput id:return2 coinid:0xA7A7A32D8EE63C867F784E2A864A618A149349A07C2EA7521EE03D17694157D2 //locked into script
  ->txnoutput id:return2 address:0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A amount:10 tokenid:0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2  //send to owner (prevstate(1))
  ->txnsign id:return2 publickey:0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93  //signed by owner prevstate(0)
  ->txnpost id:return2
```

**5.- Prepare the conditions to lock BOB's tokens up and let ALICE to exchange BOB's funds by 5 minimas of hers**
  - If you have notice, what we have been doing so far in this tutorial, is setup a generic script that can be customized by state variable to meet anyone conditions to lock up funds and exchange them with others.
  - If you look closer to the previous part when we set up the script (**step 3**) using ` send ` command and state variables, that part said: that BOB locked 10 MyTOKENS, and the state variables configured the script to exchange the BOB's tokens by other 10 tokens MyTOKEN, so it is nonsense, as the same amount of the same tokens locked up are exchanged for the same.
  But that was done to let the owner **cancel the contract** and get his funds back and show you that with generic stripts and state variables you can configure them the way you want.

  - So now, we are going to set up the conditions to exchange the 10 MyTOKEN's BOB locked up by the ALICE 5 minimas, so in the end, ALICE will get 10 MyTOKEN and BOB will get 5 minimas for his locked funds.
  - To do that we only need to repeat the **step 3** with different state variables to configure the generic script, so here you go:

  - ->``` send address:0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6 amount:10 tokenid:0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2 state:{"0":"0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93","1":"0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A","2":"5","3":"0x00"} ```

  - **Response** (first lines after hiting enter the console prompt)
    - "command":"send",
    - "params":{
    - "address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6",
    - "amount":"10",
    - "tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2",
    - "state":{
      - "0":"0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93",
      - "1":"0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A",
      - "2":"5",
      - "3":"0x00"
    }

  - Now we need to know the coinid of the locked funds into the script so we run the command ` coins relevant:true ` and we find the info: **"coinid":"0x7F9CD4C8549BA1563174512B73A8ACB3832C60D9C6DEC1F80F6098D4F2869182"** this coinid is the one that ALICE will need to use when she build the transaction to execute the script to exchange her 5 minimas by 10 BOB's Tokens.

----------- **BOB node is prepared, the script build and ready to someone to execute his script** ----------------

--------------------------------------------------------------

### ALICE node:

**0. ALICE address with 5 minimas**

- **Address**:
  - "publickey":"0x1F6943C5032EBC8B199C019C9BDA4CA009F5B16B27D6AF6158266B4526C0A482"
  - "address":"0x0740957606C3D442F18DAE5907393CE71189868DF51D521692971F9608007D77"
  - "coinid":"0xF75912A7BF7DD09A9B354044B7D40208E6A2B0F3B872AAB2E2CF3708E299E4F9" (5 minimas)


**1. ALICE must create the same script as BOB, the **address** will be the same as BOB's script**.

- ->``` scripts action:addscript script:"IF SIGNEDBY( PREVSTATE(0) ) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT  PREVSTATE(1) PREVSTATE(2) PREVSTATE(3)) RETURN TRUE" ```

**2. ALICE must build a transaction to the BOB's script to exchange 5 Minima by 10 MyTOKEN**.
  - **Note-1**:VERIFYOUT( @INPUT PREVSTATE(1) PREVSTATE(2) PREVSTATE(3) )
    - The @INPUT is returning the index of the input containing the guarded coin and in this script it is used inside
   the VERIFYOUT, so the script is checking the output PREVSTATE(1)=BOB's address that has the same index as what the @INPUT returns.
    - So according that paragraph, if ALICE use as first input the **MyTOKEN** coind guarded by BOB script it must use as first output the 5 minima's(PREVSTATE(2)) spent.
  - **Note-2**: !!! **very important** !!!
    - We are spending 5 minimas of ALICE to meet the conditions of BOB's exchange contract, so if you remember from the last tutorial, every transaction generate and UTxo (unspent coin) and it is locked into an script(the hash of it is the address).
    So when we sent 5 minimas from ALICE, these 5 minimas are lock into an script, so when ALICE build the transaction in order to spend her 5 minima it must satisfy the script conditions of these 5 minima, and this condition is simply to sign the transaction with the public key of the address where the 5 minima of ALICE are

 - **Transaction steps:**
```
 ->txncreate id:script1
 ->txninput id:script1 coinid:0x7F9CD4C8549BA1563174512B73A8ACB3832C60D9C6DEC1F80F6098D4F2869182    // Tokens guarded by bob's script
 ->txninput id:script1 coinid:0xF75912A7BF7DD09A9B354044B7D40208E6A2B0F3B872AAB2E2CF3708E299E4F9    // ALICE coins to be exchanged(spent) for BOB's Tokens
 ->txnoutput id:script1 address:0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A amount:5  // Send ALICE 5 minimas to BOB's address contract
 ->txnoutput id:script1 address:0x0740957606C3D442F18DAE5907393CE71189868DF51D521692971F9608007D77 amount:10 tokenid:0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2  // Tokens of the script to Alice wallet

 ->txnsign id:script1 publickey:0x1F6943C5032EBC8B199C019C9BDA4CA009F5B16B27D6AF6158266B4526C0A482  //Alice publickey of address where 5 minimas are
 ->txnpost id:script1
```

**ALICE node is prepared, the transaction done and posted,so ALICE should have now 10 MyTokens and BOB should have 5 minimas**

------------------------------------------------------------
**RESPONSE:**
Minima @ 04/05/2022 11:47:49 [37.3 MB] : NEW Spent Coin : {"coinid":"0x7F9CD4C8549BA1563174512B73A8ACB3832C60D9C6DEC1F80F6098D4F2869182","amount":"0.0000000000000000000000000000000000000000001","address":"0x2C5D936A2BBA7284F0F30CDF26086B14169805FC0A15931D7A3D7110F38B9ED6","tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2","token":{"name":{"name":"MyTOKEN"},"coinid":"0xB1ACA8EEE7BEBCFD3832321E6EF8E5BD1C6795DDA0EF3DC1522AB13ED6D3D8AF","total":"200","decimals":0,"script":"RETURN TRUE","totalamount":"0.000000000000000000000000000000000000000002","scale":44,"tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2"},"tokenamount":"10","floating":false,"storestate":true,"state":[{"port":0,"type":1,"data":"0x8220F42636CD5F4A1E836F35ED4DB6B71A54158A327AD863112CE726D76FEB93","keeper":true},{"port":1,"type":1,"data":"0x2178C79E068D133583F6A9CE9572C4E54C6FDC9E3987D56B3064395F5B88095A","keeper":true},{"port":2,"type":2,"data":"5","keeper":true},{"port":3,"type":1,"data":"0x00","keeper":true}],"mmrentry":"1093","spent":true,"created":"288290"}
Minima @ 04/05/2022 11:47:49 [37.3 MB] : NEW Spent Coin : {"coinid":"0xF75912A7BF7DD09A9B354044B7D40208E6A2B0F3B872AAB2E2CF3708E299E4F9","amount":"5","address":"0x0740957606C3D442F18DAE5907393CE71189868DF51D521692971F9608007D77","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[],"mmrentry":"1094","spent":true,"created":"288307"}
Minima @ 04/05/2022 11:47:49 [37.4 MB] : NEW Unspent Coin : {"coinid":"0xE1297E9A0206318B550A3787316990C702EDE4CD9CB8027EE5BD4176D604C9CB","amount":"0.0000000000000000000000000000000000000000001","address":"0x0740957606C3D442F18DAE5907393CE71189868DF51D521692971F9608007D77","tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2","token":{"name":{"name":"MyTOKEN"},"coinid":"0xB1ACA8EEE7BEBCFD3832321E6EF8E5BD1C6795DDA0EF3DC1522AB13ED6D3D8AF","total":"200","decimals":0,"script":"RETURN TRUE","totalamount":"0.000000000000000000000000000000000000000002","scale":44,"tokenid":"0xE84646800BFD1C91261195EB2C6D019047EBFD129C1C6F58FD0C0E0294E71AB2"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1097","spent":false,"created":"288403"}


------------------------------------------------------------
##### Ask for some tokens on minima discord "#app-chat" channel and someone DM you and ask for your address to send some minima, once finished the tutorial, do the same to help other people to learn, that's the minima community philosophy, help each other.
------------------------------------------------------------

- Go back to tutorials page: <https://github.com/MBCOT/Minima-Tutorials>

by @JOSUA and the help of ----> @mbor and @spartacusrex and @elias (**without his help this tutorial wouldn't have been possible**) <----
