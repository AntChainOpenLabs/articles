<!--
 *@Author:reborncd
-->
<!--
 *@Author:reborncd
-->
# Attack Analysis on Linear Finance

<font face="Song style">

On September 22, 2023, Linear Finance—a cross-chain asset protocol—reported a malicious attack from the previous day. The attacker managed to mint an unlimited amount of lAAVE and then exchange them for lUSD on the Linear Exchange. This attack resulted in an absolute depletion of lUSD liquidity on PancakeSwap and Ascendex, causing lUSD's value to plummet to zero. 
> https://medium.com/linear-finance/%E2%84%93usd-exploit-283f2d32a2f3

The key to this attack lies in the ability of the attacker to mint an unlimited amount of lAAVE tokens. Our main focus here is to delve into the mechanism of the unlimited lAAVE token minting.

## Unlimited Minting of lAAVE Tokens: A Closer Look

### Main Addresses Involved in The Attack

Attacker address: 
>0xBd6111dB894D6cc4Aede0D4429F9D7EA55abD41b

Attack contract：
>0x58a278A80D2150481cEB9877aCE7d95eed6d5578

Deployer of attack contract：
>0xAC70Eba7890B1A4c0416E4e4d1FedD47B1417Fc2

Linear Finance's access control contract：
>0x50bde2dc074c849efc19c554e55f9f1befc7f0bf

The original admin of access control contract：
>0x5C9d6aFE82C8f1c33aB274C577932F2D40778347

The new admin of access control contract：
>0xc1A4aCafED94E69359361429593433ca5E917DA3
lAAVE token contract：0x3566c27ccdc102d91728c06f38c9bc907044d796

Attack transaction: 
>https://bscscan.com/tx/0xb9eac27439da28edad2560c81b6ddb331b33861a78a508ea6e7479d963fe280e

### Timeline of The Attack

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695630894906-4276e40e-8580-48f7-8003-cb5fec7ca6b7.png?x-oss-process=image%2Fresize%2Cw_1500%2Climit_0)






1. On September 7th, the original admin 0x5C9d6a of Linear Finance's access control contract invoked the ```SetAdmin``` function of the access control contract 0x50bde2, transferring the admin role to the new admin address 0xc1A4aC ([detailed transaction](https://bscscan.com/tx/0xd985f533b0393abe672facaa2e64a16df12dd2e829d93795e1905d94c1395cae)). The admin of the access control contract has the authority to call the ```SetIssueAssetRole``` function of the contract, granting the specified address the ```ISSUE_ASSET``` role, thereby enabling them to mint tokens.

   ![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/tif/26056616/1695369260780-081355c0-a407-4aa4-b1ce-d86d85c5f807.tif?x-oss-process=image%2Fformat%2Cpng%2Fresize%2Cw_1386%2Climit_0)

2. On September 15th, the creator of the attack contract, 0xAC70Eb, deployed the attack contract 0x58a278 ([detailed transaction](https://bscscan.com/tx/0xcc5d837340b71bef4197631dde40a53aa5026df677f49651e5708b0584a12e4f)).

3. On the same day, September 15th, the new admin of the Linear Finance's access control contract, 0xc1A4aC, invoked the ```SetIssueAssetRole``` function of the access control contract, granting the attack contract 0x58a278 the ```ISSUE_ASSET``` role, allowing it to mint tokens ([detailed transaction](https://bscscan.com/tx/0x5a4dbca85cd2b523e83b9fd4d913b6e904e40c3e12e1213f0f34b3a2a2826847)).

4. On September 21st, the attacker 0xBd6111 called the mintToken function of the attack contract 0x58a278, resulting in a significant amount of lAAVE token mint. 
By disassembling and examining the code of the attack contract 

   >https://library.dedaub.com/decompile?md5=63283effc0a281ce6e55249ca3bfc941

    We can see that the attacker invoked the ```mint``` function of the lAAVE token contract within the attack contract. The lAAVE token contract verified if the caller has the ```ISSUE_ASSET``` role. Since the attack contract has already been granted the ```ISSUE_ASSET``` role in the 3rd step, the attacker passed the verification and minted a large amount of lAAVE tokens ([detailed transaction](https://bscscan.com/tx/0xb9eac27439da28edad2560c81b6ddb331b33861a78a508ea6e7479d963fe280e)).

   ![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/tif/26056616/1695368623638-5884dfc2-2d95-44e7-b04e-cfa8a64c8cc8.tif?x-oss-process=image%2Fformat%2Cpng%2Fresize%2Cw_1500%2Climit_0)<center><i>mintToken function of the attack contract</i></center>

   ![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/tif/26056616/1695368750360-8b44c932-3955-445f-b7fa-bbcd5da2ccf5.tif?x-oss-process=image%2Fformat%2Cpng%2Fresize%2Cw_1500%2Climit_0)
   <center><i>mint function of lAAVE</i></center>

## Conclusion

Tracing the entire attack pathway indicates a strategic transfer of permissions. The primary suspect is address 0x5C9d6a, which handed over admin permissions of the Linear Finance's permission verification contract to address 0xc1A4aC. This set off a sequence of events: the deployment of the malicious contract by 0xAC70Eb and its subsequent exploitation by 0xBd6111, culminating in the massive lAAVE token mint. 

Further investigation of the fund flow across the attack chain revealed that the involved addresses (0xc1A4aC, 0xAC70Eb, and 0xBd6111) all traced back to the Tornado.Cash. Specific transactions highlight this connection ([transaction 1](https://bscscan.com/tx/0x27a086bd52c686732c0760ee52c5b8241a30eea809a75d75fe370541a851d5e), [transaction 2](https://bscscan.com/tx/0xeedf4025fb612110826f80c3b10102949692ee432eebae74d004b627444d0ae6) , and [transaction 3](https://bscscan.com/tx/0xb56dfb41ff9c69d958a9ccc425418e1350dc46f850a613846318e49f45688b76)). Consequently, there's a strong indication that the trio of addresses implicated in this attack could be under the control of a singular attacker.

Given these findings, we're led to believe that the private key associated with address 0x5C9d6a (initial admin for Linear Finance's permission verification) might have been compromised and possibly acquired by the entity behind 0xc1A4aC. This breach appears to be the root cause of the attack.

## Recommendations

1. **Embrace Multisig Wallets**: For pivotal permission-based operations in a project, lean on Multisig Wallets over single admin address. It offers an added security layer, ensuring crucial operations get multiple approvals, eliminating single points of failure.
2. **Robust Permission Checks**: Especially for high-risk operations like token minting and burning, implement rigorous permission checks. Alongside, continually test and validate to prevent unauthorized intrusions.