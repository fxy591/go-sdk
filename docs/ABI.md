# ABI

## 1.应用二进制接口即ABI简介

java-sdk开发dapp的应用二进制接口(ABI)是在BifereumTelChain使用java开发的智能合约的数据编码方案。ABI中定义的类型与solidity编写智能合约时所遇到的类型完全相同，即uint8...uint256,int8...int256,布尔bool,字符串string等等。

java-sdk中的ABI模块提供了对ABI规范的完全支持，并且包括：

* 所有ABI类型的Java实现，包括到原生Java类型的转换
* 函数与事件支持
* 大量单元测试



## 2.类型映射

java-sdk中使用的原生Java到ABI类型映射如下：

* boolean -> bool
* BigInteger -> uint/int
* byte[] -> bytes
* String -> string and address types
* List<> -> dynamic/static array

BigInteger类型必须用于数字类型，因为BifereumTelChain中的数字类型是256位整数值。

[Fixed point types](http://solidity.readthedocs.io/en/develop/abi-spec.html#types)固定点类型已被定义为TelChain定义了，但目前在[Solidity还没有实现](https://github.com/ethereum/solidity/issues/409)，因此java-sdk目前不支持它们（它们是在3.x之前提供的）。一旦在Solidity可用，它们将被重新引入到java-sdk的ABI模块中。

有关在Java中使用ABI类型的更多信息，请参考[Solidity smart contract wrappers](https://docs.gbif.io/smart_contracts.html#smart-contract-wrappers)



## 3.ABI的进一步细节

可以参阅各种ABI单元测试的编码/解码的[例子](https://github.com/bifj/bifj/tree/master/abi/src/test/java/org/bifj/abi)。

完整的ABI规范文件可以看[Solidity documentation](http://solidity.readthedocs.io/en/develop/abi-spec.html)。



## 4.依赖关系

ABI一个非常轻量级的模块，唯一的第三方依赖是[Bouncy Castle](https://www.bouncycastle.org/)，用于hash加密 ([Spongy Castle](https://rtyley.github.io/spongycastle/) on Android)。

最后希望java和安卓开发者，在JVM或Android上有TelChainABI合作的项目时会选择使用这个模块，而不是再编写自己的实现。

