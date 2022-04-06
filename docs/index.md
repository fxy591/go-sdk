# 入门介绍

## 1.入门开始

### go-sdk简介

go-sdk是一个轻量级、高度模块化、响应式、类型安全的go类库提供丰富API，用于处理TelChain智能合约及与TelChain网络上的客户端(节点)进行集成。

可以通过它进行TelChain区块链的开发，而无需为你的应用平台编写集成代码。

## 2.go-sdk开发入门

首先将最新版本的go-sdk安装到项目中。

以下为go.mod文件内容：

```
module github.com/bif/bif-sdk-go

go 1.17

require (
	github.com/aristanetworks/goarista v0.0.0-20200423211322-0b5ff220aee9
	github.com/btcsuite/btcd v0.21.0-beta
	github.com/btcsuite/btcutil v1.0.2
	github.com/davecgh/go-spew v1.1.1
	github.com/gorilla/websocket v1.4.2
	github.com/libp2p/go-libp2p-core v0.5.2
	github.com/pborman/uuid v1.2.0
	github.com/prometheus/common v0.9.1
	github.com/stretchr/testify v1.5.1
	github.com/teleinfo-bif/bit-gmsm v1.0.5
	golang.org/x/crypto v0.0.0-20201221181555-eec23a3978ad
	golang.org/x/sys v0.0.0-20200219091948-cb0a6d8edb6c
	gopkg.in/check.v1 v1.0.0-20200227125254-8fa46927fb4f
)

require (
	github.com/alecthomas/template v0.0.0-20190718012654-fb15b899a751 // indirect
	github.com/alecthomas/units v0.0.0-20190717042225-c3de453c63f4 // indirect
	github.com/gogo/protobuf v1.3.1 // indirect
	github.com/google/uuid v1.0.0 // indirect
	github.com/ipfs/go-cid v0.0.5 // indirect
	github.com/konsorten/go-windows-terminal-sequences v1.0.1 // indirect
	github.com/kr/text v0.1.0 // indirect
	github.com/libp2p/go-buffer-pool v0.0.2 // indirect
	github.com/libp2p/go-openssl v0.0.4 // indirect
	github.com/minio/blake2b-simd v0.0.0-20160723061019-3f5f724cb5b1 // indirect
	github.com/minio/sha256-simd v0.1.1 // indirect
	github.com/mr-tron/base58 v1.1.3 // indirect
	github.com/multiformats/go-base32 v0.0.3 // indirect
	github.com/multiformats/go-multiaddr v0.2.1 // indirect
	github.com/multiformats/go-multibase v0.0.1 // indirect
	github.com/multiformats/go-multihash v0.0.13 // indirect
	github.com/multiformats/go-varint v0.0.5 // indirect
	github.com/niemeyer/pretty v0.0.0-20200227124842-a10e7caefd8e // indirect
	github.com/pmezard/go-difflib v1.0.0 // indirect
	github.com/sirupsen/logrus v1.4.2 // indirect
	github.com/spacemonkeygo/spacelog v0.0.0-20180420211403-2296661a0572 // indirect
	github.com/spaolacci/murmur3 v1.1.0 // indirect
	gopkg.in/alecthomas/kingpin.v2 v2.2.6 // indirect
	gopkg.in/yaml.v2 v2.2.8 // indirect
)
```

## 3.启动客户端

需要启动一个TelChain客户端，当然如果你已经启动了就不需要再次启动。

```
gbif --singleton.dpos
```

```
Gbif gbif = Gbif.build(new HttpService("http://ip:port"));
```

当不需要Bifj实例时，需要调用 `shutdown`方法来释放它所使用的资源。

```
gbif.shutdown()
```

## 4.发送请求

**发送同步请求**

```
Gbif gbif = Gbif.build(new HttpService());  // defaults to http://localhost:8545/
Web3ClientVersion web3ClientVersion = gbif.bifClientVersion().send();
String clientVersion = web3ClientVersion.getWeb3ClientVersion();
```

## 5.IPC

go-sdk还支持通过文件套接字快速运行进程间通信（IPC），支持客户端在相同的主机上同时运行go-sdk。在创建服务时，使用相关的 `IPCService`就可以实现而不需要通过 `HTTPService`。

```
// OS X/Linux/Unix:
Gbif gbif = Gbif.build(new UnixIpcService("/path/to/socketfile"));
...

// Windows
Gbif gbif = Gbif.build(new WindowsIpcService("/path/to/namedpipefile"));
...
```

## 6.通过go打包TelChain智能合约

go-sdk可以自动打包智能合同代码，以便进行TelChain智能合同部署和交互。

要打包代码，需要先编译智能合同：

```
$ solc <contract>.sol --bin --abi --optimize -o <output-dir>/
</output-dir></contract>
```

```
bifj solidity generate /path/to/<smart-contract>.bin /path/to/<smart-contract>.abi -o /path/to/src/main/java -p com.your.organisation.name
</smart-contract></smart-contract>
```

接下来就可以新建和部署智能合约了：

```
Gbif gbif= Gbif.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");

YourSmartContract contract = YourSmartContract.deploy(
        <bifj>, <credentials>,
        GAS_PRICE, GAS_LIMIT,
        <param1>, ..., <paramn>).send();  // constructor params
</paramn></param1></credentials></bifj>
```

或者使用一个现有的智能合约：

```
YourSmartContract contract = YourSmartContract.load(
        "0x<address>|<ensname>", <bifj>, <credentials>, GAS_PRICE, GAS_LIMIT);
</credentials></bifj></ensname></address>
```

然后就可以进行智能合约的交互了：

```
TransactionReceipt transactionReceipt = contract.someMethod(
             <param1>,
             ...).send();
</param1>
```

调用智能合约：

```
Type result = contract.someMethod(<param1>, ...).send();
</param1>
```

## 7.Filters

go-sdk的响应式函数可以使观察者通过事件去通知消息订阅者变得很简单，并能够记录在区块链中。接收所有新的区块并把它们添加到区块链中：

```
Subscription subscription = gbif.blockObservable(false).subscribe(block -> {
    ...

});
```

接收所有新的交易并把它们添加到区块链中：

```
Subscription subscription = gbif.transactionObservable().subscribe(tx -> {
    ...

});
```

接收所有已经提交到网络中等待处理的交易。(他们被统一的分配到一个区块之前。)

```
Subscription subscription = gbif.pendingTransactionObservable().subscribe(tx -> {
    ...

});
```

或者你重置所有的区块到最新的位置，那么当有新建区块的时候会通知你。

```
Subscription subscription = catchUpToLatestAndSubscribeToNewBlocksObservable(
        <startblocknumber>, <fulltxobjects>)
        .subscribe(block -> {
            ...

});
</fulltxobjects></startblocknumber>
```

**主题过滤**也被支持：

```
CoreFilter filter = new CoreFilter(DefaultBlockParameterName.EARLIEST,
        DefaultBlockParameterName.LATEST, <contract-address>)
             .addSingleTopic(...)|.addOptionalTopics(..., ...)|...;
gbif.coreLogObservable(filter).subscribe(log -> {
    ...

});
</contract-address>
```

当不再需要时，订阅也应该被取消：

```
subscription.unsubscribe();
```

**注意：Infura中不支持filters。**

## 8.交易

go-sdk支持使用TelChain钱包文件（推荐的）和用于发送事务的TelChain客户端管理命令。

使用钱包文件发送积分给其他人：

```
Gbif gbif= Gbif.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");
TransactionReceipt transactionReceipt = Transfer.sendFunds(
        gbif, credentials, "0x<address>|<ensname>",
        BigDecimal.valueOf(1.0), Convert.Unit.coreER)
        .send();
</ensname></address>
```

或者你希望建立你自己定制的交易：

```
Gbif gbif = Gbif.build(new HttpService());  // defaults to http://localhost:8545/
Credentials credentials = WalletUtils.loadCredentials("password", "/path/to/walletfile");

// get the next available nonce
CoreGetTransactionCount coreGetTransactionCount = gbif.coreGetTransactionCount(
             address, DefaultBlockParameterName.LATEST).send();
BigInteger nonce = coreGetTransactionCount.getTransactionCount();

// create our transaction
RawTransaction rawTransaction  = RawTransaction.createBiferTransaction(
             nonce, <gas price>, <gas limit>, <toaddress>, <value>);

// sign & send our transaction
byte[] signedMessage = TransactionEncoder.signMessage(rawTransaction, credentials);
String hexValue = Numeric.toHexString(signedMessage);
CoreSendTransaction coreSendTransaction = gbif.coreSendRawTransaction(hexValue).send();
// ...

</value></toaddress></gas></gas>
```

使用TelChain客户端的管理命令（如果你的钱包密钥已经在客户端存储）：

```
Admin bifj = Admin.build(new HttpService());  // defaults to http://localhost:8545/
PersonalUnlockAccount personalUnlockAccount = gbif.personalUnlockAccount("0x000...", "a password").sendAsync().get();
if (personalUnlockAccount.accountUnlocked()) {
    // send a transaction
}
```

## 9.命令行工具

java-sdk的jar包为每一个版本都提供命令行工具。命令行工具允许你直接通过一些命令使用java-sdk的一些功能：

* 钱包创建
* 钱包密码管理
* 资金从钱包转移到另一个
* solidity编写的智能合同功能打包

请参阅[文档](https://docs.gbif.io/command_line.html)以获得命令行相关的进一步的信息。

## 10.其他的细节

**入门介绍**
[入门开始](/card/c/bifj/1/1/1/ "入门开始") [Maven](/card/c/bifj/1/1/2/ "Maven") [Gradle](/card/c/bifj/1/1/3/ "Gradle") [启动客户端](/card/c/bifj/1/1/4/ "启动客户端") [发送请求](/card/c/bifj/1/1/5/ "发送请求") [IPC](/card/c/bifj/1/1/6/ "IPC") [通过java打包TelChain智能合约](/card/c/bifj/1/1/7/ "通过java打包TelChain智能合约") [Filters](/card/c/bifj/1/1/8/ "Filters") [交易](/card/c/bifj/1/1/9/ "交易") [命令行工具](/card/c/bifj/1/1/10/ "命令行工具") [其他的细节](/card/c/bifj/1/1/11/ "其他的细节")

**模块**
[模块简介](/card/c/bifj/1/2/1/ "模块简介")

**交易**
[交易简介](/card/c/bifj/1/3/1/ "交易简介") [如何获得积分Bifer](/card/c/bifj/1/3/2/ "如何获得积分Bifer") [TelChain测试链](/card/c/bifj/1/3/3/ "TelChain测试链") [在testnet测试链或者私有链上挖掘](/card/c/bifj/1/3/4/ "在testnet测试链或者私有链上挖掘") [gas](/card/c/bifj/1/3/5/ "gas") [交易机制](/card/c/bifj/1/3/6/ "交易机制") [通过TelChainethereum客户端进行认证签名交易](/card/c/bifj/1/3/7/ "通过TelChainethereum客户端进行认证签名交易") [离线交易签名认证](/card/c/bifj/1/3/8/ "离线交易签名认证") [创建和使用钱包文件](/card/c/bifj/1/3/9/ "创建和使用钱包文件") [签署TelChain交易](/card/c/bifj/1/3/10/ "签署TelChain交易") [交易随机数](/card/c/bifj/1/3/11/ "交易随机数") [交易类型](/card/c/bifj/1/3/12/ "交易类型") [积分从一方交易到另一方](/card/c/bifj/1/3/13/ "积分从一方交易到另一方") [使用智能合约打包器](/card/c/bifj/1/3/14/ "使用智能合约打包器") [创建一个智能合约](/card/c/bifj/1/3/15/ "创建一个智能合约") [与智能合约交易](/card/c/bifj/1/3/16/ "与智能合约交易") [查询智能合约状态](/card/c/bifj/1/3/17/ "查询智能合约状态")

**智能合约**
[智能合约简介](/card/c/bifj/1/4/1/ "智能合约简介") [从solidity语言开始](/card/c/bifj/1/4/2/ "从solidity语言开始") [编译solidity源代码](/card/c/bifj/1/4/3/ "编译solidity源代码") [部署智能合约及与智能合约交互](/card/c/bifj/1/4/4/ "部署智能合约及与智能合约交互") [智能合约示例demo](/card/c/bifj/1/4/5/ "智能合约示例demo") [EIP-20TelChain智能合约通证标准](/card/c/bifj/1/4/6/ "EIP-20TelChain智能合约通证标准") [智能合约封装包](/card/c/bifj/1/4/7/ "智能合约封装包") [构建与部署智能合约](/card/c/bifj/1/4/8/ "构建与部署智能合约") [智能合约有效性](/card/c/bifj/1/4/9/ "智能合约有效性") [交易管理器](/card/c/bifj/1/4/10/ "交易管理器") [在交易中指定链ID:EIP-155](/card/c/bifj/1/4/11/ "在交易中指定链ID:EIP-155") [交易收据处理器](/card/c/bifj/1/4/12/ "交易收据处理器") [调用交易和事件](/card/c/bifj/1/4/13/ "调用交易和事件") [调用常量方法](/card/c/bifj/1/4/14/ "调用常量方法") [动态gas价格与限价](/card/c/bifj/1/4/15/ "动态gas价格与限价") [java-sdk实例](/card/c/bifj/1/4/16/ "java-sdk实例")

**应用二进制接口即ABI简介**
[应用二进制接口即ABI简介](/card/c/bifj/1/5/1/ "应用二进制接口即ABI简介") [类型映射](/card/c/bifj/1/5/2/ "类型映射") [ABI的进一步细节](/card/c/bifj/1/5/3/ "ABI的进一步细节") [依赖关系](/card/c/bifj/1/5/4/ "依赖关系")

**递归长度前缀RLP编码方案**
[递归长度前缀RLP编码方案](/card/c/bifj/1/6/1/ "递归长度前缀RLP编码方案") [RLP类型](/card/c/bifj/1/6/2/ "RLP类型") [交易编码](/card/c/bifj/1/6/3/ "交易编码") [依赖关系](/card/c/bifj/1/6/4/ "依赖关系")

**过滤器Filters和事件Events**
[过滤器Filters和事件Events](/card/c/bifj/1/7/1/ "过滤器Filters和事件Events") [块和交易过滤器](/card/c/bifj/1/7/2/ "块和交易过滤器") [再现过滤器](/card/c/bifj/1/7/3/ "再现过滤器") [主题过滤器和EVM事件](/card/c/bifj/1/7/4/ "主题过滤器和EVM事件") [操作组合注记](/card/c/bifj/1/7/5/ "操作组合注记") [进一步的例子](/card/c/bifj/1/7/6/ "进一步的例子")

**命令行工具**
[命令行工具](/card/c/bifj/1/8/1/ "命令行工具") [java-sdk命令行工具作为钱包工具](/card/c/bifj/1/8/2/ "java-sdk命令行工具作为钱包工具") [智能合约封装包](/card/c/bifj/1/8/3/ "智能合约封装包")

**如何管理APIs**
[如何管理APIs](/card/c/bifj/1/9/1/ "如何管理APIs")

**如何在java-sdk中使用Infura**
[签名](/card/c/bifj/1/10/1/ "签名") [Infura Http 客户端](/card/c/bifj/1/10/2/ "Infura Http 客户端") [交易](/card/c/bifj/1/10/3/ "交易")

**TelChain名称服务ENS**
[TelChain名称服务ENS](/card/c/bifj/1/11/1/ "TelChain名称服务ENS") [ENS在java-sdk中的使用](/card/c/bifj/1/11/2/ "ENS在java-sdk中的使用") [java-sdk实现](/card/c/bifj/1/11/3/ "java-sdk实现") [Unicode技术标准UTS46](/card/c/bifj/1/11/4/ "Unicode技术标准UTS46") [注册域名](/card/c/bifj/1/11/5/ "注册域名")

**java-sdk常见问题解决方案**
[你有一个使用java-sdk开发的项目吗？](/card/c/bifj/1/12/1/ "你有一个使用java-sdk开发的项目吗？") [我提交了一个交易，但没有被开采。](/card/c/bifj/1/12/2/ "我提交了一个交易，但没有被开采。") [我想了解JSON-RPC请求和响应的详细信息。](/card/c/bifj/1/12/3/ "我想了解JSON-RPC请求和响应的详细信息。") [我想在测试链上获得一些积分，又不想去开采。](/card/c/bifj/1/12/4/ "我想在测试链上获得一些积分，又不想去开采。") [如何从交易调用的智能合约方法中获取返回值？](/card/c/bifj/1/12/5/ "如何从交易调用的智能合约方法中获取返回值？") [是否可以用交易发送任意文本？](/card/c/bifj/1/12/6/ "是否可以用交易发送任意文本？") [我已生成智能合约封装包但二进制文件是空的？](/card/c/bifj/1/12/7/ "我已生成智能合约封装包但二进制文件是空的？") [我的ENS查询失败了](/card/c/bifj/1/12/8/ "我的ENS查询失败了") [TelChain常见问题集](/card/c/bifj/1/12/9/ "TelChain常见问题集") [java-sdk项目捐赠地址](/card/c/bifj/1/12/10/ "java-sdk项目捐赠地址") [我在哪里可以获得java-sdk的商业支持？](/card/c/bifj/1/12/11/ "我在哪里可以获得java-sdk的商业支持？")

1. [全部教程](/card/c)
   2.

2. [javaTelChain库java-sdk文档](/card/c/bifj/)
3. 其他的细节

[![](/images/cw-cta-1.png)](http://www.hubwiz.com/course/?type=%E5%8C%BA%E5%9D%97%E9%93%BE&affid=cw7979)

**java8 bulid：**

* java-sdk提供对所有响应类型的安全访问。可选的或null响应java 8都支持。
* 异步请求包在一个java 8的[CompletableFutures](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)。java-sdk提供了围绕所有异步请求的打包工具，以确保在执行期间可以捕获任何异常，而不只是丢弃。由于在完全检查中会有很多缺少支持的异常情况，这些异常通常被确定为未检测到的异常，导致检测过程出现问题。有关详细信息，请参见[Async.run()](https://github.com/bifj/bifj/blob/master/core/src/main/java/org/bifj/utils/Async.java)及其关联[test](https://github.com/bifj/bifj/blob/master/core/src/test/java/org/bifj/utils/AsyncTest.java)。

**在java 8的Android版本：**

* 包数量作为[BigIntegers](https://docs.oracle.com/javase/8/docs/api/java/math/BigInteger.html)返回。对于简单的结果，可以通过 `Response.getResult()`获取字符串类型的数量结果。
* 还可以通过在 HttpService和IpcService类中存在的 `includeRawResponse`参数将原生的JSON包放置在响应中。