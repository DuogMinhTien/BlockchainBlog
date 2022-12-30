---
sidebar_position: 6
---

# `msg` trong Solidity

Trong Solidity, `msg` là một biến toàn cục đặc trưng được định nghĩa sẵn mà biểu thị tin nhắn hiện tại đang được xử lý. Nó là một thể hiện của kiểu dữ liệu `Message` và chứa nhiều thuộc tính khác nhau cung cấp thông tin về tin nhắn hiện tại. Đây là định nghĩa đầy đủ của kiểu dữ liệu `Message`:

```cpp
struct Message {
    address payable sender;
    address payable recipient;
    uint256 value;
    bytes memory data;
    bytes memory signature;
}
```

Thuộc tính `sender` chứa địa chỉ của tài khoản gửi tin nhắn. Thuộc tính `recipient` chứa địa chỉ của tài khoản nhận tin nhắn. Thuộc tính value chứa số lượng Ether được gửi cùng tin nhắn (nếu có). Thuộc tính `data` chứa dữ liệu được truyền cùng tin nhắn, và thuộc tính `signature` chứa tiền tố tin nhắn được ký số Ethereum của tin nhắn.

Đây là một ví dụ về cách sử dụng `msg` trong một hợp đồng Solidity:

```cpp
pragma solidity ^0.6.0;

contract MyContract {
    address public owner;

    constructor() public {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Must send a positive value");
    }
}
```

Trong ví dụ này, thuộc tính `msg.sender` được sử dụng để đặt giá trị của biến `owner` bằng địa chỉ của tài khoản tạo hợp đồng. Thuộc tính `msg.value` cũng được sử dụng trong hàm `deposit` để đảm bảo rằng hàm chỉ có thể được gọi với một giá trị Ether dương.

## Full code

```cpp
pragma solidity >=0.7.4 <0.9.0;

contract simpleAuction {
    //Variable

    address payable public benificiary;
    uint public auctionEndTime;
    uint public highestBid;
    address public highestBidder;
    bool ended = false;

    event highestBidIncrease (address bidder, uint amount);
    event auctionEnded (address winner, uint amount);

    mapping (address => uint) public pendingReturns;

    constructor (uint _biddingTime, address payable _benificiary) {
        benificiary = _benificiary;
        auctionEndTime = block.timestamp + _biddingTime;
    }

    //Function
    function bid () public payable {
        if (block.timestamp > auctionEndTime) {
            revert ("Phien dau gia da ket thuc!");
        }
        if (msg.value <= highestBid) {
            revert ("Gia cua ban thap hon gia cao nhat!");
        }
        if (highestBid != 0) {
            pendingReturns[highestBidder] += highestBid;
        }
        highestBidder = msg.sender;
        highestBid = msg.value;
        emit highestBidIncrease (msg.sender, msg.value);
    }

    function withdraw () public returns (bool){
        uint amount = pendingReturns[msg.sender];
        if (amount > 0) {
            pendingReturns[msg.sender] = 0;
            if (!payable (msg.sender).send (amount)) {
                pendingReturns[msg.sender]  = amount;
                return false;
            }
        }
        return true;
    }

    function auctionEnd () public {
        if (ended) {
            revert ("Phien dau da co the ket thuc");
        }

        if (block.timestamp < auctionEndTime) {
            revert ("Thoi gian phien dau gia chua ket thuc");
        }

        ended = true;

        emit auctionEnded (highestBidder, highestBid);

        benificiary.transfer (highestBid);
    }
}
```
