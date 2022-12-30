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

