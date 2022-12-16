# 面向任务的支付协议

A protocol for task-based payments on the EVM Compatible blockchain.

这是一个基于以太坊的任务支付协议，用于解决基于任务的支付场景中的互信问题，可用于 DAO 任务分发或个人外包项目等

### 任务流程如下：

1. 任务由三方共同发起：发起者、接收者、仲裁方。
2. 接受者和仲裁方对任务内容确认签名后，由发起方上链生成订单，并锁定任务报酬。
3. 任务报酬按里程碑分配，每个里程碑包含一个时间和金额，每个里程碑都会生成一个 Order。
4. 任务生成的同时，会生成一个凭证 NFT，mint 给发起方。有几个里程碑就会有几个 order 和 nft。
5. 任务到达里程碑后，发起方可以将凭证 NFT 转移给接收者，接收者可以使用此凭证来兑换报酬。

6. 任务到达里程碑后，如果有争议，任何一方都可以将订单打入 等待仲裁的状态，此时，仲裁方可以选择做两个操作之一：
   1. 仲裁方可以将订单状态修改为退单，发起方可以使用订单对应的凭证来兑换退款。
   2. 仲裁方可以将凭证 NFT 强制转移给接收者，接收者可以使用此凭证来兑换报酬。
   3. 订单状态重新回到未接单
7. 在未接单状态下，NFT 兑换报酬后，订单结单，NFT 销毁。

### nft 凭证兑换报酬或退款的逻辑是：

1. 任意时刻和任何条件下，任务接收方都可以使用此 nft 兑换 order 中锁定的报酬（order 状态需要是未结单）。
2. 不过默认情况下，nft 是 mint 给发起方的，发起方可以随时把 nft 转移给接收方，这样接收方就可以随时兑换报酬。
3. 发起方只有在一种情况下可以使用 nft 兑换锁定的报酬，就是订单状态为"退单"的时候，其他情况下持有 nft 也不能兑换。

### 任务状态：

1. 开始。代表任务已经开始，报酬已经锁定，并且仲裁无法介入，发起方无法兑换锁定的报酬。
2. 达到里程碑。代表任务已经到达里程碑，此时有争议，仲裁方可以介入操作订单状态为退单。
3. 退单。代表任务已经退单，只有此状态下，发起方才可以使用 nft 兑换锁定的报酬。
4. 结单。任何一方使用 nft 兑换报酬后，自动变为结单。

### did 和 信用徽章

合约中有三种徽章，分别是：

- 发起者徽章
- 接收者徽章
- 仲裁者徽章