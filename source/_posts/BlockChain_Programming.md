---
title: Solidity合约编程
abbrlink: 9403bcca
date: 2021-12-09 02:23:32
---
# Solidity合约编程
## QuickStart
```solidity
pragma solidity ^0.8.4;

contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;

    // Like C++ map<> / Python dict
    mapping (address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```
- 只有`public`成员变量可以被外界访问。此时这一成员类似property，包装了Getter和Setter
- `event`便于关心的用户通过订阅来追踪某些交易。它对于Transaction而言不是必要的。`emit`用于主动触发事件
- `error`定义一个异常；`revert`用于抛出异常（说明：`revert`会回滚当前调用的所有操作，包括发过来的币）
- `msg`是一个预定义的特殊变量，表示调用合约的消息
  - `msg.sender`是一个`address`，表示当前调用合约的账户地址
  - `msg.value`表示调用者附带（发送）的币值
- `block`是一个预定义的特殊变量，表示当前交易所在的Block
  - `block.timestamp` 时间戳
- `address`的值是一个不可算术运算的hash串，但Solidty提供对`address`的操作原语
  - `send`原语（需要`payable`的账户）：向`address`发送币；**转账失败返回false，不会抛出异常**
  - `transfer`原语：同`payable`账户；**转账失败会抛出异常**