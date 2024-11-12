# FusionFi 协议
![FFP](./image/image-13.png)
## 概述
在去中心化金融（DeFi）领域，FusionFi 为 AgentFi 生态提供了一种标准化的互操作性协议框架。
通过该协议，用户能够部署具有个性化金融功能的代理（Agent），自主执行资产管理、套利等操作。
FusionFi 的核心理念是在标准化的协议框架内，让 Agents 之间具备互操作性，使其能够在不同金融场景之间无缝协作，实现资本效率最大化。

## 基本概念

### AO
AO 是一部基于 Arweave 去中心化存储网络构建的超并行计算机。它采用模块化架构设计，可以通过无限数量的并行进程进行扩展，支持高效的数据存储和处理。AO 的设计目标是提供一个灵活且高效的计算平台，使得用户能够在去中心化环境中进行复杂的计算任务，同时确保数据的安全性和可访问性。关于 AO 网络的详细介绍请访问此处：https://cookbook_ao.arweave.dev/  

### Process
Process 是 AO 中的一个核心概念，指的是在计算环境中执行的独立任务或工作流。每个进程都是分散的、确定性的计算环境，根据预定义的逻辑与 AO 网络进行交互。它们的功能与智能合约类似，但提供更大的灵活性和对无限计算的访问。每个进程都维护一个持久的全息状态，作为消息日志存储在 Arweave 上。这些功能使 AO 进程能够充当可验证的自主代理(Agent)，能够在链上执行大规模计算并执行高级任务，而无需人工干预。

### Agent
Agent(代理) 是一个具有明确功能和目标的专用程序实体，Agent 可以执行单一任务，例如获取数据，也可以通过执行复杂的逻辑来自主操作，例如多步骤的工作流，条件决策以及与其他 Agent 的动态交互等。

### AgentFi
AgentFi 是一种基于 Agent 的金融模式，允许用户通过智能合约创建自主金融代理。用户可以通过个性化的智能合约代理自动执行资产管理、策略执行等操作，并且这些代理可以与其他用户(代理)、平台无缝交互，从而实现个性化的金融服务。传统 DeFi 协议存在集中式管理和操作僵化的问题，而 AgentFi 通过独立代理的方式提升了交易自由度，用户拥有自己的去中心化金融主体，亦即所谓的“主权金融”。

### Note
Note 是 FusionFi 协议的一个核心概念。它是一种资产的抽象表示模型，在协议中扮演了“数字凭证”或“合约票据”的角色。利用 Note 模型。FusionFi 可以支持丰富的金融场景，如交易、借贷、质押等。

### Settlement
Settlement ”结算“ 是 FusionFi 协议的一个核心概念。它代表在协议中处理和完成金融交易的过程。它是确保 Agents 发起的所有金融活动能够成功完成的关键环节。


## 协议架构
```

+-------------+               +--------+
|   Agents   |               |   User   |
+-------------+               +--------+
         \                         /
          \
          note                    note
                                /
           \                  /
            \                /
            +---------------+
            |   Note Pool   |
            +---------------+
                   |
                   |
                   notes
                   |
        +----------+----------+------------------- 
        |                     |                   |
+-------v-------+   +---------v---------+   +------------+
|   Settler 1   |   |    Settler 2      |   |  Settler N |
|   (User)      |   |       (bot)       |   |  (Agent)   |
+---------------+   +-------------------+   +------------+
                   \       |                /
             notes  \      |    notes      /
                     \     |              /
                      \    |             /
                       +---v-------------+
                       | Settlement      |
                       | Center 1        |
                       +-----------------+
                                 |
                                ...
                       +-----------------+
                       | Settlement      |
                       | Center M        |
                       +-----------------+
```
### 架构描述
- Note Pool 存储整个系统的所有 notes.
- Agents 或者 User 都可以直接生成 note 并发送给 note pool
- settler 负责从 note pool 中提取可结算的 notes, 并将其发送到 Settlement Center 进行结算。Settler 可以是 普通用户、套利机器人甚至是 Agent 等
- Settlement Center 负责处理结算请求，系统中可以有多个 Settlement Center, 负责执行各自的结算任务

## Note
在基于 FusionFi 协议的系统中，Note 是一种独立的金融工具或权益证明。    
Note 的形式可以是代币、债券、凭证、合约权利等，它的存在主要是为了在系统中创建具有价值、可交易的对象。每一个 Note 都可以代表一种资产、合约或特定的权益。    

### Note 的主要属性
每个 Note 通常会具备一下属性：
- NodeID: Note 的唯一标识符，便于在系统中识别和跟踪
- Issuer: Note 的发行方，发行（创建） Note 的用户或 Agent
- Holder: 当前拥有该 Note 的用户或 Agent
- AssertID: 发行 Note 的资产类型
- Amount: 发行 Note 代表的面值额
- HolderAssertID: 兑换 Note 的资产类型
- HolderAmount: 兑换 Note 代表的面值额
- IssuerDate: 发行日期
- ExpireDate: 如果 Note 有时间限制（例如到期日），则会包含一个有效期，表示该 Note 在何时之前有效
- Status: Note 当前的状态，可能是“未结算”、“已结算”、“过期”等
- SettleRule: 结算规则，Note 在被结算时的具体规则，比如结算条件、结算方式、结算价格等信息

### Note 的生命周期
在系统中，Note 的生命周期通常包含以下几个阶段：
1. 创建：Note 被 Agent 或者用户创建，产生并进入 Note Pool. 每个 Note 会携带其属性和特性信息。
2. 存储：所有 Note 会被集中存储在系统的 Note Pool 中，Note Pool 在系统中扮演一个共享存储空间，方便其他实体（如 Settler) 访问。
3. 提取：Settler 从 Note Pool 中检索符合条件的 Note, 准备进行结算。Settler 可能会根据特定的策略或条件（如价格波动或套利）来选择 Note.
4. 结算：Settler 会将符合条件的 Note 发送到 Settlement Center 进行结算。Settlement Center 负责执行具体的结算操作，例如支付、权益转移等。
5. 完成：结算完成后，Note 会被标记为”已结算“。只有在单笔结算中的所有 Note 都结算完成，才能为 Note 更改状态，保证单笔结算的原子性。

### Note 的应用场景
Note 在系统中具有多种作用，可以支持丰富的金融场景：
- 结算支持：通过 Note 的结构化定义，系统能够支持高效的结算过程，不同的 Settlement Center 可以根据 Note 的定义进行自动化结算操作.
- 套利机会：对于套利者来说，Note 可以被用作价格波动或者市场条件发生变化下的套利工具。套利者的自动化机器人（Agent)可以实时监控 Note Pool 中的 Note, 并发现机会进行结算套利，甚至不需要使用自有资金就可以完成无损套利.
- 债务和信用工具： Note 可以代表债务（如借款）或信用工具（如债券），允许持有者获得利息收益或在未来某个日期偿还债务.
- 期权合约： Note 可以被作为期权工具，定义期权的行权价格、到期时间和条件。用户可以在期权到期之前通过 Note Pool 交易这些期权 Note.
- 抵押和质押： Note 可以用于抵押或者质押场景中，比如抵押某种资产来生成一个 Note, 用于在其他的金融操作中使用，比如超额抵押生成稳定币.

## Note 的技术实现用例
Note 的技术实现需要依赖一定的结构化数据模型来定义它的各个属性。  
在 AO 网络中，通常是 AO AgentFi Process 会实现各自 Note 的相关逻辑，主要包括：note 创建，状态更新，结算执行等操作。   
以下是如何在 AO Agent 中实现 Note 的详细介绍。
### 创建 Note
Note 的创建主要是将新的 Note 生成并发送到系统指定的 Note Pool。在这个过程中，Note 需要被初始化，本地存储，并在 Note Pool 中进行登记（Note Pool 存储新的 Note 信息）。Note Pool 负责存储和管理系统中所有的需要结算的 Note 信息。

#### 1. Note 数据结构
```
{
  NoteID string,
  AssetID string,
  Amount string,
  HolderAssetID string,
  HolderAmount string,
  IssueDate string,
  ExpireDate string,
  SettlementCenter string,
}
```
TODO
