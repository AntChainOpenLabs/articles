<!--
 *@Author:reborncd
-->
<!--
 *@Author:reborncd
-->
# 从模块化看Web3基础设施

<font face="Song style">

今天，笔者将与大家一起简单探讨模块化与Web3的基础设施发展。

模块化是近两年来备受关注的概念，尽管它的提出时间较早，但受限于技术发展、工程实现，该概念在近1-2年才真正有较大落地和实现。

传统的经典区块链架构可以分为四个层次：**数据可用层（DA）、共识层、结算层和执行层**，如下图所示。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260024432-a333e03d-b8fc-4645-bb22-7d091848c067.png)<center><i>https://volt.capital/blog/the-merge</i></center>

一般来说，数据可用层存储交易元信息，共识层为交易定序，结算层存储State结果，执行层进行智能合约执行。当然有一些项目方基于自身的考量，在这4层的细节方面会有一定出入。

## 一、为什么我们需要模块化区块链

通常情况下，以太坊客户端将这四个层次放在一个进程中实现，其优点在于实现简单，但缺点是机器资源和软件架构无法简单地无限叠加，性能存在上限。另一方面，以太坊的性能还受限于以太坊的出块间隔和区块的Gas Limit。因此，在以太坊2.0的DankSharding正式上线之前，以太坊的单体链性能实际上很难有所突破。

当然，现在一些客户端已经开始意识到模块化的重要性，比如**Erigon、Reth**等。

在此背景下，社区将这四个层次进行模块化，旨在通过各种方式提高整体以太网生态系统的TPS。其中最典型的就是以太坊的Rollup。

## 二、Ethereum Rollup现状

### 2.1 从TVL看Rollup现状

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260034912-0a810003-1705-4304-89a1-bf229d25b066.png)
<center><i>https://defillama.com/chains</i></center>

先看一组数据，从上图可以看出，目前Layer1和Layer2的 TVL 占比中，除了以太坊占据最大的57%之外，其次是Binance Smart Chain（BSC）链。再然后便是Arbitrum、Optimism、Base、Mixin等Rollup链。

传统的Layer 1公链如Solana和Avalanche相对靠前，但与其他几个Rollup相比，它们在TVL方面仍存在较大差距 **（TVL在很大程度上反映了生态系统的繁荣程度）**。因此，我们可以初步得出一个结论，即以太坊Rollup生态系统已在公链领域基本处于主导地位，并产生一定规模效应。

P.S. 波场（Tron）在这方面比较特殊，其生态系统主要以转账交易为主。

### 2.2 成本降低但还没那么低

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260041112-8269ffb2-15fa-4890-b969-9b93cce168f6.png?x-oss-process=image%2Fresize%2Cw_676%2Climit_0)
<center><i>https://www.binance.com/en/blog/ecosystem/binance-research-halfyear-report-highlights-h1-2023-8155260287042551298</i></center>

从成本的角度来看，以太坊的成本约为4.21美元，而Optimism的成本为0.1美元，Arbitrum的成本为0.18美元。Rollup的成本虽然已经到了以太坊的几十分之一，但是由于需要将DA存储在以太坊的合约中，所以存储费用仍是大头，普遍超过70%的费用占比。

当然随着EIP 4844和DankSharding的上线，成本会进一步降低，但是目前来看，即使高于新公链几倍的成本，仍抵挡不住社区对Rollup的热情。

## 三、Rollup基本架构

接下来，让我们简单介绍一下Rollup基本架构。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260052138-1b8a78a0-4ba5-441c-91ea-e042f38f4a1d.png)
<center><i>https://foresightnews.pro/article/detail/6690</i></center>

简单来说，每个Layer2 Rollup可以被视为一个区块链。如上图所示，一般我们可以通过将Layer 1的计算逻辑转移到链下，将Layer 2压缩后的交易放入Layer 1的一个合约中，从而实现计算扩容的目标。因此，以太坊的Rollup实际上是在这四个层次中的**执行层**进行扩容。其他的**数据可用层、共识层**和**结算层**当然也可以扩容，只是生态不够繁荣，暂时未形成规模，我们会在下文介绍。

那么Rollup如何提升以太坊生态的性能呢？首先，目前每个Rollup性能大约可以达到100到500的量级。当然理论上限更高一些，但受限于当前的软件基础设施和Gas Limit的限制，还无法达到理想的水平。

以OP Stack为例，单个OP链的吞吐量大约为500TPS。如果我有5个OP链，那么总吞吐量可以达到2500TPS。当然，实际情况并不是简单地叠加，因为以太坊的经济模型是竞争性的。因此，当达到2500TPS时，单个交易的成本也会相应提高。所以交易打包者可能会选择在下一个区块中再提交，从而对性能产生负反馈。

当然目前可以初步认为，有了Rollup之后，以太坊的性能从原来的十几TPS提升到了几百TPS。要进一步提升性能，可能需要通过以太坊的DankSharding和EIP4844等协议升级来实现。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260168038-7e5aa895-0aa0-4e48-80cd-5b5684a91684.png)
<center><i>https://stack.optimism.io/docs/understand/explainer/</i></center>

## 四、从数据可用层看模块化存储层

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260173752-fc7782d6-6c5a-474a-a61b-1afa2db20e1c.png)
<center><i>https://blog.celestia.org/modular-data-availability-for-the-op-stack/</i></center>

刚才我们从计算层的角度看到了Rollup，这是率先突破的模块化生态。而除了计算层之外，许多数据可用层也在进行模块化的突破，其中Celestia、Avail、EthStorage都是其中的典型代表。

### 4.1 自建DA的机会与挑战

数据可用层想解决的仍是以太坊高昂的数据存储费用以及主网较低的性能问题，但是随着以太坊EIP-4844的协议升级，该问题可能会被极大的弱化。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260188049-425d56da-acf4-42b0-acf5-3b30bae92652.png?x-oss-process=image%2Fresize%2Cw_920%2Climit_0)
<center><i>https://twitter.com/hotbreaker3/status/1552905578749386752</i></center>

它带来的影响是什么呢？就是以太坊将拥有一个单独的Blob交易。这个交易可以很大，比如128KB，一个区块中可以容纳4或8笔交易，并且存储费用更为便宜，从而提升整体Rollup的性能、降低单笔交易的费用。

因此，对于这些DA厂商来说，他们面临的问题是如何与以太坊的EIP4844竞争，笔者认为有以下两个机会：

1、EIP4844并不保证这个Blob交易的永久存储，它只保证大约18天左右的有效性。

2、EIP4844仍是一个过渡方案，相比于DankSharding的最终方案来看，自建DA的链可以提供成本更低，容量更高的基础设施。

<figure class="half">
    <img src="https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260198843-6fd57d6b-85cd-459d-a33c-124bfb583afe.png"width="240">
    <img src="https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260202040-52153155-4965-4b8f-8d81-31bd867500ed.png"width="240">
</figure>
<center><i>https://eth-store.w3eth.io/</i></center>


上图是ETHStorage提供的存储容量和费率对比图，可以看到自建DA层一定是可以提供比以太坊链中单独一笔交易更高的扩容能力和更低的成本，只是社区是否买单是另一回事。（具体的数值仅供参考，笔者认为并不是特别严谨）

### 4.2 OnChain & OffChain DA

另一个解决思路是Off-Chain DA，即不将交易原文放到以太坊主网里，退化成Validium解决方案。不过从社区的TVL数据来看，对该模式貌似并不是很买单。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260230488-215531d3-0892-4aba-a433-b9ad29679753.png)
<center><i>https://defillama.com/chains</i></center>

以Arbitrum为例，Arbitrum是最大的OP Rollup解决方案，占据了整个Rollup生态系统中54%的TVL，但其Off-Chain DA的占比仅为0.2%。

从单个网络的角度来看，数据可用层的生态系统仍处于早期阶段，社区对其认可度尚未达到理想的水平。或者说，单个网络可能还不足以形成规模效应。可能需要等到多个网络启动时，才能形成更好的数据可用层生态系统。因此，目前来看，社区的认可度仍在快速发展，同时也有许多项目方参与到这个生态系统中。尽管没有像以太坊Rollup和模块化计算层那样火热，但也是一个技术趋势之一。

包括StarkNet、ZKSync在内的多个Rollup头部项目方也在不断推行自己的OffChain解决方案，预计年底到明年上半年会推行到测试网。

## 五、从DA到模块化共识层

上一节我们聊到计算层和数据可用层的模块化，现在再来讲讲共识层。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260258548-42bd56dd-9903-4acc-98b2-283e677999fa.png?x-oss-process=image%2Fresize%2Cw_920%2Climit_0)
<center><i>https://coincu.com/208537-espresso-review/</i></center>

共识层主要目的是为了L2的交易定序，需要注意的是，L2的定序结果不一定是最终结果，我们的定序最终结果仍以以太坊合约为主。

这一层的意义在于，Layer2不需要自己排序，信任去中心化/共享Sequencer的结果即可。对于Layer1来说，可能只会保留排序后的交易hash甚至Root根。

这种方式的劣势很明显，大家愿意将交易放在以太坊中，是因为相信以太坊具有高度的安全性。如果Layer 2出现问题，仍可以通过以太坊的交易来恢复Layer2状态。然而，对于一个新的共识层或数据可用层的基础设施，它暂时还达不到以太坊那样的公信力和认可度。

当然，共识层模块化也有自身的优势，比如成本更低，MEV空间更大，具体我们将在6.1详细论述。

综上所述，我们再次审视模块化区块链的问题。目前，计算层扩容仍然是最主流的模块化解决方案，而数据可用层和共识层的模块化也在快速发展阶段。至于结算层，通常会与数据可用层或共识层一同考虑。这是目前以以太坊为基础的模块化区块链生态架构的基本情况。

## 六、模块化带来的“共享经济”

### 6.1 共享Sequencer & MEV

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260285668-a055f6b5-b755-43c6-b5f4-0100f77d9d69.png)
<center><i>https://hackmd.io/@EspressoSystems/EspressoSequencer</i></center>
<center>Espresso Sequencer Network</center>

Sequencer最主要的作用之一是对L2交易定序、打包、提交（到L1），虽然最终的定序结果以L1 Smart Contract为准，但是大多数时候，Sequencer的排序结果与最终结果相对一致。

虽然自建Sequencer的代价并不高，但是对于一些Stack生态链来说（比如OP Stack或ZK Stack），如果能复用、共享这些基础设施，也可以降低“一键发链”的复杂度。

不过对于这些提供Sequence能力的基础设施提供方而言，如果有权利对多个Rollup链定序，在MEV上会有更多收益。

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260317164-569be87f-ca7a-44e6-82a4-972d5b8d6a1b.png)
<center><i>https://writings.flashbots.net/the-future-of-mev-is-suave/</i></center>

以FlashBots为例，其发起的SUAVE链期望可以对包括以太坊、ZK Rollup、OP Rollup等所有以太坊生态进行排序，以Intent的方式完成MEV的竞拍。又由于其一旦持有多个链的定序权，甚至可以实现“类跨链MEV”的效果，以获取比单条链更高的收益。

### 6.2 DA & Restaking

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260335264-19c48813-829e-4355-99cd-f3c958ac2821.png?x-oss-process=image%2Fresize%2Cw_920%2Climit_0)
<center><i>https://medium.com/iosg-ventures/eigenlayer-bringing-ethereum-level-trust-to-middleware-c5c426bc8c33</i></center>

常规意义上的DA会提供低价、高效的数据存储方式，甚至提供可编程的能力（比如ETHStorage）。但为了让安全性更高、生态激励模型更健康，一些DA解决方会考虑用利益“软性捆绑”以太坊验证者节点，将以太坊生态的安全性与DA生态之间形成一个弱绑定关系。

最出名的就是ReStaking的龙头项目EigenLayer，其EigenDA的数据可用性由以太坊验证者保证，而验证者因为质押了32个ETH，所以本身具有一定的可靠性。而同时，为了鼓励验证者加入EigenDA的网络，EigenLayer会给与代币等激励，从而与以太坊生态形成一定程度上的共享安全性。

不过这个方案被Vitalik猛烈抨击，原因是会对以太坊生态造成安全性下降的风险。

### 6.3 计算加速平台

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260351965-5b8de243-b985-4a7c-a2e2-2715efbfb2fc.png?x-oss-process=image%2Fresize%2Cw_920%2Climit_0)
<center><i>https://docs.opside.network/zk-raas</i></center>

ZK Rollup是一个挑战期更短，未来更可期的Rollup解决方案，但是其要求项目方具有更多的计算资源以及更专业的加速设备和算法实现。所以一些硬件加速的平台想到提供共享的硬件和ZKP生成能力，以一种PoW的模型提供ZKP加速能力。虽然在当前的ZK Rollup生态中，该模式还未完全得到接受，但是从StarkNet的共享Prover以及ZKSync 推出的ZK Stack的趋势来看，未来这不失为一种良性“共享”经济。

### 6.4 AppChain & L3

![image](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/100656720/1695260368999-6a4cafba-0465-46ee-a3a6-37c56ec257db.png)
<center><i>https://era.zksync.io/docs/reference/concepts/hyperscaling.html</i></center>

笔者认为，提到Rollup，必须要提到AppChain甚至L3的概念，原因有三：

1、单个Rollup的软件/硬件性能是有上限的，多个Rollup实现计算层扩容是一种性价比较高的解决方案。

2、未来单个超级DApp的出现，势必对隐私、性能、成本、可编程性有更高的要求，此时自建Rollup的必要性也会出现。

3、随着全链游等模式的兴起，单个Rollup对于编程语言、交互形式、信任假设的需求也会发生变化，一个不那么中心化的Rollup或许更适合部分GameFi厂商的需求。

当然，我们提到AppChain一般不会希望其是一个孤立的生态，因为这没有办法利用以太坊的资金效率，享受以太坊的存量账户优势，这也是很多Rollup项目方纷纷提出Stack解决方案的原因之一。

## 七、叙事在循环，技术在迭代

AppChain、一键发链、共享经济这些模式在上一轮新公链里已经反复出现，比如Polkadot和Cosmos。只是这一次，叙事轮到了以太坊生态。

当然不同于以往的是，这一轮Rollup的技术迭代更尊重密码学发展、社区博弈、生态行业细分。但是回到本质，社区最希望的是有若干个超级App能够出现，给与链更大的价值属性。而这一次，更低的成本、更高的性能、更公平开放的Rollup或许会带来不一样的变化。

## 未来怎么发展？

笔者认为未来的模块化赛道有以下几大趋势：

**1、各个模块化赛道将更加细分，并积极与Smart Contract Rollup积极合作；**

**2、经济模型趋于完善，促进更多矿工、Validator加入；**

**3、以太坊生态依然占据强势地位，生态服务将持续跟进ETH 4844、Danksharding、AA等技术方向。**

单打独斗和抱团取暖各有利弊，但毫无疑问，脱离社区和资本的生态大概率走不长远，未来发展如何，让我们拭目以待。