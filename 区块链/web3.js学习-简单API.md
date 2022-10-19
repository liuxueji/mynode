## 前言

这是我在区块链专栏发的第一篇文章，入职以来学习了很多区块链相关知识，但是都零零散散的，我还是喜欢通过写文章的形式去学习知识，因为学习不仅需要输入，还需要输出

## 常见API

这一块查web3.js 的[API文档]([web3.js - 以太坊 JavaScript API — web3.js 中文文档 — 登链社区 (learnblockchain.cn)](https://learnblockchain.cn/docs/web3.js/))是最好的

```
//导入web3模块，导入方法有很多，按照自己的环境导入

import Web3 from "web3";

//web3相关文档：https://learnblockchain.cn/docs/web3.js/

//初始化web3对象，后面的参数host是对应节点的地址，这里我们用https://kovan测试网络
//测试网络节点：去https://infura.io/注册获取  水龙头：https://faucet.kovan.network/ 区块浏览器： https://kovan.etherscan.io/

//因为不能用于订阅，HTTP provider 已经**不推荐使用**
this.web3 = new Web3(new Web3.providers.HttpProvider("https://eth-testnet.tokenlon.im"));
//也可以写成 this.web3 = new Web3('https://eth-testnet.tokenlon.im');
console.log(this.web3)

//改变provider，HTTP provider 不推荐使用，那我们改成WebsocketProvider: Websocket provider 是用于传统的浏览器中的标准方法.
// this.web3.setProvider('wss://eth-testnet.tokenlon.im');

//在和以太坊兼容的浏览器中使用 web3.js 时，当前环境的原生 provider 会被浏览器设置
//例如安装了MetaMask，它在浏览器中注入了window.ethereum下的提供者对象，我们就可以通过window.ethereum来初始化web3对象

//web3.givenProvider 将返回浏览器设置的原生 provider
console.log("返回浏览器设置的原生 provider",this.web3.givenProvider);
//也可以通过获取当前的provider
console.log("获取当前的provider",this.web3.currentProvider);

//判断是否连接成功
// this.web3Connected = web3.isConnected();
console.log("判断是否连接成功", this.web3.currentProvider.connected);

//查看web3版本
console.log("查看web3版本", this.web3.version)


//======web3通用工具方法======


//1.以太单位转换

//关于ETH单位：https://blog.csdn.net/wo541075754/article/details/79049425
//将wei转换为其他单位
console.log(this.web3.utils.fromWei("1000000000000000000","ether"))
//将其他单位转换成wei
console.log(this.web3.utils.toWei("1","ether"))

//2.数据类型转换
//16进制转为10进制，参数类型String
console.log(this.web3.utils.toDecimal("0x68"))

//数字转为16进制，参数类型String/Number
console.log(this.web3.utils.fromDecimal(233))

//将任意类型转换为16进制
//Number => Hex
console.log(this.web3.utils.toHex(233))

//将数字或16进制字符串转换为BigNumber
console.log(this.web3.utils.toBN('12718838912120101'))
console.log(this.web3.utils.toBN('0x68'))

//16 进制字符串的数字表示
console.log(this.web3.utils.hexToNumberString('0x68'))
//返回 16 进制字符串的Number表示
console.log(this.web3.utils.hexToNumber('0x68'))


//======账户相关操作 web3.eth======

//1.查看挖矿成功奖励的地址:可以用来获取当前账户
console.log("查看挖矿成功奖励的地址",this.web3.eth.coinbase)
//2.账户查询
console.log("账户查询",this.web3.eth.accounts)


console.log("获取链 ID",this.web3.eth.net.getId())
console.log("ChainId获取链 ID",this.web3.eth.getChainId())

//查询当前区块高度web3.eth.getBlockNumber().then(console.log);
console.log("查询当前区块高度", this.web3.eth.getBlockNumber())

```

