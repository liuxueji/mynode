构建web3对象

```
//在和以太坊兼容的浏览器中使用 web3.js 时，当前环境的原生 provider 会被浏览器设置
  //例如安装了MetaMask，它在浏览器中注入了window.ethereum下的提供者对象，我们就可以通过window.ethereum来初始化web3对象
  var web3Provider;
  if (window.ethereum) {
	web3Provider = window.ethereum;
	try {
	  // 请求用户授权
	  await window.ethereum.enable();
	} catch (error) {
	  //用户不授权时，这里处理异常情况，同时可以设置重试等机制
	  console.log(error)
	}
  } else if (window.web3) {// 老版 MetaMask Legacy dapp browsers...
	web3Provider = window.web3.currentProvider;
  } else {
	// 这里处理连接在不支持dapp的地方打开的情况
	web3Provider = new Web3.providers.HttpProvider('https://eth-testnet.tokenlon.im');
	console.log("Non-Ethereum browser detected. You should consider trying MetaMask!")

	//这种情况下表示用户在不支持dapp的浏览器打开，无法初始化web3

	//这里一般的处理逻辑是：使用第一篇中的那种自己初始化，获得区块上的基础数据，但是没法获取用户的账户信息
	//或者直接提示错误，不进行任何处理
  }
  this.web3 = new Web3(web3Provider);

```

初始化后就可以通过web3对象进行相关操作

### 基础

- 获取账户地址

  ```
  console.log("获取账户地址Coinbase",  this.web3.eth.getCoinbase());
  ```

- 获取metamask中连接的所有账户

  ```
  console.log(this.web3.eth.getAccounts());
  ```

- 获取网络ID，用来区分在测试网还是正式网

  ```
  console.log("获取链 ID",this.web3.eth.net.getId())
  console.log("ChainId获取链 ID",this.web3.eth.getChainId())this.web3.eth.net.getId((err, netID) => {
      // Main Network: 1   表示主网
      // Ropsten Test Network: 3  //测试网Ropsten
      // Kovan Test Network: 42  //测试网Kovan
      // Rinkeby Test Network: 4  //测试网Rinkeby
     console.log(netID)});
  ```

  

- 查询当前区块高度

  ```
  console.log("查询当前区块高度", this.web3.eth.getBlockNumber())
  this.web3.eth.getBlockNumber().then(console.log);
  ```

  

- 查询某个地址的余额，单位是wei

  ```
  this.web3.eth.getBalance("0xFedC0F66dDba0a7EE51F47b894Db35b222b0EF42").then(console.log);
  ```

- 获取当前metamask账户地址的eth余额

  ```
  this.web3.eth.getCoinbase((err, coinbase) =>{
       this.web3.eth.getBalance(coinbase).then(console.log);
  })
  ```

  

- 通过hash查询交易

  ```
  console.log("查询交易",this.web3.eth.getTransaction('0x0f77813b2f252b0b189824bcac09d19d0d8e0a3ae506dfbb0a90906d115d4d2c'))
  ```

  

- 查询交易Receipt，一般通过这里的status来判断是否成功

  ```
  console.log("查询交易receipt",this.web3.eth.getTransactionReceipt('0x0f77813b2f252b0b189824bcac09d19d0d8e0a3ae506dfbb0a90906d115d4d2c'))
  
  ```

  