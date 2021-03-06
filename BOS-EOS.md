# BOS与EOS差异

作为一个对EOS比较了解的开发者, 简单介绍一下部署BOS和EOS节点的差异.

## CPU主频要求

BOS 要求出块节点的主频达到 4GHz. 

在EOS上, 出块节点的cpu主频很大程度影响了tps和cpu的价格. 我认为BOS这一要求可以进一步提升TPS同时降低CPU价格.

## P2P自发现功能

在EOS上, 节点只能与配置文件中的P2P节点进行数据传输, 这样的情况不利之处在于:

- 需要频繁维护节点列表
- 知名节点网络压力大
- 新加入的节点难以提供数据服务

BOS加入了P2P自发现功能,能够与其他节点共享P2P列表,这样可以有效提高整个网络的健壮性.

通过config.ini配置文件中的以下配置控制自发现:
```
p2p-discoverable = true
```
为了避免连接不可信节点,建议BP节点不要开自发现.

## 按照时区顺序进行出块

EOS前21名的bp出块是按照出块节点账户名的字母排序的, 这样带来了什么问题呢? 有可能出块顺序中相邻的两个节点部署的地理位置确相差十分远. 因为网络延迟, 经常会出现微分叉.

BOS修改了出块顺序, 按照出块节点的时区来排序 这样可以有效降低网络延时.

在注册bp的时候,第四个参数为时区:
```
cleos -u api节点 system regproducer 账户名 出块公钥 官网地址 时区
```
时区字段为:-11～12.

## 节点信息提供

EOS上节点详细信息一般是访问:节点官网/bp.json

BOS规定节点详细信息访问位置:节点官网/bos.json

## Notify Plugin
BOS 内置提供Notify Plugin, 可以实现与History Plugin类似的使用方式，可以低成本、快速的获得账户监听功能。

## 其他改进(配置无关)

### 安全随机数

目前EOS上的DAPP已经发生多起被黑事件, 一个最大的原因就是DAPP生成随机数的算法不妥当, BOS自带随机数api, 这样可以有效避免此类安全事件.

### 低保

EOS上的cpu资源模型, 经常会发生一个很尴尬的局面, cpu价格高企的时候, 用户因为抵押的cpu不够, 不能发起交易, 连给自己抵押的交易都不能发起, 需要其他人帮助.

BOS上为用户提供资源的低保, 每个用户每天能够获得一定时间免费的资源, 这样可以有效解决资源不足的问题, 提升了用户体验.

### 其他

更多的优化, 请看[BOS白皮书](https://github.com/boscore/Documentation)



# The Differences between BOS and EOS
 
As a developer who knows quite well about EOS, let's take a brief look at the differences between deploying BOS and EOS nodes.

## CPU Frequency Requirement
BOS requires the main frequency for producing blocks reach to 4GHz.

On EOS, the CPU frequency of the BPs greatly affects the price of CPU and the TPS. I think the BOS can further increase TPS while reducing CPU price.

## P2P Self-discovery Function
On EOS, nodes can only transfer data to P2P nodes in the configuration file. The disadvantages are:
- Need to maintain a frequent list of nodes
- Well-known nodes have more network pressure
- Newly participated nodes are difficult to provide data services

BOS added the P2P self-discovery function and can share P2P lists with other nodes, which can effectively improve the robustness of the entire network.

Controlling self-discovery through the following configurations in the config.ini file:
```
p2p-discoverable = true
```
In order to avoid connecting untrusted nodes, it is recommended that BPs not to turn on self-discovery function.

## Producing blocks in timezone order
The top 21 BPs of EOS are sorted by the letter of their account names, and what is the problem? It is possible that the geographical location of the two BPs is very far apart, which will lead to many forks due to the network latency.

BOS has modified the order of the block-producing and sorts them according to the timezone. This can effectively reduce the network latency.


When registering the BP account, the 4th parameter is the timezone:
```
cleos system regproducer BPACCOUNT XXXXX https://xxxx.com 11
```

## Information provided by: 
The node details on EOS: node official website/bp.json
The node details on BOS: node official website/bos.json

## Notify Plugin
BOS has a built-in Notify Plugin that can be used in a similar way to the History Plugin, enabling low-cost, fast access to account monitoring, etc. 


## Other Improvements (not related to configurations)

### Safer Random Number Scheme
At present, there are many hacked events in DAPPs on EOS. One of the biggest reasons is that the algorithm for generating random numbers by DAPP is inappropriate. BOS comes with random number API, which can effectively avoid such security events.

### Guaranteed Minimum Provision
The CPU resource model on EOS often leads us to a very embarrassing situation. When the price of CPU is high, the user cannot initiate a transaction because of the insufficient CPU mortgage, even to mortgage for one's own account and he needs to ask others to help.

BOS provides users with a minimum guaranteed resources. Each user can get free resources for a certain period every day, which can effectively solve the problem of insufficient resources and improve the user experience.

### Others
More optimations, please view [BOS whitepaper](https://github.com/boscore/Documentation/blob/master/BOSCoreTechnicalWhitePaper.md)
