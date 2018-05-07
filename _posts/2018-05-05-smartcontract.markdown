---
layout:       post
title:        "智能合约"
subtitle:     "smartcontract"
date:         2018-05-05
author:       "Gxm"
header-img:   "img/project.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - blockchain
---

send_transaction:
参数列表：
from： 发送地址
to: 目的地址
value：发送数量
nonce: 交易次数
gasprice: gas 价格
gaslimit: gas 限制

最基本的智能合约例子：
```
"use strict";
var BankVaultContract = function () {

  // nothing

};
BankVaultContract.prototype = {
  init: function() {

      // nothing

  }
};
module.exports = BankVaultContract;
```

银行存取钱智能合约例子：
```
'use strict'; //开启严格模式语法

var DepositeContent = function (text) {//创建账户
  if (text) {//对账户初始化。若传入参数，账户余额和链高度初始化为参数内容；否则置为0
    var o = JSON.parse(text);
    this.balance = new BigNumber(o.balance);
    this.expiryHeight = new BigNumber(o.expiryHeight);
  } else {
    this.balance = new BigNumber(0);
    this.expiryHeight = new BigNumber(0);
  }
};

DepositeContent.prototype = {//prototype 中toString方法
  toString: function () {
    return JSON.stringify(this);
  }
};

var BankVaultContract = function () {//函数表达式存储于变量中，通过变量名来调用函数
  LocalContractStorage.defineMapProperty(this, "bankVault", { //定义bankvault属性
    parse: function (text) {
      return new DepositeContent(text);
    },
    stringify: function (o) {
      return o.toString();
    }
  });
};

// save value to contract, only after height of block, users can takeout
BankVaultContract.prototype = {//智能合约必须是一个prototype对象，必须包含init方法，私有方法以_开头。
  init: function () {//init方法，只在部署智能合约的时候被执行一次
    //TODO:
  },

  save: function (height) { 
    //存钱的逻辑：
    //1: 取得发送地址，待交易数量，链高度；
    //2: 确认当前地址是否已开户，未开户的话需要新开账户
    //3: 在链上跟新交易数量
    //4: 一致化操作
    var from = Blockchain.transaction.from;
    var value = Blockchain.transaction.value;
    var bk_height = new BigNumber(Blockchain.block.height);
    var orig_deposit = this.bankVault.get(from);
    if (orig_deposit) {
      value = value.plus(orig_deposit.balance);
    }

    var deposit = new DepositeContent();
    deposit.balance = value;
    deposit.expiryHeight = bk_height.plus(height);

    this.bankVault.put(from, deposit);
  },

  takeout: function (value) {
    //取钱的逻辑
    //1: 取得交易地址，链高度
    //2: 校验是否开户，，取钱数量是否超过账户余额
    //3: event记录交易事件
    //4: 一致化操作
    var from = Blockchain.transaction.from;
    var bk_height = new BigNumber(Blockchain.block.height);
    var amount = new BigNumber(value);

    var deposit = this.bankVault.get(from);
    if (!deposit) {
      throw new Error("No deposit before.");
    }

    if (bk_height.lt(deposit.expiryHeight)) {
      throw new Error("Can not takeout before expiryHeight.");
    }

    if (amount.gt(deposit.balance)) {
      throw new Error("Insufficient balance.");
    }

    var result = Blockchain.transfer(from, amount);
    if (!result) {
      throw new Error("transfer failed.");
    }
    Event.Trigger("BankVault", {
      Transfer: {
        from: Blockchain.transaction.to,
        to: from,
        value: amount.toString()
      }
    });

    deposit.balance = deposit.balance.sub(amount);
    this.bankVault.put(from, deposit);
  },
  balanceOf: function () {
    var from = Blockchain.transaction.from;
    return this.bankVault.get(from);
  },
  verifyAddress: function (address) {
    // 1-valid, 0-invalid
    var result = Blockchain.verifyAddress(address);
    return {
      valid: result == 0 ? false : true
    };
  }
};
module.exports = BankVaultContract;
```
