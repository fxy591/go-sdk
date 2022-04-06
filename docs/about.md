# 密码学

## 一、 基础知识

### 对称加密三要素

1. 明文plain text

2. 加密算法：加密规则(常见des,3des,aes)
3. 密钥：根据算法的不同，长度不同

### 对称解密三要素

1. 密文
2. 解密算法：可能与加密算法相同，也可以不能（异或加密：相同；凯撒密码：不同）
3. 密钥：与加密相同

### 加密方式分类及特点

#### 1. 对称加密

![image-20220114140457410](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114140457410.png)

 1. 密钥数量：1个

 2. 特点：

    * 加密效率高
    * 双方使用的密钥相同

 3. 安全性：

    相对于非对称加密 不安全

 4. 使用情况：

    主流的加密方式

#### 2. 非对称加密

![image-20220114140809050](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114140809050.png)

**每个人有两把钥匙（公钥，私钥）；如果A给B发送消息，使用b的公钥进行加密；b的公钥加密的信息只有b的私钥可以解密。**

 	1. 密钥数量：2个 
 	 * 公钥
 	   * 任何人都可以持有
 	   * 一般用于加密作用
 	 * 私钥
 	   * 只有自己持有
 	   * 一般不用于加密，而是用于签名
 	   * 签名的数据，可以证明是私钥持有人发送的数据
 	   * 私钥签名的数据，私钥持有人无法否认自己发送过这个消息
 	2. 特点：
 	 * 公钥加密的只有自己的私钥能解开
 	 * 加密、解密效率很低，一般不做大量数据使用
 	3. 安全性：高
 	4. 使用情况：
 	 * 一般配合对称加密一起使用
 	 * 建立连接，先使用非对称加密协商对称加密算法和密钥，然后使用对称加密进行后续加解密

### 加密领域常识

- 不要使用保密的密码算法
- 使用低强度的密码比不进行任何加密更危险
- 任何密码总有一天都会被破解==(非对称加密算法由英国安全局1973发明，后由RSA于1977公开)==
- 密码只是信息安全的一部分（社会工程学，钓鱼）

### 计算机单位介绍

位：bit，0或1，最小的单位

==字节：Byte，1Byte = 8bit==

千字节：KByte，1K = 1024B， （硬盘里1K = 1000B）

兆字节：MByte，1M = 1024K = 1024B * 1024 =  1024 *  1024 * 8 bit

1 GB = 1024M = 1024K = 1024B =1024 * 8 bit

1TB = 1024GB

1PB = 1024TB

==小贴士，手机的下载速度一般是Mbit，这个速度不是我们电脑上常说的兆，需要除以8。==

## 二、编解码与加解密

### 编码和解码

编码：把字符转成二进制比特流的过程

解码：把比特流转换成可读字符的过程

#### 常用编解码方式

1. gob包-》go内置的编解码包
2. html编码
3. json编解码
4. binary包-》go内置的编解码包

### 加解密

- 加解密就是对比特流进行编解码
- 加密：对明文的比特序列进行编码，得到密文的比特序列(字节流)

![image-20220114154404165](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114154404165.png)

## 三、加密与解密算法

### 对称加密算法

#### 1. DES：Data Encryption Standard

##### 概述

> DES（Data Encryption Standard）**是1977年美国联邦信息处理标准（FIPS）中所采用的一种对称密码（FIPS46.3）。DES一直以来被美国以及其他国家的政府和银行等广泛使用。然而，随着计算机的进步，现在DES已经能够被暴力破解，强度大不如前了。
>
> RSA公司举办过破泽DES密钥的比赛（DESChallenge），我们可以看一看RSA公司官方公布的比赛结果：
>
> - 1997年的DES Challenge1中用了96天破译密钥
> - 1998年的DES ChallengeIl-I中用了41天破译密钥
> - 1998年的DES ChallengeII-2中用了56小时破译密钥
> - 1999年的DES ChallengeIll中只用了22小时15分钟破译密钥
>
> 由于DES的密文可以在短时间内被破译，因此除了用它来解密以前的密文以外，现在我们不应该再使用DES了。



>  补充:
>
>  DES算法为[密码体制](https://baike.baidu.com/item/%E5%AF%86%E7%A0%81%E4%BD%93%E5%88%B6/1576830)中的对称密码体制，又被称为美国[数据加密标准](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%8A%A0%E5%AF%86%E6%A0%87%E5%87%86)，是1972年美国IBM公司研制的对称密码体制[加密算法](https://baike.baidu.com/item/%E5%8A%A0%E5%AF%86%E7%AE%97%E6%B3%95/2816213)。 明文按64位进行分组，[密钥](https://baike.baidu.com/item/%E5%AF%86%E9%92%A5/101144)长64位，密钥事实上是56位参与DES运算（第8、16、24、32、40、48、56、64位是校验位， 使得每个密钥都有奇数个1）分组后的明文组和56位的密钥按位替代或交换的方法形成密文组的加密方法。

##### 加密和解密

DES是一种将64比特的明文加密成64比特的密文的对称密码算法，==它的密钥长度是56比特==。尽管<font color="red">从规格上来说，DES的密钥长度是64比特，但由于每隔7比特会设置一个用于错误检查的比特，因此实质上其密钥长度是56比特</font>。

<font color="red">DES是以64比特的明文（比特序列）为一个单位来进行加密的</font>，**这个64比特的单位称为分组**。一般来说，以分组为单位进行处理的密码算法称为**分组密码（blockcipher）**，DES就是分组密码的一种。

DES每次只能加密64比特的数据，如果要加密的明文比较长，就需要对DES加密进行迭代（反复），而迭代的具体方式就称为模式（mode）。

![image-20220114160519804](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114160519804.png)

##### 特点

1. 不安全，不建议使用
2. 秘钥：8字节（64 - 8 = 56比特，每七个bit就设置一个校验位）
2. 加密时，会对明文进行分组，分组长度是8bytes，得到的密文也是8bytes为1组

![image-20220114155217013](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114155217013.png)

#### 2. 3DES ：Triple Data Encryption Standard

##### 概述

> 现在DES已经可以在现实的时间内被暴力破解，因此我们需要一种用来替代DES的分组密码，三重DES就是出于这个目的被开发出来的。
>
> **三重DES（triple-DES）是为了增加DES的强度，==将DES重复3次所得到的一种密码算法==，通常缩写为3DES**。

##### 加密与解密

三重DES的加解密机制如图所示：

![image-20220114160740477](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114160740477.png)



![image-20220114161538288](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114161538288.png)

> 明文经过三次DES处理才能变成最后的密文，由于**DES密钥的长度实质上是56比特**，因此<font color="red">三重DES的密钥长度就是56×3=168比特, 加上用于错误检测的标志位8x3, 共192bit</font>。
>
> 从上图我们可以发现，三重DES并不是进行三次DES加密（加密-->加密-->加密），而是<font color="red">**加密-->解密-->加密**</font>的过程。在加密算法中加人解密操作让人感觉很不可思议，实际上这个方法是IBM公司设计出来的，目的是为了让三重DES能够兼容普通的DES。
>
> 
>
> <font color="red">当三重DES中所有的密钥都相同时，三重DES也就等同于普通的DES了</font>。这是因为在前两步加密-->解密之后，得到的就是最初的明文。因此，以前用DES加密的密文，就可以通过这种方式用三重DES来进行解密。也就是说，三重DES对DES具备向下兼容性。
>
> 
>
> 如果密钥1和密钥3使用相同的密钥，而密钥2使用不同的密钥（也就是只使用两个DES密钥），这种三重DES就称为DES-EDE2。EDE表示的是加密（Encryption) -->解密（Decryption)-->加密（Encryption）这个流程。
>
> 
>
> 密钥1、密钥2、密钥3全部使用不同的比特序列的三重DES称为DES-EDE3。
>
> 
>
> 尽管三重DES目前还被银行等机构使用，但其处理速度不高，而且在安全性方面也逐渐显现出了一些问题。



##### 特点

1. 加密过程： 加密 -》 解密-》 加密
   * 中间使用解密的原因是为了兼容以前的DES
   * 解密过程是以解密的方式进行加密，整体还是三次加密
   * 秘钥：8bytes * 3 = 24bytes， =》24 * 8 = 192bit
   * 数据分组长度：与DES相同，8bytes（64比特）
   * **加解密效率低（过渡的加密算法）**
   * **少用**
2. 解密过程：解密 -》 加密 -》 解密 
3. 3个密钥
   * 如果密钥1与密钥2相同，或者2与3相同，这就相当于DES，与前面兼容
   * 如果1与3相同，相当于有两个密钥，专业名字：3DES-EDE2 
   * 如果三个密钥都不相同，专业名字：3DES-EDE3

#### 3. AES：Advance Encryption Standard

##### 概述

> AES（Advanced Encryption Standard）是取代其前任标准（DES）而成为新标准的一种对称密码算法。全世界的企业和密码学家提交了多个对称密码算法作为AES的候选，最终在2000年从这些候选算法中选出了一种名为==**Rijndael**==的对称密码算法，并将其确定为了AES。
>
> Rijndael是由比利时密码学家Joan Daemen和Vincent Rijmen设汁的分组密码算法，今后会有越来越多的密码软件支持这种算法。
>
> **==Rijndael的分组长度为128比特==**，密钥长度可以以32比特为单位在128比特到256比特的范围内进行选择（不过==**在AES的规格中，密钥长度只有128、192和256比特三种**==）。

##### 特点：

1. 秘钥长度：可选的，16bytes, 24bytes, 32bytes， （des：8bytes, 3des: 24bytes）
2. 分组长度：16bytes， （des合约3des:8bytes）
3. 加解密效率高，推荐使用

#### 4. 小结

##### 	1. 对称加密特点

- 加密与解密使用的密钥相同。
- 在一定程度上实现了数据的机密性，且简单、快速。
- 但是由于算法一般都是公开的，因此机密性几乎完全依赖于密钥。
- 同一发送方与不同接收方进行通信时应使用不同的密钥(1 -> n)，防止数据被窃听或拦截后被解密。

##### 2. 牢记知识点

DES ： key -》8字节，  分组-》8字节

3DES：key-》24字节， 分组-》8字节

AES：key-》16字节（128），24字节（192），32字节（256）， 分组-》16字节



###### 模式：

对于需要填充的模式，要按照分组长度来填充

对于需要iv向量的，iv的长度要与分组一致

##### 3. 存在问题

1. 当需要加密的明文长度超过分组长度时，如何加密？
2. 用对称密码进行通信时，还会出现密钥的配送问题，即如何将密钥安全地发送给接收者？

### 分组模式

#### 1. 基本知识

***为什么要分组？***

被加密的数据可以很大，需要对数据进行迭代的加密，所以用对数据进行分组

##### 概述

```go
"分组密码的模式 -- 分组密码是如何迭代的"
```

> 本章中我们将探讨一下分组密码的模式
>
> 我们在上一章中介绍的DES和AES都属于分组密码，它们只能加密固定长度的明文。如果需要加密任意长度的明文，就需要对分组密码进行迭代，而分组密码的迭代方法就称为分组密码的“模式”。
>
> 分组密码有很多种模式，如果模式的选择不恰当，就无法保证机密性。例如，如果使用ECB模式，明文中的一些规律就可以通过密文被识别出来。
>
> 分组密码的主要模式（ECB、CBC、CFB、OFB、CTR），最后再来考察一下到底应该使用哪一种模式。

##### 分组密码

> **分组密码（blockcipher）**是每次只能处理特定长度的一块数据的一类密码算法，这里的一块"就称为分组（block）。此外，一个分组的比特数就称为分组长度（blocklength）。
>
> 例如，**DES和三重DES的分组长度都是64比特**。这些密码算法一次只能加密64比特的明文．并生成64比特的密文。
>
> **AES的分组长度可以从128比特、192比特和256比特中进行选择。当选择128比特的分组长度时，AES一次可加密128比特的明文，并生成128比特的密文。**

##### 模式

> **分组密码算法只能加密固定长度的分组，但是我们需要加密的明文长度可能会超过分组密码的分组长度，这时就需要对分组密码算法进行迭代，以便将一段很长的明文全部加密。而迭代的方法就称为分组密码的模式（mode）**。
>
> 话说到这里，很多读者可能会说：“如果明文很长的话，将明文分割成若干个分组再逐个加密不就好了吗？”事实上可没有那么简单。将明文分割成多个分组并逐个加密的方法称为ECB模式，这种模式具有很大的弱点（稍后讲解）。对密码不是很了解的程序员在编写加密软件时经常会使用ECB模式，但这样做会在不经意间产生安全漏洞，**因此大家要记住千万不能使用ECB模式**。
>
> 模式有很多种类，分组密码的主要模式有以下5种：
>
> - **ECB模式**：Electronic Code Book mode（电子密码本模式）       **不适用 淘汰**
> - **CBC模式**：Cipher Block Chaining mode（密文分组链接模式） **常用**
> - **CFB模式**：Cipher FeedBack mode（密文反馈模式）                  **支持，不建议使用**
> - **OFB模式**：Output FeedBack mode（输出反馈模式）                **支持，不建议使用**
> - **CTR模式**：CounTeR mode（计数器模式）                                   **常用**

##### 明文分组和密文分组

> 在介绍模式之前，我们先来学习两个术语。
>
> **明文分组: **是指分组密码算法中作为加密对象的明文。明文分组的长度与分组密码算法的分组长度是相等的。
>
> **密文分组: **是指使用分组密码算法将明文分组加密之后所生成的密文。

![image-20220114163248747](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114163248747.png)

* **密文分组的长度与明文分组一致**

##### 加密算法和分组模式的关系

![加密算法与分组模式的关系](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day1/03-资料/加密算法与分组模式的关系.jpg)

#### 2. ECB模式

> ECB(Electronic Code Book, 电子密码本)模式是最简单的加密模式，<font color="red">明文消息被分成固定大小的块（分组），并且每个块被单独加密。</font>  每个块的加密和解密都是独立的，且使用相同的方法进行加密，所以可以进行并行计算，==但是这种方法一旦有一个块被破解，使用相同的方法可以解密所有的明文数据==，
>
> 特点：<font color="red">安全性比较差。  适用于数据较少的情形，加密前需要把明文数据填充到块大小的整倍数。</font>

![image-20220114164415061](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114164415061.png)

![image-20220114164627792](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114164627792.png)

###### 加密效果

![image-20220114164521810](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114164521810.png)



###### ![ECB填充](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day1/03-资料/ECB填充.jpg)<font color ="red">特点：</font>

- 加密效率高，但是不安全，加密不彻底
- 需要对数据进行分组后做数据填充
- 每一个分组独立的进行加解密
- 只要有一个分组被破解，所有的分组都被破解
- 不用使用，go语言没有支持这种分组模式
- 分组长度由加密算法决定（DES：8bytes, AES: 16bytes）

#### 3. CBC模式

##### 基础补充

|      | 按位操作符号 | 逻辑操作符号 |
| :--: | :----------: | :----------: |
|  与  |      &       |      &&      |
|  或  |      \|      |     \|\|     |
|  非  |      ~       |      ！      |
| 异或 |      ^       |      无      |

A & B， 按位与

0000， 1000  => 8

0000， 1001 =>9

 & 有一个为0则为0

0000， 1000 => 8

```go
逻辑与：A && B   ==> AND

if (c == 0 && a == b) {
    fmt.print("hello world")
}
```

或

A | B， 按位或

0000， 1000 => 8

0000， 1001 => 9

| 有一个为1则为1

0000， 1001 => 9

```go
逻辑或：A || B  ==> OR

if (c == 0 || a == b) {
	
}
```

异或：==规则：相同为0，不同为1， （同龄人）==

A⊕B， 异或操作

0000， 1000   =》 8

0000， 1001   =》9

⊕

0000， 0001  =》 1

###### 异或加密解密

* 加密过程：

0000， 1000   =》 8  ====> 明文

0000， 1001   =》9  ====> 秘钥

​	       ⊕  =》 算法

0000， 0001  =》 1  =》密文

* 解密过程：

0000， 0001  =》 1  =》密文

0000， 1001   =》9  ====> 秘钥

​	      ⊕   =》 算法

0000，1000  =》8 =》明文

##### CBC-密文分组链接模式 （先异或再加密）

> CBC(Cipher Block Chaining, 密文分组链接)<font color="red">模式中每一个分组要先和前一个分组加密后的数据进行XOR异或操作，然后再进行加密</font>。  这样每个密文块依赖该块之前的所有明文块，为了保持每条消息都具有唯一性，<font color="red">第一个数据块进行加密之前需要用初始化向量IV进行异或操作</font>。  <font color="blue">CBC模式是一种最常用的加密模式，它主要缺点是加密是连续的，不能并行处理，并且与ECB一样消息块必须填充到块大小的整倍数。</font>

![image-20220114171041341](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114171041341.png)

<font color ="red">特点：</font>

- 数据分组长度根据算法而定
- 需要提供初始化向量（Initialize Vector），要求长度必须与分组长度相同
- 每一个密文都是下一次加密操作的输入
- 不能够并行加密，可以并行解密
- 加密强度高
- 如果数据切割后长度不满足需求，需要对数据进行填充。

**ECB与CBC模式的比较**

![image-20220114171429730](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114171429730.png)

#### 4. CFB 模式（先加密再异或）

> CFB模式的全称是Cipher FeedBack模式（密文反馈模式）。在CFB模式中，<font color="red">前一个分组的密文加密后和当前分组的明文XOR异或操作生成当前分组的密文</font>。
>
> 所谓反馈，这里指的就是返回输入端的意思，即前一个密文分组会被送回到密码算法的输入端。
>
> CFB模式的解密和CBC模式的加密在流程上其实是非常相似的。

![image-20220114193017237](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114193017237.png)

![image-20220114193057797](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114193057797.png)

![image-20220114193606659](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114193606659.png)

<font color ="red">特点：</font>

- 分组长度取决于加密算法。
- 需要初始化向量，长度必须与明文分组相同。
- 先对密文进行加密，然后再与明文分组进行异或。（CBC是先异或，再加密）
- ==由于没有直接对明文分组进行加密，所以不需要填充==
- 注意，解密的时候，是对初始向量进行加密操作，这样才能得到同样的数据

**CBC与CFB比较**

![image-20220114193732136](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114193732136.png)

#### 5. OFB

> OFB式的全称是Output-Feedback模式（输出反馈模式）。在OFB模式中，密码算法的输出会反馈到密码算法的输入中， 即上一个分组密码算法的输出是当前分组密码算法的输入（下图）。
>
> OFB模式并不是通过密码算法对明文直接进行加密的，而是通过将 “明文分组" 和 “密码算法的输出” 进行XOR来产生 “密文分组” 的，在这一点上OFB模式和CFB模式非常相似。

![image-20220114194049594](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114194049594.png)

<font color ="red">特点：</font>

- 分组长度取决于加密算法
- 不断对初始向量的输出进行加密，从而得到数据来源
- 不需要进行数据填充

**CFB与OFB对比**

![image-20220114194312298](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114194312298.png)

#### 6. CTR

> CTR模式的全称是CounTeR模式（计数器模式）。<font color="red">CTR摸式是一种通过将逐次累加的计数器进行加密来生成密钥流的流密码</font>（下图）。
>
> CTR模式中，每个分组对应一个逐次累加的计数器，并通过对计数器进行加密来生成密钥流。也就是说，最终的密文分组是通过将计数器加密得到的比特序列，与明文分组进行XOR而得到的。

![image-20220114195957688](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114195957688.png)

![image-20220114200039477](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114200039477.png)

<font color ="red">特点：</font>

- 分组长度取决于加密算法
- 不需要填充
- 可以并行加密和并行解密，效率高，推荐使用

##### 计数器的生成方法

> 每次加密时都会生成一个不同的值（nonce）来作为计数器的初始值。当分组长度为128比特（16字节）时，计数器的初始值可能是像下面这样的形式。

> 其中**前8个字节为nonce（随机数）**，这个值在每次加密时必须都是不同的，后8个字节为分组序号，这个部分是会逐次累加的。在加密的过程中，计数器的值会产生如下变化：

![image-20220114200204722](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114200204722.png)

> 按照上述生成方法，可以保证计数器的值每次都不同。由于计数器的值每次都不同，因此每个分组中将计数器进行加密所得到的密钥流也是不同的。也是说，这种方法就是用分组密码来模拟生成随机的比特序列。



OFB与CTR对比

![image-20220114200250601](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114200250601.png)

#### 7. 小结

![image-20220114200642589](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114200642589.png)

需要填充：ECB CBC

不需要填充：CFB OFB CTR

#### 8. 代码实现

##### des + CBC

手册: https://studygolang.com/pkgdoc

###### 1. 步骤分析

```go
package main

import "fmt"

/*
需求：算法：des ， 分组模式：CBC

des :
秘钥：8bytes
分组长度：8bytes

cbc:
1. 提供初始化向量，长度与分组长度相同，8bytes
2. 需要填充


加密分析

1. 创建并返回一个使用DES算法的cipher.Block接口。

	func NewCipher(key []byte) (cipher.Block, error)
	- 包名：des
	- 参数：秘钥，8bytes
	- 返回值：一个cipher.Block接口

	type Block interface {
		// 返回加密字节块的大小
		BlockSize() int
		// 加密src的第一块数据并写入dst，src和dst可指向同一内存地址
		Encrypt(dst, src []byte)
		// 解密src的第一块数据并写入dst，src和dst可指向同一内存地址
		Decrypt(dst, src []byte)
	}

2. 进行数据填充
//TODO


3. 引入CBC模式, 返回一个密码分组链接模式的、底层用b加密的BlockMode接口，初始向量iv的长度必须等于b的块尺寸。
	func NewCBCEncrypter(b Block, iv []byte) BlockMode
	- 包名：cipher
	- 参数1：cipher.Block
	- 参数2：iv， initialize vector
	- 返回值：分组模式，里面提供加解密方法

	type BlockMode interface {
		// 返回加密字节块的大小
		BlockSize() int
		// 加密或解密连续的数据块，src的尺寸必须是块大小的整数倍，src和dst可指向同一内存地址
		CryptBlocks(dst, src []byte)
	}

解密分析


*/

```

###### 2. 测试框架

```go
//输入明文，秘钥，输出密文
func desCBCEncrypt(src, key []byte) []byte {
	//TODO
	fmt.Printf("加密开始，输入的数据为：%s\n", src)

	fmt.Printf("加密结束，加密数据为%x\n", src)
	return []byte{}
}

func main() {
	src := []byte("12345678")
	key := []byte("12345678")

	cipherData := desCBCEncrypt(src, key)

	fmt.Printf("cipherData : %x\n", cipherData)

}

```

###### 3. 实现加密函数-无填充

```go
//输入明文，秘钥，输出密文
func desCBCEncrypt(src, key []byte) []byte {
	fmt.Printf("加密开始，输入的数据为：%s\n", src)

	//1. 创建并返回一个使用DES算法的cipher.Block接口。
	//NewCipher(key []byte) (cipher.Block, error)
	block, err := des.NewCipher(key)

	fmt.Printf("block size : %d\n", block.BlockSize())

	if err != nil {
		panic(err)
	}

	//2. 进行数据填充
	//TODO

	//3. 引入CBC模式, 返回一个密码分组链接模式的、底层用b加密的BlockMode接口，初始向量iv的长度必须等于b的块尺寸。
	//func NewCBCEncrypter(b Block, iv []byte) BlockMode

	iv := bytes.Repeat([]byte("1"), block.BlockSize())

	blockMode := cipher.NewCBCEncrypter(block, iv)

	//4. 加密操作
	blockMode.CryptBlocks(src /*加密后的密文*/ , src /*明文*/)

	fmt.Printf("加密结束，加密数据为%x\n", src)
	return src
}
```

![image-20220114220321877](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114220321877.png)

###### 4.填充函数实现

* 填充逻辑分析

![填充逻辑分析](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day1/03-资料/填充逻辑分析.png)

```go
//填充函数, 输入明文, 分组长度, 输出：填充后的数据
func paddingInfo(src []byte, blockSize int) []byte {
	//1. 得到明文的长度
	length := len(src)

	//2. 需要填充的数量

	remains := length % blockSize        //3
	paddingNumber := blockSize - remains //5

	//3. 把填充的数值转换为字符
	s1 := byte(paddingNumber) // '5'

	//4. 把字符拼成数组
	s2 := bytes.Repeat([]byte{s1}, paddingNumber) //[]byte{'5', '5', '5', '5, '5'}

	//5. 把拼成的数组追加到src后面
	srcNew := append(src, s2...)

	//6. 返回新的数组
	return srcNew
}

```

* 函数调用

![image-20220114224145840](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114224145840.png)

* 运行结果

![image-20220114224113681](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114224113681.png)

###### 5. 解密步骤分析

```go
解密分析
1. 创建并返回一个使用DES算法的cipher.Block接口。

	func NewCipher(key []byte) (cipher.Block, error)
	- 包名：des
	- 参数：秘钥，8bytes
	- 返回值：一个cipher.Block接口

	type Block interface {
		// 返回加密字节块的大小
		BlockSize() int
		// 加密src的第一块数据并写入dst，src和dst可指向同一内存地址
		Encrypt(dst, src []byte)
		// 解密src的第一块数据并写入dst，src和dst可指向同一内存地址
		Decrypt(dst, src []byte)
	}


2. 返回一个密码分组链接模式的、底层用b解密的BlockMode接口，初始向量iv必须和加密时使用的iv相同。
	func NewCBCDecrypter(b Block, iv []byte) BlockMode
	- 包名：cipher
	- 参数1：cipher.Block
	- 参数2：iv， initialize vector
	- 返回值：分组模式，里面提供加解密方法

	type BlockMode interface {
		// 返回加密字节块的大小
		BlockSize() int
		// 加密或解密连续的数据块，src的尺寸必须是块大小的整数倍，src和dst可指向同一内存地址
		CryptBlocks(dst, src []byte)
	}

3. 解密操作

4. 去除填充
//TODO
*/
```

###### 6. 解密函数-未去除填充

```go
//输入密文，秘钥，得到明文
func desCBCDecrypt(cipherData, key []byte) []byte {
	fmt.Printf("解密开始，输入的数据为：%x\n", cipherData)

	//1. 创建并返回一个使用DES算法的cipher.Block接口。
	//NewCipher(key []byte) (cipher.Block, error)
	block, err := des.NewCipher(key)

	fmt.Printf("block size : %d\n", block.BlockSize())

	if err != nil {
		panic(err)
	}

	//3. 引入CBC模式

	iv := bytes.Repeat([]byte("1"), block.BlockSize())

	blockMode := cipher.NewCBCDecrypter(block, iv)

	//4. 解密操作
	blockMode.CryptBlocks(cipherData /*解密后的明文*/ , cipherData /*密文*/)

	fmt.Printf("解密结束，解密数据为%s\n", cipherData)

	//5. 去除填充
	//TODO

	return cipherData
}
```



```go
func main() {
	src := []byte("123456789123123123")
	key := []byte("12345678")

	cipherData := desCBCEncrypt(src, key)

	fmt.Printf("cipherData : %x\n", cipherData)

	plainText := desCBCDecrypt(cipherData, key)
	fmt.Printf("plainText str: %s\n", plainText)
	fmt.Printf("plainText hex: %x\n", plainText)
}
```

###### 7.去除填充函数

```go
//去除填充
func unpaddingInfo(plainText []byte) []byte {
	//1. 获取长度
	length := len(plainText)

	if length == 0 {
		return []byte{}
	}

	//2. 获取最后一个字符
	lastByte := plainText[length-1]

	//3. 将字符转换成数字
	unpaddingNumber := int(lastByte)

	//4. 切片获取需要的数据
	return plainText[:length-unpaddingNumber]

}
```

* 调用过程

![image-20220114230658848](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114230658848.png)

```go
func main() {
	src := []byte("不是一番寒彻骨，哪得梅花扑鼻香!!!")
	key := []byte("12345678")

	cipherData := desCBCEncrypt(src, key)

	fmt.Printf("cipherData : %x\n", cipherData)
	fmt.Printf("+++++++++++++++++++++++++\n")

	plainText := desCBCDecrypt(cipherData, key)
	fmt.Printf("plainText str: %s\n", plainText)
	fmt.Printf("plainText hex: %x\n", plainText)
}
```

* 运行结果

![image-20220114230641036](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114230641036.png)

##### aes + CTR

###### 1. 加密分析

```go
/*
需求： 使用aes， ctr

aes :
- 分组长度： 16
- 秘钥：16

ctr:
- 不需要填充
- 需要提供一个数字


1. 创建一个cipher.Block接口。参数key为密钥，长度只能是16、24、32字节，用以选择AES-128、AES-192、AES-256。
func NewCipher(key []byte) (cipher.Block, error)
- 包：aes
- 秘钥
- cipher.Block接口


2. 选择分组模式：ctr
返回一个计数器模式的、底层采用block生成key流的Stream接口，初始向量iv的长度必须等于block的块尺寸。
func NewCTR(block Block, iv []byte) Stream
- block
- iv
- 秘钥流

3. 加密操作
type Stream interface {
    // 从加密器的key流和src中依次取出字节二者xor后写入dst，src和dst可指向同一内存地址
    XORKeyStream(dst, src []byte)
}

*/
```

###### 2. 加密测试代码

```go
package main

import (
	"crypto/aes"
	"crypto/cipher"
	"bytes"
	"fmt"
)



func aesCTREncrypt(src, key []byte) []byte {

	//1. 创建一个cipher.Block接口。
	block, err := aes.NewCipher(key)
	if err != nil {
		panic(err)
	}

	fmt.Print("aes block size : ", block.BlockSize())

	iv := bytes.Repeat([]byte("1"), block.BlockSize())

	//2. 选择分组模式：ctr
	stream := cipher.NewCTR(block, iv)

	//3. 加密操作
	stream.XORKeyStream(src /*密文*/ , src /*明文*/)

	return src
}

func main() {

	src := []byte("不是一番寒彻骨，哪得梅花扑鼻香!!! 123456734523452345	")
	key := []byte("1234567887654321")

	cipherData := aesCTREncrypt(src, key)

	fmt.Printf("cipherData : %x\n", cipherData)

}
```

###### 3. 解密分析

```go
func aesCTRDecrypt(cipherData, key []byte) []byte {

	//1. 创建一个cipher.Block接口。
	block, err := aes.NewCipher(key)
	if err != nil {
		panic(err)
	}

	iv := bytes.Repeat([]byte("1"), block.BlockSize())

	//2. 选择分组模式：ctr
	stream := cipher.NewCTR(block, iv)

	//3. 解密操作
	stream.XORKeyStream(cipherData /*明文*/ , cipherData)

	return cipherData
}
```

###### 4.完整代码

```go
package main

import (
	"crypto/aes"
	"crypto/cipher"
	"bytes"
	"fmt"
)

/*
需求： 使用aes， ctr

aes :
- 分组长度： 16
- 秘钥：16

ctr:
- 不需要填充
- 需要提供一个数字


1. 创建一个cipher.Block接口。参数key为密钥，长度只能是16、24、32字节，用以选择AES-128、AES-192、AES-256。
func NewCipher(key []byte) (cipher.Block, error)
- 包：aes
- 秘钥
- cipher.Block接口


2. 选择分组模式：ctr
返回一个计数器模式的、底层采用block生成key流的Stream接口，初始向量iv的长度必须等于block的块尺寸。
func NewCTR(block Block, iv []byte) Stream
- block
- iv
- 秘钥流

3. 加密操作
type Stream interface {
    // 从加密器的key流和src中依次取出字节二者xor后写入dst，src和dst可指向同一内存地址
    XORKeyStream(dst, src []byte)
}

*/

func aesCTREncrypt(src, key []byte) []byte {
	fmt.Printf("明文： %s\n", src)

	//1. 创建一个cipher.Block接口。
	block, err := aes.NewCipher(key)
	if err != nil {
		panic(err)
	}

	fmt.Println("aes block size : ", block.BlockSize())

	iv := bytes.Repeat([]byte("1"), block.BlockSize())

	//2. 选择分组模式：ctr
	stream := cipher.NewCTR(block, iv)

	//3. 加密操作
	stream.XORKeyStream(src /*密文*/ , src /*明文*/)

	return src
}

func aesCTRDecrypt(cipherData, key []byte) []byte {

	//1. 创建一个cipher.Block接口。
	block, err := aes.NewCipher(key)
	if err != nil {
		panic(err)
	}

	iv := bytes.Repeat([]byte("1"), block.BlockSize())

	//2. 选择分组模式：ctr
	stream := cipher.NewCTR(block, iv)

	//3. 解密操作
	stream.XORKeyStream(cipherData /*明文*/ , cipherData)

	return cipherData
}

func main() {

	src := []byte("不是一番寒彻骨，哪得梅花扑鼻香!!! 123456734523452345	")
	key := []byte("1234567887654321")

	cipherData := aesCTREncrypt(src, key)

	fmt.Printf("cipherData : %x\n", cipherData)

	fmt.Printf("+++++++++++++++++++++++++\n")

	plainText := aesCTRDecrypt(cipherData, key)
	fmt.Printf("plainText ： %s\n", plainText)
}

```

* 运行截图

![image-20220114232514313](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220114232514313.png)



### 对称加密存在的问题

- 秘钥管理困难——当通信对象很多时会面临众多秘钥的有效管理问题。

![image-20220115181055917](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115181055917.png)

- 秘钥分发困难——对于一个新的数据通信对象，密钥怎样进行传输的问题。两个人如何保证秘钥不被窃取？

### 非对称加密

 #### RSA 

##### 1.基本知识

私钥：使用随机数按照一定规则生成的

公钥：由私钥推导而来。



**随机数据 =》算法 =》 私钥 =》 公钥**



私钥：只有自己持有，不可以向任何人传播

公钥：任何人都可以持有，公钥加密的数据只能被配套的私钥解开。

###### 生成RSA的秘钥对几个概念

- x509:https://baike.baidu.com/item/x509/1240109
- pem:https://blog.csdn.net/crjmail/article/details/79095385
- base64:

1. x509证书规范、pem、base64
   - pem编码规范 - 数据加密
   - base64 - 对数据编码, 可逆
     - 不管原始数据是什么, 将原始数据使用64个字符来替代
       - a-z  A-Z 0-9 + /
2. ASN.1抽象语法标记
3. PKCS1标准



##### 2.openssl生成方式

````shell
#目前主流密钥长度至少都是1024bits以上，低于1024bit的密钥已经不建议使用（安全问题）
OpenSSL> genrsa -out rsa_private_key.pem   1024  #生成私钥, 1024是密钥长度

#可以不指定私钥长度，默认是2048位，长度建议1024以上，这样安全！！


OpenSSL> rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem #生成公钥
OpenSSL> exit #退出OpenSSL程序
````

#### 常见使用场景

##### 1. 通信加密

公钥加密，私钥解密

##### 2. HTTPS

验证服务器，数字证书，使用ca认证公钥

##### 3. 签名（防止篡改）

哈希+非对称加密

##### 4. 网银U盾

验证client，U盾相当于私钥，公钥在服务端

##### 5. github ssh(secure shell)登录

ssh: https://blog.csdn.net/PeipeiQ/article/details/80702514

ssh: https://www.cnblogs.com/yyds/p/6992125.html

> - ssh是一种网络协议，主要用于计算机之间的加密登录与数据传输
> - ssh登录的时候没有ca认证，需要用户自己确认登录主机的指纹，点击yes后把远程主机的指纹存放到本地的know_hosts中，后续登录会跳过警告。
> - ssh-keygen -t rsa，演示

#### RSA生成规则

![rsa生成规则](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day2/03-资料/rsa生成规则.png)

##### 1. 规则描述

参考链接：https://www.cnblogs.com/jiftle/p/7903762.html

算法描述：
（1）选择一对不同的、足够大的素数p，q。
（2）计算n=pq。
（3）计算f(n)=(p-1)(q-1)，同时对p, q严加保密，不让任何人知道。
（4）找一个与f(n)互质的数e，且1<e<f(n)。
（5）计算d，使得de≡1 mod f(n)。这个公式也可以表达为d ≡e-1 mod f(n)
这里要解释一下，≡是数论中表示同余的符号。公式中，≡符号的左边必须和符号右边同余，也就是两边模运算结果相同。显而易见，不管f(n)取什么值，符号右边1 mod f(n)的结果都等于1；符号的左边d与e的乘积做模运算后的结果也必须等于1。这就需要计算出d的值，让这个同余等式能够成立。
（6）公钥KU=(e,n)，私钥KR=(d,n)。
（7）加密时，先将明文变换成0至n-1的一个整数M。若明文较长，可先分割成适当的组，然后再进行交换。设密文为C，则加密过程为：![img](http://img.hexun.com/2009-06-24/118958535.gif)。
（8）解密过程为：![img](http://img.hexun.com/2009-06-24/118958536.gif)。 

```go
选择P，Q：100~200位的大素数

N: P * Q
F(n) = (P -1 )(Q - 1)
选择E:  1 < E < F(n)
D*E === 1 mod F(n)， 同余运算符 =》 退出D
```

##### 2. demo

```go
P:3, Q:11
N  : 3 * 11 = 33
F(n) = (3 -1)(11-1) = 2 * 10 = 20
E: 1 < E < 20 ==> 选 3
( D * E ) %F(n) = 1
( D * 3 ) %20 = 1  => D选择7
公钥： {E, N} => {3, 33}
私钥： {D, N} => {7, 33}
```



加密时，先对明文进行转换，对数值进行加密

解密时，先根据规则解密，根据字符表转换为明文



#### GO创建RSA私钥

##### 1. 分析

```go
/*
需求: 生成并保存私钥，公钥


生成私钥分析：

1. GenerateKey函数使用随机数据生成器random生成一对具有指定字位数的RSA密钥。
func GenerateKey(random io.Reader, bits int) (priv *PrivateKey, err error)
- 参数1：随机数
- 参数2：秘钥长度
- 返回值：私钥


2. 要对生成的私钥进行编码处理， x509， 按照规则，进行序列化处理, 生成der编码的数据
MarshalPKCSPv1ublicKey将公钥序列化为PKCS格式DER编码。
func MarshalPKCSPublicKey(pub *PrivateKey) ([]byte, error)


3. 创建Block代表PEM编码的结构, 并填入der编码的数据
type Block struct {
    Type    string            // 得自前言的类型（如"RSA PRIVATE KEY"）
    Headers map[string]string // 可选的头项
    Bytes   []byte            // 内容解码后的数据，一般是DER编码的ASN.1结构
}

4. 将Pem Block数据写入到磁盘文件
func Encode(out io.Writer, b *Block) error


*/
```



私钥：

```go
type PrivateKey struct {
        PublicKey            // public part.
        D         *big.Int   // private exponent
        Primes    []*big.Int // prime factors of N, has >= 2 elements.

        // Precomputed contains precomputed values that speed up private
        // operations, if available.
        Precomputed PrecomputedValues
}
```



公钥：

```go
type PublicKey struct {
        N *big.Int // modulus
        E int      // public exponent
}
```



##### 2. 生成私钥代码

```go
package main

import (
	"crypto/rsa"
	"crypto/rand"
	"crypto/x509"
	"encoding/pem"
	"os"
	"fmt"
)

const privateKeyFile = "./privateRsaKey.pem"

//需求: 生成并保存私钥，公钥

func generateKeyPair(bits int) error {

	//生成私钥分析：
	//1. GenerateKey函数使用随机数据生成器random生成一对具有指定字位数的RSA密钥。
	//func GenerateKey(random io.Reader, bits int) (priv *PrivateKey, err error)
	//包： rsa
	//- 参数1：随机数, crypto/rand, 随机数生成器
	//- 参数2：秘钥长度
	//- 返回值：私钥
	privateKey, err := rsa.GenerateKey(rand.Reader, bits)

	if err != nil {
		return err
	}

	//
	//2. 要对生成的私钥进行编码处理， x509， 按照规则，进行序列化处理, 生成der编码的数据
	//MarshalPKCS1PrivateKey将公钥序列化为PKCS格式DER编码。

	// MarshalPKCS1PrivateKey converts a private key to ASN.1 DER encoded form.

	//func MarshalPKCS1PrivateKey(key *rsa.PrivateKey) []byte {
	priDerText := x509.MarshalPKCS1PrivateKey(privateKey)

	//3. 创建Block代表PEM编码的结构, 并填入der编码的数据
	//type Block struct {
	//    Type    string            // 得自前言的类型（如"RSA PRIVATE KEY"）
	//    Headers map[string]string // 可选的头项
	//    Bytes   []byte            // 内容解码后的数据，一般是DER编码的ASN.1结构
	//}

	block := pem.Block{
		Type:    "SZ RSA PRIVATE KEY", //随便填写
		Headers: nil,                  //可选信息，包括私钥加密方式等
		Bytes:   priDerText,           //私钥编码后的数据
	}

	//4. 将Pem Block数据写入到磁盘文件
	fileHandler1, err := os.Create(privateKeyFile)
	if err != nil {
		return err
	}

	//func Encode(out io.Writer, b *Block) error
	err = pem.Encode(fileHandler1, &block)

	if err != nil {
		return err
	}

	return nil
}

func main() {

	fmt.Printf("generate rsa private key ...\n")
	err := generateKeyPair(1024)
	if err != nil {
		fmt.Printf("generate rsa private failed, err : %v", err)
	}

	fmt.Printf("generate rsa private key successfully!\n")
}

```



##### 3. 公钥生成代码

```go
	fmt.Println("++++++++++++++ 生成公钥 +++++++++++")

	/*
	1. 获取公钥， 通过私钥获取
	2. 要对生成的私钥进行编码处理， x509， 按照规则，进行序列化处理, 生成der编码的数据
	3. 创建Block代表PEM编码的结构, 并填入der编码的数据
	4. 将Pem Block数据写入到磁盘文件
	*/

	//1. 获取公钥， 通过私钥获取
	pubKey := privateKey.PublicKey //注意是对象，而不是地址

	//2. 要对生成的私钥进行编码处理， x509， 按照规则，进行序列化处理, 生成der编码的数据
	pubKeyDerText := x509.MarshalPKCS1PublicKey(&pubKey)

	//3. 创建Block代表PEM编码的结构, 并填入der编码的数据
	block1 := pem.Block{
		Type:    "SZ RSA Public Key",
		Headers: nil,
		Bytes:   pubKeyDerText,
	}

	//4. 将Pem Block数据写入到磁盘文件

	fileHandler2, err := os.Create(publicKeyFile)
	if err != nil {
		return err
	}
```



#### RSA加解密

##### 1. 公钥加密

```go
1. 通过公钥文件，读取公钥信息 ==》 pem encode 的数据
2. pem decode， 得到block中的der编码数据
3. 解码der，得到公钥
4. 公钥加密
```

![公钥生成流程](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day2/03-资料/公钥生成流程.jpg)

```go
package main

import (
	"io/ioutil"
	"encoding/pem"
	"crypto/x509"
	"crypto/rsa"
	"crypto/rand"
	"fmt"
)


const privateKeyFile = "./RsaPrivateKey.pem"
const publicKeyFile = "./RsaPublicKey.pem"

func rsaPubEncrypt(filename string, plainText []byte) (error, []byte) {
	//1. 通过公钥文件，读取公钥信息 ==》 pem encode 的数据
	info, err := ioutil.ReadFile(filename)

	if err != nil {
		return err, nil
	}

	//2. pem decode， 得到block中的der编码数据
	block, _ := pem.Decode(info)
	//返回值1 ：pem.block
	//返回值2：rest参加是未解码完的数据，存储在这里

	//type Block struct {
	//    Type    string            // 得自前言的类型（如"RSA PRIVATE KEY"）
	//    Headers map[string]string // 可选的头项
	//    Bytes   []byte            // 内容解码后的数据，一般是DER编码的ASN.1结构
	//}

	//3. 解码der，得到公钥
	//derText := block.Bytes
	derText := block.Bytes
	publicKey, err := x509.ParsePKCS1PublicKey(derText)

	if err != nil {
		return err, nil
	}

	//4. 公钥加密
	//EncryptPKCS1v15使用PKCS#1 v1.5规定的填充方案和RSA算法加密msg。
	//func EncryptPKCS1v15(rand io.Reader, pub *PublicKey, msg []byte) (out []byte, err error)

	cipherData, err := rsa.EncryptPKCS1v15(rand.Reader, publicKey, plainText)

	if err != nil {
		return err, nil
	}

	return nil, cipherData
}

func main() {
	src := []byte("祝班主任节日快乐!")
	err, cipherData := rsaPubEncrypt(publicKeyFile, src)

	if err != nil {
		fmt.Println("公钥加密失败!")
	}

	fmt.Printf("cipherData : %x\n", cipherData)

}
```

##### 2. 私钥解密

思路分析同上



代码实现:

```go
func rsaPriKeyDecrypt(filename string, cipherData []byte) (error, []byte) {
	//1. 通过私钥文件，读取私钥信息 ==》 pem encode 的数据
	info, err := ioutil.ReadFile(filename)

	if err != nil {
		return err, nil
	}

	//2. pem decode， 得到block中的der编码数据
	block, _ := pem.Decode(info)
	//返回值1 ：pem.block
	//返回值2：rest参加是未解码完的数据，存储在这里

	//type Block struct {
	//    Type    string            // 得自前言的类型（如"RSA PRIVATE KEY"）
	//    Headers map[string]string // 可选的头项
	//    Bytes   []byte            // 内容解码后的数据，一般是DER编码的ASN.1结构
	//}

	//3. 解码der，得到私钥
	//derText := block.Bytes
	derText := block.Bytes
	privateKey, err := x509.ParsePKCS1PrivateKey(derText)

	if err != nil {
		return err, nil
	}

	//4. 私钥解密
	//DecryptPKCS1v15使用PKCS#1 v1.5规定的填充方案和RSA算法解密密文。如果random不是nil，函数会注意规避时间侧信道攻击。
	//func DecryptPKCS1v15(rand io.Reader, priv *PrivateKey, ciphertext []byte) (out []byte, err error)
	plainText, err := rsa.DecryptPKCS1v15(rand.Reader, privateKey, cipherData)

	if err != nil {
		return err, nil
	}

	return nil, plainText
}

```



main:

```go
func main() {
	src := []byte("祝班主任节日快乐!")
	err, cipherData := rsaPubEncrypt(publicKeyFile, src)

	if err != nil {
		fmt.Println("公钥加密失败!, err :", err)
	}

	fmt.Printf("cipherData : %x\n", cipherData)
	fmt.Println("++++++++++++++++++++++++++++++")

	err, plainText := rsaPriKeyDecrypt(privateKeyFile, cipherData)
	if err != nil {
		fmt.Println("私钥解密失败!, err : ", err)
	}
	fmt.Printf("plainText : %s\n", plainText)
}
```

##### 运行结果

![image-20220115210135192](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115210135192.png)



## 四、Base64

### 概述

Base64编码，是我们程序开发中经常使用到的编码方法。因为base64编码的字符串，更适合不同平台、不同语言的传输（一个字符可能其他的系统没有）。它是一种基于用64个可打印字符来表示二进制数据的表示方法。它通常用作存储、传输一些二进制数据编码方法，一句：将二进制数据文本化（转成ASCII）。

##### 作用

- 由于某些系统中只能使用ASCII字符。Base64就是用来将非ASCII字符的数据转换成ASCII字符的一种方法。
- 对二进制文件进行文本化后的传输
- 前后台交互时，经常使用base64，这样可以避免特殊字符传输错误

 

使用命令测试步骤：

```go
1. cp /bin/ls .

2. base64 ls > 1.txt
3. 查看1.txt  =》 可读的文本数据
4. base64 -D 1.txt > myls  ==> 将文本数据解码为原来的ls数据， -D decode
5. ls -al 1.txt ./ls ./myls
=====
 duke ~$  ls -al 1.txt ./ls ./myls
-rwxr-xr-x  1 duke  staff  38704  3  6 14:49 ./ls
-rwxr-xr-x  1 duke  staff  38704  3  6 14:49 ./myls
-rw-r--r--  1 duke  staff  51609  3  6 14:49 1.txt
=====

6. chmod +x myls  => 添加执行权限
7. ./myls  ==》 与ls功能相同
```

### 2. 字符集

```sh
普通的base64字符集 #常用
A-Z : 26
a-z : 26
0-9: 10
+, / : 2

64个
========
URL专用的base64字符集
A-Z : 26
a-z : 26
0-9: 10
-，_ : 2

64个
```

### 3. 编码规则

```sh
MAn => 3 * 8 = 24 / 6 = 4

M =>  77 = 64 + 8 + 4 + 1 => 0100, 1101

Base64编码的数据比原来的字节数大。
由3字节 => 4字节
man =>twfu
==========

当需要编码的数据不足时，使用等号（=）填充，解码时，会自动剔除
```

- base64就是一种基于64个可打印字符来表示二进制数据的方法。
- 编码后便于传输，尤其是不可见字符或特殊字符，对端接收后解码即可复原。
- base64只是编码，并不具有加密作用。

为了保证所输出的编码位可读字符，Base64制定了一个编码表，以便进行统一转换。编码表的大小为2^6=64，这也是Base64名称的由来。

##### Base64编码表

![image-20220115214549046](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115214549046.png)

###### 恰好三个字节情况

![image-20220115214335399](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115214335399.png)

###### 不足三个字节情况

![image-20220115214348388](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115214348388.png)

#### 4. go代码测试base64

```go
package main

import (
	"fmt"
	"encoding/base64"
)

func main() {

	fmt.Printf("标准base64编码...\n")

	//info := []byte("国足宇宙第一!!!")
	info := []byte("https://studygolang.com/pkgdoc&hell?/?=")

	encodeInfo := base64.StdEncoding.EncodeToString(info)

	fmt.Printf("encode info 1   : %s\n", encodeInfo)

	fmt.Printf("URL base64编码...\n")

	urlEncodeInfo := base64.URLEncoding.EncodeToString(info)
	fmt.Printf("url encode info : %s\n", urlEncodeInfo)

}

```

运行结果

```sh
 duke ~/go/src/01_授课代码/05-shenzhen-term3/01-密码学$  go run 06-base64-test.go 
标准base64编码...
encode info 1   : aHR0cHM6Ly9zdHVkeWdvbGFuZy5jb20vcGtnZG9jJmhlbGw/Lz89
URL base64编码...
url encode info : aHR0cHM6Ly9zdHVkeWdvbGFuZy5jb20vcGtnZG9jJmhlbGw_Lz89
 duke ~/go/src/01_授课代码/05-shenzhen-term3/01-密码学$  
```

## 五、Hash(哈希)

### 基础知识

#### 1. 命令：

```sh
mac   : shasum -a 256 <文件名>
linux : sha256Sum <文件名>

sha256哈希运算，是一个hash算法

//sha256运算后，得到256位的哈希数值， 使用16进制打印如下：
46a546cfdc716cd3d7e49795a3b77428470778126b2b0e004932adb7844a5a54

64 * 4 = 256bit
```

#### 2. 特性：

```sh
Hash: 可以对输入的数据内容生成一个唯一的数值 唯一标识
对于#同一个算法，有如下特性：
1. 输入内容不变，输出内容不变
2. 输入内容改变，哪怕是一点点改变，输出的内容千差万别
3. 无论输入的内容大小如何，（1M, 1K, 1G）， 生成的哈希长度相同
4. 哈希运算是对输入内容做摘要（指纹），无法根据哈希值反推会原文。
```

输入：原像

输出：摘要，指纹，哈希值

算法：哈希函数，摘要函数，消息摘要函数，杂凑函数



##### 关于术语

> 单向散列函数的相关术语有很多变体，不同参考资料中所使用的术语也不同，下面我们就介绍其中的儿个。
>
> 单向散列函数也称为**消息摘要函数**（message digest function）、**哈希函数**或者**杂凑函数**。
>
> 输入单向散列函数的消息也称为**原像**（pre-image）。
>
> 单向散列函数输出的散列值也称为**消息摘要**（message digest）或者**指纹**（fingerprint）。
>
> **完整性**也称为一致性。
>
> 顺便说一句，单向散列函数中的“散列”的英文"hash一词，原意是古法语中的“斧子”，后来被引申为“剁碎的肉末"，也许是用斧子一通乱剁再搅在一起的那种感觉吧。单向散列函数的作用，实际上就是将很长的消息剁碎，然后再混合成固定长度的散列值。

- 根据任意长度的消息计算出固定长度的散列值
- 能够快速计算出散列值
- 消息不同散列值也不同

==重要特性：==

- 原像不可逆： 

  ```sh
  具备单向性， 1k => 10G内容, 不可能
  ```

- 抗碰撞性：

  ```sh
  2^256 可能  =》全宇宙可观测原子总数
  
  给一个哈希值：46a546cfdc716cd3d7e49795a3b77428470778126b2b0e004932adb7844a5a54
  你去拼装一段内容，使得运行同样的算法，同样的哈希值。不可能完成的
  ```


### 哈希应用

#### 1. 检测软件是否被篡改

> 我们可以使用单向散列函数来确认自己下载的软件是否被篡改。
>
> 很多软件，尤其是安全相关的软件都会把通过单向散列函数计算出的散列值公布在自己的官方网站上。用户在下载到软件之后，可以自行计算散列值，然后与官方网站上公布的散列值进行对比。通过散列值，用户可以确认自己所下载到的文件与软件作者所提供的文件是否一致。
>
> 这样的方法，在可以通过多种途径得到软件的情况下非常有用。为了减轻服务器的压力，很多软件作者都会借助多个网站（镜像站点）来发布软件，在这种情况下，单向散列函数就会在检测软件是否被篡改方面发挥重要作用。

![image-20220115221302226](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115221302226.png)

#### 2. 消息认证码

> 使用单向散列函数可以构造消息认证码。
>
> 消息认证码是将“发送者和接收者之间的共享密钥”和“消息，进行混合后计算出的散列值。使用消息认证码可以检测并防止通信过程中的错误、篡改以及伪装。
>

#### 3. 伪随机数生成器

> 使用单向散列函数可以构造伪随机数生成器。
>
> 密码技术中所使用的随机数需要具备“事实上不可能根据过去的随机数列预测未来的随机数列”这样的性质。为了保证不可预测性，可以利用单向散列函数的单向性。

#### 4. 一次性口令

> 使用单向散列函数可以构造一次性口令（one-time password）。
>
> 一次性口令经常被用于服务器对客户端的合法性认证。在这种方式中，通过使用单向散列函数可以保证口令只在通信链路上传送一次（one-time），因此即使窃听者窃取了口令，也无法使用。

#### 5. 密码存储

> 网站数据库中，对密码的存储并不是密码的明文，而是密码的哈希值，
>
> 每次登录时，会对密码进行哈希处理，然后与数据库对比。
>
> 即使数据库被盗，黑客也无法拿到用户的密码，保证用户账户安全

#### 6. 数字签名

==私钥对文件签名时，并不会对文件本身做签名，而是对这个文件的哈希值进行签名==

> 在进行数字签名时也会使用单向散列函数。
>
> 数字签名是现实社会中的签名（sign）和盖章这样的行为在数字世界中的实现。数字签名的处理过程非常耗时，因此一般不会对整个消息内容直接施加数字签名，而是先通过单向散列函数计算出消息的散列值，然后再对这个散列值施加数字签名。

### 常用的Hash算法

#### 1. md4, md5

> MD4是由Rivest于1990年设计的单向散列函数，能够产生128比特的散列值（RFC1186，修订版RFC1320）。不过，随着Dobbertin提出寻找MD4散列碰撞的方法，因此现在它已经不安全了。
>
> MD5是由Rwest于1991年设计的单项散列函数，能够产生128比特的散列值（RFC1321）。
>
> MD5的强抗碰撞性已经被攻破，也就是说，现在已经能够产生具备相同散列值的两条不同的消息，因此它也已经不安全了。
>
> MD4和MD5中的MD是消息摘要（Message Digest）的缩写。

md5: 生成hash长度的长度：128位(32字节)。sha256: 256位(64字节)

##### - 方式一

```go
//16bytes, 128bit
func md5Test1(info []byte) []byte {
	//对多量数据进行哈希运算

	//1. 创建一个哈希器
	hasher := md5.New()

	io.WriteString(hasher, "hello ")
	io.WriteString(hasher, "world ")

	//2. 执行Sum操作，得到哈希值
	//hash := hasher.Sum(nil)
	//sum(b), 如果b不是nil， 那么返回的值为b+hash值， b的ascii值后追加hello world的哈希值
	hash := hasher.Sum([]byte("0x"))

	return hash
}
```

![image-20220115223336388](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115223336388.png)

##### - 方式二

```go
package main

import (
	"crypto/md5"
	"io"
	"fmt"
)

//哈希运算，使用go包，有两种调用方式

//方式一

//16bytes, 128bit
func md5Test1(info []byte) []byte {
	//对多量数据进行哈希运算

	//1. 创建一个哈希器
	hasher := md5.New()

	io.WriteString(hasher, "hello ")
	io.WriteString(hasher, "world!")

	//2. 执行Sum操作，得到哈希值
	//hash := hasher.Sum(nil)
	//sum(b), 如果b不是nil， 那么返回的值为b+hash值
	hash := hasher.Sum([]byte("0x"))

	return hash
}

//方式二
func md5Test2(info []byte) []byte {
	hash := md5.Sum(info)

	//将数组转换为切片
	return hash[:]
}

func main() {

	hash := md5Test1(nil)

	fmt.Printf("hash : %x\n", hash)

	fmt.Printf("+++++++++++++++\n")

	src := []byte("hello world!")
	hash2 := md5Test2(src)

	fmt.Printf("hash2 : %x\n", hash2)
}
```

![image-20220115223839072](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115223839072.png)

#### 2. sha1, sha2

- SHA-1
- SHA-224
- ==SHA-256==
- SHA-384
- SHA-512

> SHA-1是由NIST（NationalInstituteOfStandardsandTechnology，美国国家标准技术研究所）设计的一种能够产生160比特的散列值的单向散列函数。1993年被作为美国联邦信息处理标准规格（FIPS PUB 180）发布的是SHA,1995年发布的修订版FIPS PUB 180-1称为SHA-1。
>
> SHA-1的消息长度存在上限，但这个值接近于2^64^比特，是个非常巨大的数值，因此在实际应用中没有问题。
>
> SHA-256、SHA-384和SHA-512都是由NIST设计的单向散列函数，它们的散列值长度分别为256比特、384比特和512比特。这些单向散列函数合起来统称SHA-2，它们的消息长度也存在上限（SHA-256的上限接近于 2^64^ 比特，SHA-384 和 SHA-512的上限接近于 2^128^ 比特）。这些单向散列函数是于2002年和 SHA-1 一起作为 FIPS PUB 180-2发布的 SHA-1 的强抗碰撞性已于2005年被攻破, 也就是说，现在已经能够产生具备相同散列值的两条不同的消息。不过，SHA-2还尚未被攻破。

##### sha1



##### sha2: （一系列哈希算法，更可靠，更安全）

```go
- SHA-224
- SHA-256   ===> 比特币， 以太坊，都使用
- SHA-384
- SHA-512
```



|             |   比特数   |   字节数   |
| ----------- | :--------: | :--------: |
| MD4         |   128bit   |   16byte   |
| ==MD5==     | ==128bit== | ==16byte== |
| SHA-1       |   160bit   |   20byte   |
| SHA-224     |   224bit   |   28byte   |
| ==SHA-256== | ==256bit== | ==32byte== |
| SHA-384     |   384bit   |   48byte   |
| SHA-512     |   512bit   |   64byte   |



###### sha256

```go
package main

import (
	"os"
	"crypto/sha256"
	"io"
	"fmt"
)

//使用打开文件方式获取哈希

const filename = "填入自己的文件"

func main() {

	//1. open 文件
	file, err := os.Open(filename)

	defer file.Close()

	if err != nil {
		panic(err)
	}

	//2. 创建hash
	hasher := sha256.New()

	/*
	type Hash interface {
		io.Writer
		Sum(b []byte) []byte

		Reset()

		Size() int
		BlockSize() int
	}
	*/

	//3. copy句柄
	//func Copy(dst Writer, src Reader) (written int64, err error) {
	length, err := io.Copy(hasher, file)

	if err != nil {
		panic(err)
	}

	fmt.Printf("length : %d\n", length)

	//4. hash sum操作
	hash := hasher.Sum(nil)

	fmt.Printf("hash : %x\n", hash)

}
```

运行结果

![image-20220115224920846](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220115224920846.png)



## 六、消息认证

### 使用消息认证码的原因

#### 对称加密的问题：

```sh
背景：

Alice   向  Bob说， 你好！


对称加密：password


A：你好  =》 加密 =》 *&*……*（……TUGG   =====》 传输

B：密文（ *&*……*（……TUGG   =====》） =》 解密  =》 你好！！



++++++++++++++++++++++

假如A发的消息就是不可读（&*……%￥￥……&*&%￥#@） 加密，解密

B收到密文后解密：（&*……%￥￥……&*&%￥#@）

问题：B解出来的是乱码，B无法判断这个消息就是A发的原文！！


解决办法：引入消息认证码
```

#### 演示

```go
func main() {

	src := []byte("不是一番寒彻骨，哪得梅花扑鼻香!!! 123456734523452345	")
	key := []byte("1234567887654321")

	cipherData := aesCTREncrypt(src, key)

	fmt.Printf("cipherData : %x\n", cipherData)

	fmt.Printf("+++++++++++++++++++++++++\n")

	cipherData = append(cipherData, []byte("hello")...)

	plainText := aesCTRDecrypt(cipherData, key)
	fmt.Printf("plainText ： %x\n", plainText)
}
```



==密文被改变，依然可以解出数据，但是不知道数据是否可靠==

```go
 duke ~/go/src/01_授课代码/05-shenzhen-term3/01-密码学$  go run 02-aes-ctr.go 
明文： 不是一番寒彻骨，哪得梅花扑鼻香!!! 123456734523452345     
aes block size :  16
cipherData : d8d1cfb975e7c16451a9d7fda95ea6782cc5133d42c19e3f63cb4d82e494889eac4b756e0b427b263a07a527811c16b80f3d49a4240b66fb6ae21afffa114c5e8a17f9fa
+++++++++++++++++++++++++
plainText ： e4b88de698afe4b880e795aae5af92e5bdbbe9aaa8efbc8ce593aae5be97e6a285e88ab1e68991e9bcbbe9a699212121203132333435367333435323334353233343509e9364ebf0b
```

### 消息认证码（MAC）

**消息认证码（message authentication code）是一种确认完整性并进行认证的技术，取三个单词的首字母，简称为MAC。**



- 保证数据未被篡改
- 对发送者进行身份认证



![image-20220117115136570](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220117115136570.png)



1. 发送者Alice与接收者Bob事先共享密钥。
2. 发送者Alice根据汇款请求消息计算MAC值（使用共享密钥）。
3. 发送者Alice将汇款请求消息和MAC值两者发送给接收者Bob。
4. 接收者Bob根据接收到的汇款请求消息计算MAC值（使用共享密钥）。
5. 接收者Bob将自己计算的MAC值与从Alice处收到的MAC值进行对比。
6. 如果两个MAC值一致，则接收者Bob就可以断定汇款请求的确来自Alice（认证成功）；如果不一致，则可以断定消息不是来自Alice（认证失败）。





### MAC使用场景

#### 1. SWIFT

环球银行金融电信协会，是一个组织，为银行间的交易报价护航。银行间交互使用了SWIFT进行交互，这里面对消息的完成性和身份验证，就是使用了消息认证码。

- 最初使用消息认证码，由人工配送秘钥
- 后来使用公钥



#### 2. https

- ssl/tls协议，里面的握手协议使用了消息认证码
- https = http + ssl/tls(security socket layer)



#### 3. IPSec

- IP协议的增强版，使用了消息认证码



### HMAC

#### 1. ==H代表哈希==

> HMAC是一种使用单向散列函数来构造消息认证码的方法（RFC2104），其中HMAC的H就是Hash的意思。
>
> HMAC中所使用的单向散列函数并不仅限于一种，任何高强度的单向散列函数都可以被用于HMAC,如果将来设计出新的单向散列函数，也同样可以使用。
>
> 使用SHA-I、MD5、RIPEMD-160所构造的HMAC，分别称为HMAC-SHA-1、HMAC-MD5和HMAC-RlPEMD。

![image-20220117120010635](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220117120010635.png)

#### 2. 使用分析：

```go
//接收端和验证端都要执行
//New函数返回一个采用hash.Hash作为底层hash接口、key作为密钥的HMAC算法的hash接口。
func New(h func() hash.Hash, key []byte) hash.Hash

- 参数1：自己指定哈希算法， 是一个函数
	- md5.New
	- sha1.New
	- sha256.New

- 参数2：秘钥
- 返回值：哈希函数对象


//仅在验证端执行
//比较两个MAC是否相同，而不会泄露对比时间信息。（以规避时间侧信道攻击：指通过计算比较时花费的时间的长短来获取密码的信息，用于密码破解）
func Equal(mac1, mac2 []byte) bool
- 参数1：自己计算的哈希值
- 参数2：接收到的哈希值
- 返回值：对比结果
```



#### 3. 代码

```go
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"fmt"
)

/*
//接收端和验证端都要执行
//New函数返回一个采用hash.Hash作为底层hash接口、key作为密钥的HMAC算法的hash接口。
func New(h func() hash.Hash, key []byte) hash.Hash

- 参数1：自己指定哈希算法， 是一个函数
- md5.New
- sha1.New
- sha256.New

- 参数2：秘钥
- 返回值：哈希函数对象


//仅在验证端执行
//比较两个MAC是否相同，而不会泄露对比时间信息。（以规避时间侧信道攻击：指通过计算比较时花费的时间的长短来获取密码的信息，用于密码破解）
func Equal(mac1, mac2 []byte) bool
- 参数1：自己计算的哈希值
- 参数2：接收到的哈希值
- 返回值：对比结果
*/

//生成hmac（消息认证码）
func generateHMAC(src []byte, key []byte) []byte {
	//1. 创建哈希器
	hasher := hmac.New(sha256.New, key)

	//2. 生成mac值
	//mac := hasher.Sum(src)
	hasher.Write(src)

	mac := hasher.Sum(nil)

	return mac
}

//认证mac
func verifyHMAC(src, key, mac1 []byte) bool {
	//1. 对端接收到的源数据

	//2. 对端接收到的mac1

	//3. 对端计算本地的mac2
	mac2 := generateHMAC(src, key)

	//4. 对比mac1与mac2
	return hmac.Equal(mac1, mac2)
}

func main() {
	src := []byte("hello world")
	key := []byte("1234567890")

	mac1 := generateHMAC(src, key)

	fmt.Printf("mac1 : %x\n", mac1)

	isEqual := verifyHMAC(src, key, mac1)

	fmt.Printf("isEqual : %v\n", isEqual)

	srcChanged := []byte("hello world!!!!!")

	isEqual = verifyHMAC(srcChanged, key, mac1)

	fmt.Printf("after changed, isEqual : %v\n", isEqual)
}
```

<img src="/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220117144546145.png" alt="image-20220117144546145" style="zoom:200%;" />

### 消息认证存在的问题

1. 无法有效的配送秘钥
2. 无法进行第三方证明
3. 无法防止发送方否认



4. ==解决办法：非对称加密数字签名！！==

## 七、数字签名

### 基础知识

```go
"数字签名 --- 消息到底是谁写的"
```

|              |         私钥         |            公钥            |
| ------------ | :------------------: | :------------------------: |
| 非对称加密   |   接收者解密时使用   |      发送者加密时使用      |
| 数字签名     | 签名者生成签名时使用 |    验证者验证签名时使用    |
| 谁持有秘钥？ |       个人持有       | 只要需要，任何人都可以持有 |

公钥，私钥

公钥：加密

私钥：数字签名

### 签名认证流程

![数字签名流程介绍](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day3/03-资料/数字签名流程介绍.jpg)

注意：

- ==签名的数据不是数据本身，而是数据的哈希值==

消息认证问题的解决

1. 无法有效的配送秘钥  ===》 数字签名中，不需要协商秘钥，没有配送需求
2. 无法进行第三方证明  ===》 任何人都持有公钥，都可以帮助认证
3. 无法防止发送方否认  ===》 私钥只有发送方持有，无法进行抵赖

![image-20220117150333746](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220117150333746.png)

### RSA数字签名与认证

```go
package main

import (
	"io/ioutil"
	"encoding/pem"
	"crypto/x509"
	"crypto/sha256"
	"crypto/rsa"
	"crypto/rand"
	"crypto"
	"fmt"
)

/*
私钥签名:
1. 提供私钥文件， 解析出私钥内容（decode, parse....）

2. 使用私钥进行数字签名


公钥认证
1. 提供公钥文件， 解析出公钥内容（decode, parse....）

2. 使用公钥进行数字签名认证



*/

//私钥签名: 提供私钥，签名数据，得到数字签名
func rsaSignData(filename string, src []byte) ([]byte, error) {

	//一、 提供私钥文件， 解析出私钥内容（decode, parse....）
	//1. 通过私钥文件，读取私钥信息 ==》 pem encode 的数据
	info, err := ioutil.ReadFile(filename)

	if err != nil {
		return nil, err
	}

	//2. pem decode， 得到block中的der编码数据
	block, _ := pem.Decode(info)
	//返回值1 ：pem.block
	//返回值2：rest参加是未解码完的数据，存储在这里

	//type Block struct {
	//    Type    string            // 得自前言的类型（如"RSA PRIVATE KEY"）
	//    Headers map[string]string // 可选的头项
	//    Bytes   []byte            // 内容解码后的数据，一般是DER编码的ASN.1结构
	//}

	//3. 解码der，得到私钥
	//derText := block.Bytes
	derText := block.Bytes
	privateKey, err := x509.ParsePKCS1PrivateKey(derText)

	if err != nil {
		return nil, err
	}

	//二. 使用私钥进行数字签名

	//1. 获取原文的哈希值
	hash := sha256.Sum256(src) //返回值是[32]byte， 一个数组

	//SignPKCS1v15使用RSA PKCS#1 v1.5规定的RSASSA-PKCS1-V1_5-SIGN签名方案计算签名
	//func SignPKCS1v15(rand io.Reader, priv *PrivateKey, hash crypto.Hash, hashed []byte) (s []byte, err error)

	//2. 执行签名操作
	signature, err := rsa.SignPKCS1v15(rand.Reader, privateKey, crypto.SHA256, hash[:])
	if err != nil {
		return nil, err
	}

	return signature, nil
}

//公钥认证
func rsaVerifySignature(sig []byte, src []byte, filename string) error {
	//一. 提供公钥文件， 解析出公钥内容（decode, parse....）
	//1. 通过公钥文件，读取公钥信息 ==》 pem encode 的数据
	info, err := ioutil.ReadFile(filename)

	if err != nil {
		return err
	}

	//2. pem decode， 得到block中的der编码数据
	block, _ := pem.Decode(info)

	//3. 解码der，得到公钥
	//derText := block.Bytes
	derText := block.Bytes
	publicKey, err := x509.ParsePKCS1PublicKey(derText)

	if err != nil {
		return err
	}

	//二. 使用公钥进行数字签名认证

	//1. 获取原文的哈希值
	hash := sha256.Sum256(src) //返回值是[32]byte， 一个数组

	//VerifyPKCS1v15认证RSA PKCS#1 v1.5签名。hashed是使用提供的hash参数对（要签名的）原始数据进行hash的结果。合法的签名会返回nil，否则表示签名不合法。
	//func VerifyPKCS1v15(pub *PublicKey, hash crypto.Hash, hashed []byte, sig []byte) error {
	err = rsa.VerifyPKCS1v15(publicKey, crypto.SHA256, hash[:], sig)

	return err
}

func main() {

	src := []byte("hello world!!!!")
	signature, err := rsaSignData(PrivateKeyFile, src)
	if err != nil {
		fmt.Printf("签名失败!, err: %s\n", err)
	}

	fmt.Printf("signature ： %x\n", signature)
	//fmt.Printf("signature ： %s\n", signature)

	fmt.Printf("++++++++++\n")

	src1 := []byte("hello world!!!!=======")

	err = rsaVerifySignature(signature, src1, PublicKeyFile)
	if err != nil {
		fmt.Printf("签名校验失败!, err: %s\n", err)
		return
	}

	fmt.Printf("签名校验成功!\n")
}

```

![image-20220117153516961](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220117153516961.png)

### 椭圆曲线介绍

> 1. 概念
>
> 椭圆曲线密码学（英语：Elliptic curve cryptography，缩写为 ECC），一种建立公开密钥加密的算法，基于椭圆曲线数学。椭圆曲线在密码学中的使用是在1985年由Neal Koblitz和Victor Miller分别独立提出的。
>
> ECC的主要优势是在某些情况下它比其他的方法使用更小的密钥——比如RSA加密算法——提供相当的或更高等级的安全。
>
> 椭圆曲线密码学的许多形式有稍微的不同，所有的都依赖于被广泛承认的解决椭圆曲线离散对数问题的困难性上。与传统的基于大质数因子分解困难性的加密方法不同，ECC通过椭圆曲线方程式的性质产生密钥。
>
> ECC 164位的密钥产生的一个安全级相当于RSA 1024位密钥提供的保密强度，而且计算量较小，处理速度更快，存储空间和传输带宽占用较少。==目前我国居民二代身份证正在使用 256 位的椭圆曲线密码，虚拟货币`比特币`也选择ECC作为加密算法。==

![image-20220117154415744](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220117154415744.png)

![椭圆曲线介绍](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day3/03-资料/椭圆曲线介绍.png)





### 生成私钥公钥

```go
// PublicKey represents an ECDSA public key.
type PublicKey struct {
	elliptic.Curve
	X, Y *big.Int
}

// PrivateKey represents an ECDSA private key.
type PrivateKey struct {
	PublicKey
	D *big.Int
}
```

注意点：

```go
//1. - 选择一个椭圆曲线（在elliptic包）//rsa需要传入秘钥位数
	//type Curve
	//func P224() Curve
	//func P256() Curve
	//func P384() Curve
	//func P521() Curve
//2.私钥编码的函数
	derText, err := x509.MarshalECPrivateKey(privateKey)

//3. 公钥编码函数， 一定要传递地址，否则出错
	derText2, err := x509.MarshalPKIXPublicKey(&publicKey)
```

```go
package main

import (
	"crypto/elliptic"
	"crypto/ecdsa"
	"crypto/rand"
	"crypto/x509"
	"encoding/pem"
	"os"
	"fmt"
)

//生成私钥公钥

func generateEccKeypair() {
	//- 选择一个椭圆曲线（在elliptic包）
	//type Curve
	//func P224() Curve
	//func P256() Curve
	//func P384() Curve
	//func P521() Curve
	curve := elliptic.P521()

	//- 使用ecdsa包，创建私钥 //ecdsa椭圆曲线数字签名
	//GenerateKey函数生成秘钥对
	//func GenerateKey(c elliptic.Curve, rand io.Reader) (priv *PrivateKey, err error)
	privateKey, err := ecdsa.GenerateKey(curve, rand.Reader)

	checkErr("generate key failed!", err)

	//- 使用x509进行编码
	//MarshalECPrivateKey将ecdsa私钥序列化为ASN.1 DER编码。
	//func MarshalECPrivateKey(key *ecdsa.PrivateKey) ([]byte, error)
	derText, err := x509.MarshalECPrivateKey(privateKey)
	checkErr("MarshalECPrivateKey", err)

	//- 写入pem.Block中
	block1 := pem.Block{
		Type:    "ECC PRIVATE KEY",
		Headers: nil,
		Bytes:   derText,
	}

	//- pem.Encode
	fileHander, err := os.Create(EccPrivateKeyFile)
	checkErr("os.Create Failed", err)

	defer fileHander.Close()

	err = pem.Encode(fileHander, &block1)
	checkErr("pem Encode failed", err)

	fmt.Printf("++++++++++++++++++++++\n")
	//获取公钥
	publicKey := privateKey.PublicKey

	//- 使用x509进行编码
	//通用的序列化方式
	//derText2, err := x509.MarshalPKIXPublicKey(publicKey)
	derText2, err := x509.MarshalPKIXPublicKey(&publicKey)
	//传递地址

	checkErr("MarshalPKIXPublicKey", err)

	//- 写入pem.Block中
	block2 := pem.Block{
		Type:    "ECC PUBLICK KEY",
		Headers: nil,
		Bytes:   derText2,
	}

	//- pem.Encode
	fileHander2, err := os.Create(EccPublicKeyFile)
	checkErr("public key os.Create Failed", err)

	defer fileHander2.Close()

	err = pem.Encode(fileHander2, &block2)
	checkErr("public key pem Encode failed", err)

}

func main() {
	generateEccKeypair()
}

```

==Golang，不支持加解密，支持ECC签名==

### 私钥签名

```go
package main

import (
	"io/ioutil"
	"encoding/pem"
	"crypto/x509"
	"crypto/sha256"
	"crypto/ecdsa"
	"crypto/rand"
	"math/big"
	"fmt"
)

//自己定义的签名结构
type Signature struct {
	r *big.Int
	s *big.Int
}

//使用私钥签名

func eccSignData(filename string, src []byte) (Signature, error) {
	//1. 读取私钥，解码
	info, err := ioutil.ReadFile(filename)

	if err != nil {
		return Signature{}, err
	}

	//2. pem decode， 得到block中的der编码数据
	block, _ := pem.Decode(info)
	//返回值1 ：pem.block
	//返回值2：rest参加是未解码完的数据，存储在这里

	//3. 解码der，得到私钥
	//derText := block.Bytes
	derText := block.Bytes
	privateKey, err := x509.ParseECPrivateKey(derText)

	if err != nil {
		return Signature{}, err
	}

	//2. 对原文生成哈希
	hash /*[32]byte*/ := sha256.Sum256(src)

	//3. 使用私钥签名
	//使用私钥对任意长度的hash值（必须是较大信息的hash结果）进行签名，返回签名结果（一对大整数）。私钥的安全性取决于密码读取器
	//func Sign(rand io.Reader, priv *PrivateKey, hash []byte) (r, s *big.Int, err error)
	r, s, err := ecdsa.Sign(rand.Reader, privateKey, hash[:])

	if err != nil {
		return Signature{}, err
	}

	sig := Signature{r, s}

	return sig, nil
}

//使用公钥校验
func eccVerifySig(filename string, src []byte, sig []byte) bool {

	//1. 读取公钥，解码
	//2. 对原文生成哈希
	//3. 使用公钥验证

	//TODO

	return true
}

func main() {

	src := []byte("Golang，不支持加解密，支持ECC签名")

	sig, err := eccSignData(EccPrivateKeyFile, src)

	if err != nil {
		fmt.Printf("err : %s\n", err)
	}

	fmt.Printf("signature : %s\n", sig)
	fmt.Printf("signature hex : %x\n", sig)
}

```



### 公钥验证

```go
//使用公钥校验
func eccVerifySig(filename string, src []byte, sig Signature) error {

	//1. 读取公钥，解码
	info, err := ioutil.ReadFile(filename)

	if err != nil {
		return err
	}

	//2. pem decode， 得到block中的der编码数据
	block, _ := pem.Decode(info)

	//3. 解码der，得到公钥
	//derText := block.Bytes
	derText := block.Bytes
	//publicKey, err := x509.ParsePKCS1PublicKey(derText)

	publicKeyInterface, err := x509.ParsePKIXPublicKey(derText)

	if err != nil {
		return err
	}

	publicKey, ok := publicKeyInterface.(*ecdsa.PublicKey)
	if !ok {
		return errors.New("断言失败，非ecds公钥!\n")
	}

	//2. 对原文生成哈希

	hash := sha256.Sum256(src)
	//3. 使用公钥验证

	//使用公钥验证hash值和两个大整数r、s构成的签名，并返回签名是否合法。
	//func Verify(pub *PublicKey, hash []byte, r, s *big.Int) bool

	isValid := ecdsa.Verify(publicKey, hash[:], sig.r, sig.s)

	if !isValid {
		return errors.New("校验失败!")
	}

	return nil
}
```



main.go

```go
func main() {

	src := []byte("Golang，不支持加解密，支持ECC签名")

	sig, err := eccSignData(EccPrivateKeyFile, src)

	if err != nil {
		fmt.Printf("err : %s\n", err)
	}

	fmt.Printf("signature : %s\n", sig)
	fmt.Printf("signature hex : %x\n", sig)

	fmt.Printf("++++++++++++++++++=\n")

	src1 := []byte("Golang，不支持加解密，支持ECC签名!!!!!!!!!!!")
	err = eccVerifySig(EccPublicKeyFile, src1, sig)
	if err != nil {
		fmt.Println(err)
		return
	}

	fmt.Printf("签名校验成功!\n")
}
```

#### bigint介绍

- bytes()
- setBytes()
- setString()

- Lsh()左移
- Rsh（）右移
- Cmp（）对比



```go
package main

import (
	"math/big"
	"fmt"
)

func main() {

	var b1 big.Int
	var b2 big.Int
	var b3 big.Int
	var b4 big.Int

	b1.SetString("10000", 10)
	b2.SetString("20000", 10)

	fmt.Printf("b2 bytes : %v\n", b2.Bytes())

	b4.Add(&b1, &b2)
	fmt.Printf("b4 : %d\n", b4.Int64())

	s1 := []byte("4000")
	b3.SetBytes(s1)

	b3.Add(&b1, &b2)

	fmt.Printf("b3 : %v\n", b3)
	fmt.Printf("b3 : %v\n", b3.Int64())

}

```

#### 将r,s拼接成bytes返回

- r,s 都是big.int类型，它们的长度相同
- 我们可以通过bigint的bytes（）方法，将r，s的字节流拼接到一起，整体返回
- 在验证端，将bytes从中间一分为二，得到两段bytes
- 通过bigint setBytes方法将r,s 还原

### 数字签名存在的问题

##### 解决方法：引入CA机构

![公钥分发困难问题](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day3/03-资料/公钥分发困难问题.jpg)

##### 浏览器证书

![image-20220120151205898](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220120151205898.png)

## 八、证书使用

### 证书使用

- 所有的网站都转成https，
- https = http + ssl

- ssl : Secure Socket Layer， 是一个通讯协议，在通讯过程中，使用了数字证书

### https通信详情

- 所有的通信不再传输公钥，而是传输数字证书。
- 证书里面包含了公钥，可以由CA机构认证。

```shell
1. 网站提供者会自己生成公钥私钥。
   - 也可以不自己生成，全部由CA帮助生成

2. 服务器提供者将公钥发送给选择的CA机构

3. CA机构也有自己的私钥公钥。CA使用自己的私钥对服务器的公钥进行签名
    - 还有一些其他辅助信息（发行机构，主题，指纹）
    - 公钥
    - 签名

    CA向服务器颁发一个数字证书


4. 当用户访问服务器的时候，服务器会将CA证书发送给客户


5. 在客户的浏览器中，已经随着操作系统预装了知名CA机构的根证书，这里面包含了CA机构的公钥，浏览器就会对服务器的证书进行验证

6. 如果验证成功，说明服务器可靠，可以正常通信（小锁头）

7. 如果验证失败，显示（Not Secure）， 提示Warning

8. 证书有效时， 浏览器会将自己支持的对称加密算法（des, 3des, aes）发送给服务器， 生成随机秘钥（对称），使用服务器的公钥，对上述信息加密。发送给服务器

9. 服务器选择一个加密算法，使用对称秘钥加密消息，发送给客户端

10. 双方达成一致，接下来通信转换为对称加密
```

### windows下查看数字证书

```sh
#打开证书管理器
Win + R
certmgr.msc
```



导出der格式数字证书

使用openssl命令查看证书信息

```sh
openssl x509 -in <证书名字.crt> -inform der -text //-pubkey  //-noout
```

证书详情：

```sh
duke ~/Desktop$  openssl x509 -in GlobalSignTest.cer -inform der -text -pubkey
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            04:00:00:00:00:01:21:58:53:08:a2
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: OU=GlobalSign Root CA - R3, O=GlobalSign, CN=GlobalSign
        Validity
            Not Before: Mar 18 10:00:00 2009 GMT
            Not After : Mar 18 10:00:00 2029 GMT
        Subject: OU=GlobalSign Root CA - R3, O=GlobalSign, CN=GlobalSign
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:cc:25:76:90:79:06:78:22:16:f5:c0:83:b6:84:
                    ca:28:9e:fd:05:76:11:c5:ad:88:72:fc:46:02:43:
                    c7:b2:8a:9d:04:5f:24:cb:2e:4b:e1:60:82:46:e1:
                    52:ab:0c:81:47:70:6c:dd:64:d1:eb:f5:2c:a3:0f:
                    82:3d:0c:2b:ae:97:d7:b6:14:86:10:79:bb:3b:13:
                    80:77:8c:08:e1:49:d2:6a:62:2f:1f:5e:fa:96:68:
                    df:89:27:95:38:9f:06:d7:3e:c9:cb:26:59:0d:73:
                    de:b0:c8:e9:26:0e:83:15:c6:ef:5b:8b:d2:04:60:
                    ca:49:a6:28:f6:69:3b:f6:cb:c8:28:91:e5:9d:8a:
                    61:57:37:ac:74:14:dc:74:e0:3a:ee:72:2f:2e:9c:
                    fb:d0:bb:bf:f5:3d:00:e1:06:33:e8:82:2b:ae:53:
                    a6:3a:16:73:8c:dd:41:0e:20:3a:c0:b4:a7:a1:e9:
                    b2:4f:90:2e:32:60:e9:57:cb:b9:04:92:68:68:e5:
                    38:26:60:75:b2:9f:77:ff:91:14:ef:ae:20:49:fc:
                    ad:40:15:48:d1:02:31:61:19:5e:b8:97:ef:ad:77:
                    b7:64:9a:7a:bf:5f:c1:13:ef:9b:62:fb:0d:6c:e0:
                    54:69:16:a9:03:da:6e:e9:83:93:71:76:c6:69:85:
                    82:17
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Subject Key Identifier:
                8F:F0:4B:7F:A8:2E:45:24:AE:4D:50:FA:63:9A:8B:DE:E2:DD:1B:BC
    Signature Algorithm: sha256WithRSAEncryption
         4b:40:db:c0:50:aa:fe:c8:0c:ef:f7:96:54:45:49:bb:96:00:
         09:41:ac:b3:13:86:86:28:07:33:ca:6b:e6:74:b9:ba:00:2d:
         ae:a4:0a:d3:f5:f1:f1:0f:8a:bf:73:67:4a:83:c7:44:7b:78:
         e0:af:6e:6c:6f:03:29:8e:33:39:45:c3:8e:e4:b9:57:6c:aa:
         fc:12:96:ec:53:c6:2d:e4:24:6c:b9:94:63:fb:dc:53:68:67:
         56:3e:83:b8:cf:35:21:c3:c9:68:fe:ce:da:c2:53:aa:cc:90:
         8a:e9:f0:5d:46:8c:95:dd:7a:58:28:1a:2f:1d:de:cd:00:37:
         41:8f:ed:44:6d:d7:53:28:97:7e:f3:67:04:1e:15:d7:8a:96:
         b4:d3:de:4c:27:a4:4c:1b:73:73:76:f4:17:99:c2:1f:7a:0e:
         e3:2d:08:ad:0a:1c:2c:ff:3c:ab:55:0e:0f:91:7e:36:eb:c3:
         57:49:be:e1:2e:2d:7c:60:8b:c3:41:51:13:23:9d:ce:f7:32:
         6b:94:01:a8:99:e7:2c:33:1f:3a:3b:25:d2:86:40:ce:3b:2c:
         86:78:c9:61:2f:14:ba:ee:db:55:6f:df:84:ee:05:09:4d:bd:
         28:d8:72:ce:d3:62:50:65:1e:eb:92:97:83:31:d9:b3:b5:ca:
         47:58:3f:5f
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzCV2kHkGeCIW9cCDtoTK
KJ79BXYRxa2IcvxGAkPHsoqdBF8kyy5L4WCCRuFSqwyBR3Bs3WTR6/Usow+CPQwr
rpfXthSGEHm7OxOAd4wI4UnSamIvH176lmjfiSeVOJ8G1z7JyyZZDXPesMjpJg6D
FcbvW4vSBGDKSaYo9mk79svIKJHlnYphVzesdBTcdOA67nIvLpz70Lu/9T0A4QYz
6IIrrlOmOhZzjN1BDiA6wLSnoemyT5AuMmDpV8u5BJJoaOU4JmB1sp93/5EU764g
SfytQBVI0QIxYRleuJfvrXe3ZJp6v1/BE++bYvsNbOBUaRapA9pu6YOTcXbGaYWC
FwIDAQAB
-----END PUBLIC KEY-----
-----BEGIN CERTIFICATE-----
MIIDXzCCAkegAwIBAgILBAAAAAABIVhTCKIwDQYJKoZIhvcNAQELBQAwTDEgMB4G
A1UECxMXR2xvYmFsU2lnbiBSb290IENBIC0gUjMxEzARBgNVBAoTCkdsb2JhbFNp
Z24xEzARBgNVBAMTCkdsb2JhbFNpZ24wHhcNMDkwMzE4MTAwMDAwWhcNMjkwMzE4
MTAwMDAwWjBMMSAwHgYDVQQLExdHbG9iYWxTaWduIFJvb3QgQ0EgLSBSMzETMBEG
A1UEChMKR2xvYmFsU2lnbjETMBEGA1UEAxMKR2xvYmFsU2lnbjCCASIwDQYJKoZI
hvcNAQEBBQADggEPADCCAQoCggEBAMwldpB5BngiFvXAg7aEyiie/QV2EcWtiHL8
RgJDx7KKnQRfJMsuS+FggkbhUqsMgUdwbN1k0ev1LKMPgj0MK66X17YUhhB5uzsT
gHeMCOFJ0mpiLx9e+pZo34knlTifBtc+ycsmWQ1z3rDI6SYOgxXG71uL0gRgykmm
KPZpO/bLyCiR5Z2KYVc3rHQU3HTgOu5yLy6c+9C7v/U9AOEGM+iCK65TpjoWc4zd
QQ4gOsC0p6Hpsk+QLjJg6VfLuQSSaGjlOCZgdbKfd/+RFO+uIEn8rUAVSNECMWEZ
XriX7613t2Saer9fwRPvm2L7DWzgVGkWqQPabumDk3F2xmmFghcCAwEAAaNCMEAw
DgYDVR0PAQH/BAQDAgEGMA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFI/wS3+o
LkUkrk1Q+mOai97i3Ru8MA0GCSqGSIb3DQEBCwUAA4IBAQBLQNvAUKr+yAzv95ZU
RUm7lgAJQayzE4aGKAczymvmdLm6AC2upArT9fHxD4q/c2dKg8dEe3jgr25sbwMp
jjM5RcOO5LlXbKr8EpbsU8Yt5CRsuZRj+9xTaGdWPoO4zzUhw8lo/s7awlOqzJCK
6fBdRoyV3XpYKBovHd7NADdBj+1EbddTKJd+82cEHhXXipa0095MJ6RMG3NzdvQX
mcIfeg7jLQitChws/zyrVQ4PkX4268NXSb7hLi18YIvDQVETI53O9zJrlAGomecs
Mx86OyXShkDOOyyGeMlhLxS67ttVb9+E7gUJTb0o2HLO02JQZR7rkpeDMdmztcpH
WD9f
-----END CERTIFICATE-----
```



### 证书信任链

> 证书直接是可以有信任关系的, 通过一个证书可以证明另一个证书也是真实可信的. 实际上，证书之间的信任关系，是可以嵌套的。比如，C 信任 A1，A1 信任 A2，A2 信任 A3......这个叫做证书的信任链。只要你信任链上的头一个证书，那后续的证书，都是可以信任滴。 
>
> 假设 C 证书信任 A 和 B；然后 A 信任 A1 和 A2；B 信任 B1 和 B2。则它们之间，构成如下的一个树形关系（一个倒立的树）。 ![image-20220120153735054](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220120153735054.png)
>
> 处于最顶上的树根位置的那个证书，就是“**根证书**”。除了根证书，其它证书都要依靠上一级的证书，来证明自己。那谁来证明“根证书”可靠捏？实际上，根证书自己证明自己是可靠滴（或者换句话说，根证书是不需要被证明滴)。



### 生成自签名证书

下列两种方式生成的证书都是pem格式的，可以导入到计算机。

- 私钥文件
- 数字证书（包含公钥）

#### 1. 方式一

创建私钥  》 创建请求 》生成证书

1. 创建一个目录如Mytest, 进入该目录, 在该目录下打开命令行窗口

2. 启动openssl

   ```shell
   openssl    # 执行该命令即可
   ```

3. 使用openssl工具生成一个RSA私钥, 注意：生成私钥，需要提供一个至少4位的密码。

   ```shell
   genrsa -des3 -out server.key 2048  // 2048私钥的位数，可以不指定，默认值：？？？
   	- des3: 使用3des对私钥进行加密，//使用req参数的可以不指定这个参数，加下面
   ```

4. 生成CSR（证书签名请求）

   会引导我们填写申请方的信息：国家，省份，城市，部门…, 格式是pem格式

   ```shell
   req -new -key server.key -out server.csr
   
   
   #查看请求
   req -in server.csr -text
   ```

5. 删除私钥中的密码, 第一步给私钥文件设置密码是必须要做的, 如果不想要可以删掉

   ```shell
   rsa -in server.key -out server.key
   	-out 参数后的文件名可以随意起
   ```

6. 生成自签名证书

   ```shell
   x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
   
   
   #生成的证书是pem进行base64编码的
   #查看方式
   ```



> 在Windows下安装，Openssl-Win64.exe
>
> 
>
> 进入到：C:\Program Files\OpenSSL-Win64\bin\openssl.exe
>
> 右键单击->管理员运行 -> OPenSSL >
>
> 如果不是管理员打开: Permission Denied —> 权限不够
>
> 执行 : genrsa -des3 -out server.key 2048

==自签名证书，自己颁发给自己，自己验证自己。==

#### 2. 方式二

不需要生成csr，直接生成证书，没有指定Subject相关的数据，所以还会引导输入

```sh
openssl req -x509 -newkey rsa:4096 -keyout server2.key -out cert.crt -days 365 -nodes
```

>  -nodes 不设置密码



解析证书：

```sh
 openssl x509 -in cert.pem  -text
```



### 常见的证书格式

#### 1. pem格式

我们使用openssl生成的都是pem格式的

解析过程

```sh
openssl x509 -in cert.crt -text
```



- Privacy Enhanced Mail(信封)

- 查看内容，以"-----BEGIN..."开头，以"-----END..."结尾。

- 查看PEM格式证书的信息：

  ```sh
  `Apache和*NIX服务器偏向于使用这种编码格式。
  openssl x509 -in certificate.pem -text -noout
  ```

  

#### 2. der格式

我们使用Windows导出的可以是der

对于der格式的，解析方式如下：

```sh
openssl x509 -in itcastcrt.cer -text -inform der  // 额外的参数 -inform der
```



- Distinguished Encoding Rules

- 打开看是二进制格式，不可读。

- Java和Windows服务器偏向于使用这种编码格式。

- 查看DER格式证书的信息

  ```sh
  `der是格式，与证书的后缀名没有直接关系
  openssl x509 -in certificate.der -inform der -text -noout  `请试试-pubkey参数
  ```



#### 3. windows导出格式选择

![image-20220120232314962](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220120232314962.png)

### 公钥基础设施（PKI）

> 仅制定证书的规范还不足以支持公钥的实际运用，我们还需要很多其他的规范，例如证书应该由谁来颁发，如何颁发，私钥泄露时应该如何作废证书，计算机之间的数据交换应采用怎样的格式等。这一节我们将介绍能够使公钥的运用更加有效的公钥基础设施。

#### 1. 什么是公钥基础设施

> 公钥基础设施（Public-Key infrastructure）是为了能够更有效地运用公钥而制定的一系列规范和规格的总称。公钥基础设施一般根据其英语缩写而简称为PKI。
>
> PKI只是一个总称，而并非指某一个单独的规范或规格。例如，RSA公司所制定的PKCS（Public-Key Cryptography Standards，公钥密码标准）系列规范也是PKI的一种，而互联网规格RFC（Requestfor Comments）中也有很多与PKI相关的文档。此外，X.509这样的规范也是PKI的一种。在开发PKI程序时所使用的由各个公司编写的API（Application Programming Interface, 应用程序编程接口）和规格设计书也可以算是PKI的相关规格。
>
> 因此，根据具体所采用的规格，PKI也会有很多变种，这也是很多人难以整体理解PKI的原因之一。
>
> 为了帮助大家整体理解PKI,我们来简单总结一下PKI的基本组成要素（用户、认证机构、仓库）以及认证机构所负责的工作。

#### 2. PKI的组成要素

PKI的组成要素主要有以下三个：

- 用户 --- 使用PKI的人
- 认证机构 --- 颁发证书的人
- 仓库 --- 保存证书的数据库

![image-20220120232509738](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220120232509738.png)

- 作废证书

  > 当用户的私钥丢失、被盗时，认证机构需要对证书进行作废（revoke）。此外，即便私钥安然无恙，有时候也需要作废证书，例如用户从公司离职导致其失去私钥的使用权限，或者是名称变更导致和证书中记载的内容不一致等情况。
  >
  > 纸质证书只要撕毁就可以作废了，但这里的证书是数字信息，即便从仓库中删除也无法作废，因为用户会保存证书的副本，但认证机构又不能人侵用户的电脑将副本删除。
  >
  > 要作废证书，认证机构需要制作一张证书==**作废清单（Certificate Revocation List),简称为CRL**==。
  >
  > CRL是认证机构宣布作废的证书一览表，具体来说，是一张已作废的证书序列号的清单，并由认证机构加上数字签名。证书序列号是认证机构在颁发证书时所赋予的编号，在证书中都会记载。
  >
  > PKI用户需要从认证机构获取最新的CRL,并查询自己要用于验证签名（或者是用于加密）的公钥证书是否已经作废这个步骤是非常重要的。
  >
  > 假设我们手上有Bob的证书，该证书有合法的认证机构签名，而且也在有效期内，但仅凭这些还不能说明该证书一定是有效的，还需要查询认证机构最新的CRL，并确认该证书是否有效。一般来说，这个检查不是由用户自身来完成的，而是应该由处理该证书的软件来完成，但有很多软件并没有及时更能CRL。

## 九、SSL

### HTTP, HTTPS, SSL/TLS

- HTTPS = HTTP + SSL/TLS

- 早期的版本SSL （3.0之后叫TLS）

- 现在：TLS

- 1.0 TLS = 3.0 SSL

- 1.1  TLS = 3.1 SSL, 目前版本TLS1.2



### 关系图示

![](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day4/03-资料/https不等于ssl.jpg)



### SSL通信图示

![](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day4/03-资料/ssl图示.jpg)



### SSL协议细节（拓展）

![tls协议分析](/Users/rainbowcat/go语言学习资料/4天掌握GO语言密码学-用实践验证理论资料/day4/03-资料/tls协议分析.png)

* SSL两层协议

1. 握手相关
2. 数据记录

### 代码实现

#### 一、http server单向认证

- 客户端认证服务器
- 服务器不认证客户端
- 服务器的证书使用openssl自签名证书（我们使用server.crt就可以当做ca证书）



##### 1. 服务器端

* 分析流程

```sh
1. 创建http server

2. 启动http server，启动时加载自己的证书， 启动时使用tls
```

* 生成服务器证书

使用-subj参数，指定服务器的相关信息，与之前的不同，此时不需要引导输入。

```sh
openssl req \
    -x509 \
    -nodes \
    -newkey rsa:2048 \
    -keyout server.key \
    -out server.crt \
    -days 3650 \
    -subj "/C=CN/ST=Beijing/L=Beijing/O=Global Security/OU=IT Department/CN=*"
    
```

* 代码

```go
package main

import (
	"net/http"
	"log"
	"fmt"
)

func main() {

	//1. 创建http server
	server := http.Server{
		//Addr    string  // TCP address to listen on, ":http" if empty
		Addr: ":8848", //监听端口

		//Handler Handler // handler to invoke, http.DefaultServeMux if nil
		Handler: nil, //填写nil时， 会使用默认的处理器， 还是要自己实现处理逻辑

		//TLSConfig *tls.Config
		TLSConfig: nil,
	}

	//编写处理逻辑
	http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
		fmt.Println("HandleFunc called!\n")
		writer.Write([]byte("hello world!!!!!"))
	})

	//2. 启动http server，启动时加载自己的证书， 启动时使用tls
	err := server.ListenAndServeTLS("./server.crt", "./server.key")

	if err != nil {
		log.Fatal(err)
	}
}

```



###### 附加知识：

1. ```
   localhost是本机的ip： 127.0.0.1
   ```

![image-20220121190105562](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220121190105562.png)

2. ```
   2. 一定要写成：https, =>  https://localhost:8848，
   
   3. 浏览器建议使用chrome
   
   4. server.crt， server.key与server.go放到同级目录中，所以我们的代码没有使用绝对路径
   
   5. func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {}
   
      第二个参数是回调函数，只有客户端有请求的时候，才会触发。
   ```

   

##### 2. 客户端

* 分析流程

```sh
1. 注册给服务器颁发证书的ca
- 读取ca证书
- 把ca的证书添加到ca池中

2. 配置tls

3.创建http client

4. client发起请求

5. 打印返回值
```

![image-20220121191715369](/Users/rainbowcat/Library/Application Support/typora-user-images/image-20220121191715369.png)

* 代码

```go
package main

import (
	"io/ioutil"
	"log"
	"crypto/x509"
	"crypto/tls"
	"net/http"
	"fmt"
)

func main() {

	//1. 注册给服务器颁发证书的ca
	//- 读取ca证书, 我们的证书是自签名的，server.crt能够认证自己，server.crt当成CA证书
	caCerInfo /*pem格式*/ , err := ioutil.ReadFile("./server.crt")
	if err != nil {
		log.Fatal(err)
	}

	//- 把ca的证书添加到ca池中
	//- 创建ca池
	cerPool := x509.NewCertPool()

	//- 将ca添加到ca池
	cerPool.AppendCertsFromPEM(caCerInfo)

	//
	//2. 配置tls
	// RootCAs defines the set of root certificate authorities
	// that clients use when verifying server certificates.
	// If RootCAs is nil, TLS uses the host's root CA set.
	//RootCAs *x509.CertPool

	//将我们承认ca池配置给tls
	cfg := tls.Config{
		RootCAs: cerPool,
	}

	//fmt.Printf("cfg : %s", cfg)

	//3.创建http client
	client := http.Client{
		Transport: &http.Transport{
			TLSClientConfig: &cfg,
			//TLSClientConfig: nil,
		},
	}

	//4. client发起请求
	response, err := client.Get("https://localhost:8848")
	if err != nil {
		log.Fatal(err)
	}

	//5. 打印返回值
	bodyInfo, err := ioutil.ReadAll(response.Body)

	if err != nil {
		log.Fatal(err)
	}

	//勿忘
	response.Body.Close()

	//body
	fmt.Printf("body : %s\n", bodyInfo)

	//状态码
	fmt.Printf("status code : %s\n", response.Status)
}
```





#### 二、双向认证

- 客户端认证服务器
- 服务器认证客户端
- 服务器的证书使用openssl自签名证书（我们使用server.crt就可以当做ca证书）
- 客户端的证书使用openssl自签名证书（我们使用client.crt就可以当做ca证书）



##### 1.服务器

* 分析流程

```sh
1. 注册client ca证书
- 读取client的ca证书
- 创建ca池
- 把client 的 ca 添加到ca池

2. 配置tls ==> cfg


3. 创建http server， 使用cfg

4. 启动http server，启动时加载自己的证书， 启动时使用tls
```

* 实现代码

```go
package main

import (
	"io/ioutil"
	"log"
	"crypto/x509"
	"crypto/tls"
	"net/http"
	"fmt"
)

func main() {

	//1. 注册client ca证书
	//- 读取client的ca证书, client的证书也是自签名的，自己认证自己
	caInfo, err := ioutil.ReadFile("./client.crt")
	if err != nil {
		log.Fatal(err)
	}

	//- 创建ca池
	caCertPool := x509.NewCertPool()

	//- 把client 的 ca 添加到ca池
	caCertPool.AppendCertsFromPEM(caInfo)

	//2. 配置tls ==> cfg
	cfg := tls.Config{
		// 我们要认证client, 需要两个字段
		// ClientAuth determines the server's policy for
		// TLS Client Authentication. The default is NoClientCert.
		//ClientAuth ClientAuthType

		//	const (
		//	NoClientCert ClientAuthType = iota
		//	RequestClientCert
		//	RequireAnyClientCert
		//	VerifyClientCertIfGiven
		//	RequireAndVerifyClientCert
		//)

		//我们设置服务器认证客户端
		ClientAuth: tls.RequireAndVerifyClientCert,

		// ClientCAs defines the set of root certificate authorities
		// that servers use if required to verify a client certificate
		// by the policy in ClientAuth.
		//ClientCAs *x509.CertPool
		ClientCAs: caCertPool, //客户端的ca池填充在这里
	}

	//3. 创建http server， 使用cfg
	server := http.Server{
		//三个字段Addr, Handler, TLSConfig
		Addr: ":8848",
		//Handler:   nil,
		Handler:   myhandler{},
		TLSConfig: &cfg,
	}

	fmt.Printf("准备启动服务器...\n")
	//4. 启动http server，启动时加载自己的证书， 启动时使用tls
	err = server.ListenAndServeTLS("./server.crt", "./server.key")
	if err != nil {
		log.Fatal(err)
	}
}

type myhandler struct {
}

func (h myhandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	fmt.Printf("ServeHTTP called!\n")
	w.Write([]byte("hello world!!!!"))
}

```







##### 2.客户端

* 分析流程

```sh
1. 注册给服务器颁发证书的ca
- 读取ca证书
- 把ca的证书添加到ca池中

1.5 加载客户端的证书和秘钥 ==> clientCert(修改了)

2. 配置tls, ==》 增加clientCert(修改了)

3.创建http client

4. client发起请求

5. 打印返回值
```

* 实现代码

```go
package main

import (
	"io/ioutil"
	"log"
	"crypto/x509"
	"crypto/tls"
	"net/http"
	"fmt"
)

func main() {

	//1. 注册给服务器颁发证书的ca
	//- 读取ca证书
	caCertInfo, err := ioutil.ReadFile("./server.crt")

	if err != nil {
		log.Fatal(err)
	}

	//- 把ca的证书添加到ca池中
	//- 创建ca pool
	caCertPool := x509.NewCertPool()
	//添加caCert
	caCertPool.AppendCertsFromPEM(caCertInfo)

	//
	//1.5 加载客户端的证书和秘钥 ==> clientCert(修改了)
	//func LoadX509KeyPair(certFile, keyFile string) (Certificate, error) {
	clientCert, err := tls.LoadX509KeyPair("./client.crt", "./client.key")

	if err != nil {
		log.Fatal(err)
	}

	//
	//2. 配置tls, ==》 增加clientCert(修改了)
	//- RootCAs
	//Certificates
	cfg := tls.Config{
		//服务器的ca池
		RootCAs: caCertPool,

		//客户端证书
		Certificates: []tls.Certificate{clientCert},
	}

	//
	//3.创建http client
	client := http.Client{
		Transport: &http.Transport{
			TLSClientConfig: &cfg,
		},
	}

	//4. client发起请求
	response, err := client.Get("https://localhost:8848")
	if err != nil {
		log.Fatal(err)
	}

	bodyInfo, err := ioutil.ReadAll(response.Body)
	if err != nil {
		log.Fatal(err)
	}

	defer response.Body.Close()

	//5. 打印返回值
	fmt.Printf("body info : %s\n", bodyInfo)
	fmt.Printf("status code : %s\n", response.Status)
}
```



















## 补充

### 凯撒密码

hello world -> 加密 =》khoor zrug -> 解密 -> hello world

加密：左移k位

1. 明文： hello world
2. 算法：向右移动
3. 秘钥：3

解密：右移k位

1. 密文：khoor zrug
2. 算法：向左移动（与加密算法不同）
3. 秘钥：3