<!--
 *@Author:reborncd
-->
<!--
 *@Author:reborncd
-->
# Linear Finance 攻击事件分析

<font face="Song style">

2023年9月22日，资产跨链协议 Linear Finance 发布公告称其在9月21日遭到了攻击。攻击者可以无限铸造 lAAVE，然后在 Linear Exchange 上将 lAAVE 换成 lUSD。该攻击导致 PancakeSwap 和 Ascendex 上所有的 lUSD 流动性被抽干，进而导致 lUSD 的价格跌至零。
>https://medium.com/linear-finance/%E2%84%93usd-exploit-283f2d32a2f3

本次攻击的关键在于攻击者可以无限铸造 lAAVE 代币。所以接下来我们主要针对 lAAVE 代币无限铸造这个点进行详细的分析。

## 无限铸造 lAAVE 代币交易分析

### 攻击链路中涉及的主要地址

攻击者地址：
>0xBd6111dB894D6cc4Aede0D4429F9D7EA55abD41b

攻击合约地址：
>0x58a278A80D2150481cEB9877aCE7d95eed6d5578

攻击合约创建者地址：
>0xAC70Eba7890B1A4c0416E4e4d1FedD47B1417Fc2

Linear Finance访问控制合约：
>0x50bde2dc074c849efc19c554e55f9f1befc7f0bf

Linear Finance访问控制合约的原admin地址：
>0x5C9d6aFE82C8f1c33aB274C577932F2D40778347

Linear Finance访问控制合约的新admin地址：
>0xc1A4aCafED94E69359361429593433ca5E917DA3

lAAVE 代币合约地址：
>0x3566c27ccdc102d91728c06f38c9bc907044d796

攻击交易：
>https://bscscan.com/tx/0xb9eac27439da28edad2560c81b6ddb331b33861a78a508ea6e7479d963fe280e

### 攻击时间线梳理

![image](https://github.com/AntChainOpenLabs/articles/blob/main/_resources/Linear%20Finance%20%E6%94%BB%E5%87%BB%E4%BA%8B%E4%BB%B6%E5%88%86%E6%9E%9001.jpg)

1. 9月7日，Linear Finance的访问控制合约的原admin地址0x5C9d6a调用Linear Finance访问控制合约0x50bde2的 ```SetAdmin``` 函数，将admin权限转移给了新admin地址0xc1A4aC（[交易详情](https://bscscan.com/tx/0xd985f533b0393abe672facaa2e64a16df12dd2e829d93795e1905d94c1395cae)）。访问控制合约的admin有权调用该合约的 ```SetIssueAssetRole``` 函数，赋予指定地址ISSUE_ASSET权限，使其有权增发代币。

   ![image](https://github.com/AntChainOpenLabs/articles/blob/main/_resources/Linear%20Finance%20%E6%94%BB%E5%87%BB%E4%BA%8B%E4%BB%B6%E5%88%86%E6%9E%9002.jpg)

2. 9月15日，攻击合约创建者0xAC70Eb部署了攻击合约0x58a278（[交易详情](https://bscscan.com/tx/0xcc5d837340b71bef4197631dde40a53aa5026df677f49651e5708b0584a12e4f)）。

3. 在9月15日同一天，Linear Finance访问控制合约的新admin——0xc1A4aC调用了访问控制合约的 ```SetIssueAssetRole``` 函数，赋予了攻击合约0x58a278 ```ISSUE_ASSET``` 权限，使其能够增发代币（[交易详情](https://bscscan.com/tx/0x5a4dbca85cd2b523e83b9fd4d913b6e904e40c3e12e1213f0f34b3a2a2826847)）。

4. 9月21日，攻击者0xBd6111调用攻击合约0x58a278的 ```mintToken``` 函数，增发了大量lAAVE代币。通过反汇编查看攻击合约的代码：

   >https://library.dedaub.com/decompile?md5=63283effc0a281ce6e55249ca3bfc941

   我们看到，攻击者在攻击合约中调用了lAAVE代币的 ```mint``` 函数进行代币增发，lAAVE代币合约会校验调用者是否具有 ```ISSUE_ASSET``` 权限，而在第3步中，访问控制合约的新admin已经赋予了攻击合约 ```ISSUE_ASSET``` 权限，因此攻击者得以顺利通过检测，从而成功增发了大量lAAVE代币（[交易详情](https://bscscan.com/tx/0xb9eac27439da28edad2560c81b6ddb331b33861a78a508ea6e7479d963fe280e)）。

   ![image](https://github.com/AntChainOpenLabs/articles/blob/main/_resources/Linear%20Finance%20%E6%94%BB%E5%87%BB%E4%BA%8B%E4%BB%B6%E5%88%86%E6%9E%9003.jpg)<center><i>攻击合约的mintToken函数</i></center>

   ![image](https://github.com/AntChainOpenLabs/articles/blob/main/_resources/Linear%20Finance%20%E6%94%BB%E5%87%BB%E4%BA%8B%E4%BB%B6%E5%88%86%E6%9E%9004.jpg)
   <center><i>lAAVE代币的mint函数</i></center>

## 总结

通过复盘整个攻击流程，我们发现，本次攻击的根本原因是地址0x5C9d6a将Linear Finance的访问控制合约0x50bde2的admin权限转移给了地址0xc1A4aC，随后，地址0xAC70Eb部署了攻击合约0x58a278，随后，新的admin地址0xc1A4aC赋予了攻击合约 ```ISSUE_ASSET``` 权限，最终攻击者0xBd6111调用了攻击合约0x58a278，增发了大量lAAVE代币。

通过对攻击链路中涉及的地址进行资金流分析，我们发现，地址0xc1A4aC、0xAC70Eb和0xBd6111的初始资金都来自Tornado.Cash (涉及[交易1](https://bscscan.com/tx/0x27a086bd52c686732c0760ee52c5b8241a30eea809a75d75fe370541a851d5e)， [交易2](https://bscscan.com/tx/0xeedf4025fb612110826f80c3b10102949692ee432eebae74d004b627444d0ae6) 和[交易3](https://bscscan.com/tx/0xb56dfb41ff9c69d958a9ccc425418e1350dc46f850a613846318e49f45688b76)）。因此这三个参与者很可能都是由攻击者控制的。

同时，我们高度怀疑，地址0x5C9d6a（Linear Finance访问控制合约的原admin）的私钥出现了泄漏并被地址0xc1A4aC的持有者获取，最终导致了本次攻击事件的发生。

## 建议

1. **使用多重签名钱包**: 对于项目中的关键权限操作，例如关键权限的转移等，我们建议建议使用多重签名钱包（Multisig Wallets）而不是单一的管理员地址。多重签名钱包提供了额外的安全保护，能够消除因单个私钥泄漏导致的单点故障风险。

2. **编写健壮的权限校验代码**：建议项目中的敏感操作，比如代币铸造、代币销毁，做好权限校验，并进行详尽的测试以避免攻击发生。