---
sidebar_position: 7
---

# Phiên đấu giá

- Bid:

  - Thời gian phiên đấu giá còn hoạt động

  - Giá trị đặt nó phải lớn hơn giá trị lớn nhất tại thời điểm đó

  - Bid != 0 (sử dụng `uint`) => Bid > 0

- Withdraw:

  - `amount` > 0

  - Rút `amount` == bid

  - After send = 0

- Auction end:

  - Khi nào kết thúc phiên đấu giá

  - Sự kiện: Transfer (transfer tiền cho người gửi vật phẩm đấu giá, và transfer vật phẩm cho người đấu giá cao nhất)

## Từ khóa `payable` trong Solidity

Trong Solidity, từ khóa `payable` được sử dụng để chỉ rằng một hàm có khả năng nhận Ether (tiền điện tử của mạng Ethereum). Nếu một hàm được đánh dấu là `payable`, nó có nghĩa là nó có thể được gọi với một giao dịch bao gồm một giá trị (còn gọi là "wei") và giá trị này sẽ được chuyển đến số dư hợp đồng.

Đây là một ví dụ về một hàm payable trong Solidity:

```cpp
pragma solidity ^0.6.0;

contract MyContract {
    address public owner;
    uint public balance;

    constructor() public {
        owner = msg.sender;
        balance = 0;
    }

    function deposit() public payable {
        require(msg.value > 0, "Must send a positive value");
        balance += msg.value;
    }
}
```

Trong ví dụ này, hàm `deposit` được đánh dấu là `payable`, có nghĩa là nó có thể được gọi với một giao dịch bao gồm một giá trị. Giá trị của giao dịch sẽ được thêm vào số dư hợp đồng, được lưu trữ trong biến `balance`.

Quan trọng là chỉ các hàm được đánh dấu là `payable` mới có thể nhận Ether. Nếu bạn cố gửi Ether đến một hợp đồng không có hàm payable, giao dịch sẽ thất bại.

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
