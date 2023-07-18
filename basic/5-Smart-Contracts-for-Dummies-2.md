------------------------------------------------------------
### **Tutorial 5**: Learning Smart Contracts for Dummies
### **- Part 2**: Exchange Contract
------------------------------------------------------------

#### **Exchange Contract**
```
--------------------------------------------------------------
IF SIGNEDBY( TheOwnerPublicKey ) THEN RETURN TRUE ENDIF
ASSERT VERIFYOUT( @INPUT 0xYouAddress 5 0x00 )
RETURN TRUE
--------------------------------------------------------------
```

### **Interpretations of script**:

- 1: This contract says, the owner can cancel at any time, by signing with their key OR you can
spend it if you send 5 Minima ( tokenid 0x00 ) to 0xYourAddress at the opposite output in
the transaction.

- 2: It's an exchange. Say I have 10 tokens, and I want 5 minimas for it. I send the 10 tokens to the script which has my address in it .
The script requires you to send me 5 minimas to spend the 10 tokens I have put in the script.
At the beginning I had 10 tokens and you had 5 minimas.
After the exchange I have 5 minimas and you have 10 tokens.
So it is an exchange. And nothing is left in the script.

- 3: Let's say Bob is the owner of the contract and put his address on it,
Bob deposit 10 Tokens on the contract.
Alice wants those 10 tokens deposited on Bob's contract, so Alice has to send 5 minimas to the Bob's contract where Bob specified his address.
So alice has to build a transaction to meet all the criteria and satisy contract rules in order to get the 10 tokens deposited on the contract.
----------------------------------------------------------------


------------------------ **STEP BY STEP** ----------------------

### Requirements, two nodes representing **BOB** and **ALICE**.

----------------------------------------------------------------
### BOB node:

**0.- Two BOB addresses, one will represent the owner of the script and the other address will represent where to receive the exchanged funds of the script.**

 - **Owner address**
    - "publickey":"0x9C9DD19945DAE6955F6FA628DF0764819B37147F740BAEDF1CBE8969A2D46556",
    - "address":"0xC05BB44D293780DE30BC41134B4A56090DA83287E26E0CA42C51070D49A60422",

 - **Bob address** to receive the exchanged funds from the contract
    - "publickey":"0xFBDA371F05A3C9118DF8342BE9ED2819F7FB20BF90F53BCABB11AD9CF388819B",
    - "address":"0x09FF2B640CE2D0E109BC8888AACD90F1EEE8F8A34C95535447E3B400697E7C78",


**1.- Create the script and write down the response.**

- ->``` scripts action:addscript script:"IF SIGNEDBY(0x9C9DD19945DAE6955F6FA628DF0764819B37147F740BAEDF1CBE8969A2D46556) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT 0x09FF2B640CE2D0E109BC8888AACD90F1EEE8F8A34C95535447E3B400697E7C78 5 0x00) RETURN TRUE" ```

  - **Response**:
      - "response":{
      "publickey":"",
      "script":"IF SIGNEDBY(0x9C9DD19945DAE6955F6FA628DF0764819B37147F740BAEDF1CBE8969A2D46556) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT 0x09FF2B640CE2D0E109BC8888AACD90F1EEE8F8A34C95535447E3B400697E7C78 5 0x00) RETURN TRUE",
      **"address":"0x49A4CE94D28F2D8E878193521957DDA2AAF5A02D21DC70F598A1A61C766F6B4E"**,
      "track":true
      }


  - **Note**:
    - We got the "coinid" after sending funds to the script address, once done, whe use the command ``` coins relevant:true ``` to look for the "coinid" related to script address, which is done in the next steps.
    **"coinid":"0x6F7D76BED3C9CC49363EE162E3466D09A275D463DA385426F5CCB542AA4D2105"**


**2.- Create 10 MyTOKEN to send to script.**

- ->``` tokencreate name:"MyTOKEN" amount:10 decimals:0 ```

  - **Response**:
  **"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"**

**3.- Send 10 MyTOKEN to the script address.**

- ->``` send address:0x49A4CE94D28F2D8E878193521957DDA2AAF5A02D21DC70F598A1A61C766F6B4E amount:10 tokenid:0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011 ```

----------- **BOB node is prepared, the script build and ready to someone to execute his script** ----------------

--------------------------------------------------------------

### ALICE node:

**0. ALICE address with 5 minimas**

- **Address**:
  - "publickey":"0x48F68D3363ABA6CDFF08B2189354C30AA6C83ED64C3C7925B9A704F2EF4652DD",
  - "address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05",
  - "coinid":"0xA042311743B82329454EE4CB4F90CFEE46B07B40D6E14168B5DB1B2E7A36A35F",


**1. ALICE must create the same script as BOB, the **address** will be the same**.

- ->``` scripts action:addscript script:"IF SIGNEDBY(0x9C9DD19945DAE6955F6FA628DF0764819B37147F740BAEDF1CBE8969A2D46556) THEN  RETURN TRUE ENDIF  ASSERT VERIFYOUT(@INPUT 0x09FF2B640CE2D0E109BC8888AACD90F1EEE8F8A34C95535447E3B400697E7C78 5 0x00) RETURN TRUE" ```

**2. ALICE must build a transaction to the BOB's script to exchange 5 Minima by 10 MyTOKEN**.
  - **Note-1**:VERIFYOUT( @INPUT 0xYouAddress 5 0x00 )
    - The @INPUT is returning the index of the input containing the guarded coin and in this script it is used inside
   the VERIFYOUT, so the script is checking the output that has the same index as what the @INPUT returns.
    - So according that paragraph, if ALICE use as first input the **MyTOKEN** coind guarded by BOB script it must use as first output the 5 minima's spent.
  - **Note-2**: !!! **very important** !!!
    - We are spending 5 minimas of ALICE to meet the conditions of BOB's exchange contract, so if you remember from the last tutorial, every transaction generate and UTxo (unspent coin) and it is locked into an script(the hash of it is the address).
    So when we sent 5 minimas from ALICE, these 5 minimas are lock into an script, so when ALICE build the transaction in order to spend the 5 minima it must satisfy the script conditions of these 5 minima, and this condition is simply to sign the transaction with the public key of the address where the 5 minima of ALICE are

 - **Transaction steps:**
```
 ->txncreate id:script1
 ->txninput id:script1 coinid:0x6F7D76BED3C9CC49363EE162E3466D09A275D463DA385426F5CCB542AA4D2105    // Tokens guarded by bob's script
 ->txninput id:script1 coinid:0xA042311743B82329454EE4CB4F90CFEE46B07B40D6E14168B5DB1B2E7A36A35F    // ALICE coins to be exchanged(spent) for BOB's Tokens
 ->txnoutput id:script1 address:0x09FF2B640CE2D0E109BC8888AACD90F1EEE8F8A34C95535447E3B400697E7C78 amount:5  // Send ALICE 5 minimas to BOB's address
 ->txnoutput id:script1 address:0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05 amount:10 tokenid:0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011  // Tokens to Alice wallet

 ->txnsign id:script1 publickey:0x48F68D3363ABA6CDFF08B2189354C30AA6C83ED64C3C7925B9A704F2EF4652DD  //Alice publickey of address where 5 minimas are
 ->txnpost id:script1
```

**ALICE node is prepared, the transaction done and posted,so ALICE should have now 10 MyTokens and BOB should have 5 minimas**

------------------------------------------------------------
**RESPONSE:**
Minima @ 29/04/2022 10:38:43 [71.1 MB] : NEW Spent Coin : {"coinid":"0x6F7D76BED3C9CC49363EE162E3466D09A275D463DA385426F5CCB542AA4D2105","amount":"0.0000000000000000000000000000000000000000001","address":"0x49A4CE94D28F2D8E878193521957DDA2AAF5A02D21DC70F598A1A61C766F6B4E","tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011","token":{"name":{"name":"MyTOKEN"},"coinid":"0x8960B9935C22119ADB39D3E5FBBC0BC69384000638C7E7C43601EBB1A640E5FF","total":"10","decimals":0,"script":"RETURN TRUE","totalamount":"0.0000000000000000000000000000000000000000001","scale":44,"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1077","spent":true,"created":"278858"}
Minima @ 29/04/2022 10:38:43 [71.1 MB] : NEW Spent Coin : {"coinid":"0xA042311743B82329454EE4CB4F90CFEE46B07B40D6E14168B5DB1B2E7A36A35F","amount":"5","address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[],"mmrentry":"1081","spent":true,"created":"278879"}
Minima @ 29/04/2022 10:38:43 [71.2 MB] : NEW Unspent Coin : {"coinid":"0xBF4C3AF586AAE7B9C62508EA5D19E32815C415DCFE1BD319A860F4A478D45413","amount":"0.0000000000000000000000000000000000000000001","address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05","tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011","token":{"name":{"name":"MyTOKEN"},"coinid":"0x8960B9935C22119ADB39D3E5FBBC0BC69384000638C7E7C43601EBB1A640E5FF","total":"10","decimals":0,"script":"RETURN TRUE","totalamount":"0.0000000000000000000000000000000000000000001","scale":44,"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1083","spent":false,"created":"279665"}
Minima @ 29/04/2022 10:38:43 [79.1 MB] : NEW Spent Coin : {"coinid":"0x6F7D76BED3C9CC49363EE162E3466D09A275D463DA385426F5CCB542AA4D2105","amount":"0.0000000000000000000000000000000000000000001","address":"0x49A4CE94D28F2D8E878193521957DDA2AAF5A02D21DC70F598A1A61C766F6B4E","tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011","token":{"name":{"name":"MyTOKEN"},"coinid":"0x8960B9935C22119ADB39D3E5FBBC0BC69384000638C7E7C43601EBB1A640E5FF","total":"10","decimals":0,"script":"RETURN TRUE","totalamount":"0.0000000000000000000000000000000000000000001","scale":44,"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1077","spent":true,"created":"278858"}
Minima @ 29/04/2022 10:38:43 [79.1 MB] : NEW Spent Coin : {"coinid":"0xA042311743B82329454EE4CB4F90CFEE46B07B40D6E14168B5DB1B2E7A36A35F","amount":"5","address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[],"mmrentry":"1081","spent":true,"created":"278879"}
Minima @ 29/04/2022 10:38:43 [79.2 MB] : NEW Unspent Coin : {"coinid":"0xBF4C3AF586AAE7B9C62508EA5D19E32815C415DCFE1BD319A860F4A478D45413","amount":"0.0000000000000000000000000000000000000000001","address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05","tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011","token":{"name":{"name":"MyTOKEN"},"coinid":"0x8960B9935C22119ADB39D3E5FBBC0BC69384000638C7E7C43601EBB1A640E5FF","total":"10","decimals":0,"script":"RETURN TRUE","totalamount":"0.0000000000000000000000000000000000000000001","scale":44,"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1083","spent":false,"created":"279665"}
Minima @ 29/04/2022 10:38:45 [42.7 MB] : NEW Spent Coin : {"coinid":"0x6F7D76BED3C9CC49363EE162E3466D09A275D463DA385426F5CCB542AA4D2105","amount":"0.0000000000000000000000000000000000000000001","address":"0x49A4CE94D28F2D8E878193521957DDA2AAF5A02D21DC70F598A1A61C766F6B4E","tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011","token":{"name":{"name":"MyTOKEN"},"coinid":"0x8960B9935C22119ADB39D3E5FBBC0BC69384000638C7E7C43601EBB1A640E5FF","total":"10","decimals":0,"script":"RETURN TRUE","totalamount":"0.0000000000000000000000000000000000000000001","scale":44,"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1077","spent":true,"created":"278858"}
Minima @ 29/04/2022 10:38:45 [42.7 MB] : NEW Spent Coin : {"coinid":"0xA042311743B82329454EE4CB4F90CFEE46B07B40D6E14168B5DB1B2E7A36A35F","amount":"5","address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05","tokenid":"0x00","token":null,"floating":false,"storestate":true,"state":[],"mmrentry":"1081","spent":true,"created":"278879"}
Minima @ 29/04/2022 10:38:45 [42.8 MB] : NEW Unspent Coin : {"coinid":"0xBF4C3AF586AAE7B9C62508EA5D19E32815C415DCFE1BD319A860F4A478D45413","amount":"0.0000000000000000000000000000000000000000001","address":"0x2C2CD566A2689E1EA4745EE846A6E44019264BDF5E9D2FA0375490DC4CEF4E05","tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011","token":{"name":{"name":"MyTOKEN"},"coinid":"0x8960B9935C22119ADB39D3E5FBBC0BC69384000638C7E7C43601EBB1A640E5FF","total":"10","decimals":0,"script":"RETURN TRUE","totalamount":"0.0000000000000000000000000000000000000000001","scale":44,"tokenid":"0x4560CCF98F5E95686D9801574F4F92B13AB3483F41CECAA6FC0E88CA6017C011"},"tokenamount":"10","floating":false,"storestate":true,"state":[],"mmrentry":"1083","spent":false,"created":"279665"}

------------------------------------------------------------
##### Ask for some tokens on minima discord "#app-chat" channel and someone DM you and ask for your address to send some minima, once finished the tutorial, do the same to help other people to learn, that's the minima community philosophy, help each other.
------------------------------------------------------------

- Go back to tutorials page: <https://github.com/elledaniels/Minima_Tutorials>

by @JOSUA and the help of ----> @mbor and @spartacusrex and @elias (**without his help this tutorial wouldn't have been possible**) <----
