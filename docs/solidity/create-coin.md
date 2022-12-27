---
sidebar_position: 2
---

# Tạo 1 đồng coin, crypto

## Quy trình

- Tạo token/coin

- Tạo số lượng token

  - Người khởi tạo `Minter`

  - Lượng cung `Suply`

  - Số dư `Balance`

- Sent token

  - Người nhận `Receiver`

    - `Amount` < `Balance`, không đủ tiền chuyển!!

    - `Balance sender` - `Amount`

    - `Balance receiver` + `Amount`

  - Số lượng chuyển `Amount`

## Bắt Đầu Code

```jsx title="first program"
// SPDX-License-Identifier: MIT
pragma solidity >= 0.7.0 <0.9.0;

contract FirstConstract {
    address public minter;
    mapping (address => uint) public balances;

    constructor () {
        minter = msg.sender;
    }
}
```

Đây là mã Solidity cho một hợp đồng đơn giản được gọi là `FirstContract`. Hợp đồng này có các đặc tính sau:

- Hợp đồng có một biến công khai được gọi là `minter` kiểu `address`, lưu trữ địa chỉ người tạo hợp đồng.

- Hợp đồng có một bản đồ công khai được gọi là `balances`, bản đồ địa chỉ vào số nguyên không dấu (`uint`). Bản đồ này có thể được sử dụng để lưu trữ số dư của mỗi địa chỉ.

- Hợp đồng có một hàm khởi tạo, đó là một hàm đặc biệt được thực thi khi hợp đồng được triển khai. Trong trường hợp này, hàm khởi tạo đặt giá trị của biến `minter` bằng địa chỉ người tạo hợp đồng, được lưu trữ trong biến `msg.sender`.

### `Mapping` là gì?

Trong Solidity, `mapping` là một kiểu dữ liệu định nghĩa một bản đồ giữa một kiểu dữ liệu này và một kiểu dữ liệu khác. Bản đồ này có thể được sử dụng để lưu trữ giá trị tương ứng của mỗi giá trị của kiểu dữ liệu đầu vào.

Ví dụ, bạn có thể khai báo một bản đồ giữa địa chỉ và số nguyên không dấu bằng cách sử dụng cú pháp sau:

```jsx
mapping (address => uint) public balances;
```

Trong ví dụ này, bản đồ `balances` có thể được sử dụng để lưu trữ số dư tương ứng của mỗi địa chỉ. Ví dụ, bạn có thể gán giá trị cho một địa chỉ bằng cách sử dụng cú pháp sau:

```jsx
balances[0x1234567890] = 100;
```

Trong ví dụ này, giá trị `100` sẽ được lưu trữ trong bản đồ `balances` tương ứng với địa chỉ `0x1234567890`.

### `msg.sender` là gì?

Trong Solidity, `msg.sender` là một biến có sẵn mà lưu trữ địa chỉ của người gửi tin nhắn (hoặc hành động) hiện tại. Đây là một biến địa chỉ rất hữu ích trong các hợp đồng Smart Contract, vì nó cho phép bạn xác định địa chỉ của người gửi tin nhắn hoặc hành động hiện tại.

Ví dụ, bạn có thể sử dụng `msg.sender` trong một hàm của hợp đồng Smart Contract để xác định địa chỉ của người gửi tin nhắn hoặc hành động hiện tại và thực hiện một số hành động tương ứng. Ví dụ:

```jsx
function transfer(address _to, uint _value) public {
    require(balances[msg.sender] >= _value, "Insufficient balance");
    balances[msg.sender] -= _value;
    balances[_to] += _value;
}
```

Trong ví dụ này, hàm `transfer` sử dụng `msg.sender` để xác định địa chỉ của người gửi tin nhắn hoặc hành động hiện tại và kiểm tra xem địa chỉ này có đủ số dư để thực hiện chuyển khoản. Nếu địa chỉ này có đủ số dư, hàm sẽ thực hiện chuyển khoản từ địa chỉ này sang địa chỉ đích được cung cấp.

### Hàm `require` trong `Solidity`

Trong Solidity, `require` là một hàm có sẵn mà được sử dụng để kiểm tra một điều kiện và ném một lỗi nếu điều kiện không đúng. Đây là một cách thuận tiện để kiểm tra các điều kiện trước khi thực hiện một hành động nào đó trong hợp đồng Smart Contract.

Ví dụ, bạn có thể sử dụng `require` để kiểm tra xem một địa chỉ có đủ số dư để thực hiện chuyển khoản như sau:

```jsx
require(balances[msg.sender] >= _value, "Insufficient balance");
```

Trong ví dụ này, nếu số dư của địa chỉ gửi tin nhắn hoặc hành động hiện tại (lưu trữ trong biến `balances[msg.sender])` không lớn hơn hoặc bằng giá trị được chuyển khoản (\_value), hàm `require` sẽ ném một lỗi với thông báo "Insufficient balance". Nếu điều kiện đúng, hàm `require` sẽ không làm gì cả và tiếp tục thực hiện

## Tiếp tục chương trình dang dở

### Thêm function `mint` vào chương trình

```jsx
function  mint (address receiver, uint amount) public {
    require (msg.sender == minter);
    require (amount < 1e60)

    balances [receiver] += amount;
}
```

Đây là một đoạn code mô tả một hàm tên là `mint` cho phép một người dùng có địa chỉ `minter` để ra mắt một số lượng đặc biệt của một tài sản kỹ thuật số và gửi nó đến một địa chỉ nhận `receiver` được chỉ định.

Hàm này có hai câu lệnh `require` đóng vai trò như các kiểm tra hoặc điều kiện mà phải được đáp ứng để hàm được thực thi.

Câu lệnh `require` đầu tiên kiểm tra rằng `msg.sender` (địa chỉ của người dùng gọi hàm) là giống như địa chỉ `minter`. Nếu không phải là trường hợp này, hàm sẽ không được thực thi và sẽ trả về lỗi. Điều này đảm bảo rằng chỉ có địa chỉ `minter` mới có thể sử dụng hàm này.

Câu lệnh `require` thứ hai kiểm tra rằng số lượng được ra mắt là ít hơn `1e60`. Đây là một số rất lớn (1 sau theo 60 số 0) và làm giới hạn tối đa cho số lượng có thể được ra mắt. Nếu số lượng được ra mắt lớn hơn giá trị này, hàm sẽ không được thực thi và sẽ trả về lỗi.

Nếu cả hai điều kiện này đều được đáp ứng, hàm sẽ được thực thi và số lượng sẽ được cộng vào số dư của địa chỉ nhận `receiver`, được lưu trữ trong bảng `balances`.

### Thêm function `send` vào chương trình

```jsx
function send (address receiver, uint amount) public {
    require (amount <= balances[msg.sender], "Insufficient balance");

    balances[msg.sender] -= amount;
    balances[receiver] += amount;
}
```

Đây là một đoạn code mô tả một hàm tên là `send` cho phép một người dùng gửi một số lượng đặc biệt của một tài sản kỹ thuật số đến một địa chỉ nhận `receiver` được chỉ định.

Hàm này có một câu lệnh `require` đóng vai trò như một kiểm tra hoặc điều kiện mà phải được đáp ứng để hàm được thực thi.

Câu lệnh `require` kiểm tra rằng số lượng đang được gửi là nhỏ hơn hoặc bằng số dư của `msg.sender`, địa chỉ của người dùng gọi hàm. Nếu không phải là trường hợp này, hàm sẽ không được thực thi và sẽ trả về lỗi với thông báo "Số dư không đủ". Điều này đảm bảo rằng người dùng có đủ tài sản kỹ thuật số để hoàn tất giao dịch.

Nếu câu lệnh `require` được đáp ứng, hàm sẽ được thực thi và số lượng sẽ được trừ khỏi số dư của địa chỉ `msg.sender` và cộng vào số dư của địa chỉ nhận `receiver`. Các số dư này được lưu trữ trong bảng `balances`.

## Thêm `event` vào chương trình

```jsx title="Thêm đoạn code này vào phía trên constructor ()"
event sent(address from, address to, uint amount);
```

Đây là một đoạn code mô tả một sự kiện tên là `sent` cho phép bạn theo dõi khi một tài sản kỹ thuật số được gửi từ một địa chỉ (`from`) đến một địa chỉ khác (`to`). Số lượng tài sản đang được gửi cũng được bao gồm trong tham số.

Sự kiện trong Solidity (ngôn ngữ lập trình được sử dụng để viết hợp đồng thông minh Ethereum) là một cách để ghi lại dữ liệu hoặc kích hoạt hành động khi điều kiện nhất định được đáp ứng. Trong trường hợp này, sự kiện `sent` sẽ được kích hoạt mỗi khi tài sản kỹ thuật số được gửi từ một địa chỉ đến một địa chỉ khác, và dữ liệu liên quan (địa chỉ `from`, địa chỉ `to` và số lượng `amount`) sẽ được ghi lại.

Bạn có thể sử dụng sự kiện trong hợp đồng của bạn để nghe và phản hồi đến các sự kiện đặc biệt xảy ra trên blockchain Ethereum. Ví dụ, bạn có thể sử dụng sự kiện `sent` trong một hợp đồng để cập nhật số dư của người dùng hoặc để kích hoạt một hành động trong hợp đồng khác.

Sự kiện được định nghĩa với từ khóa `event`, sau đó là tên của sự kiện (`sent`) và một danh sách tham số bao quanh trong dấu ngoặc. Trong trường hợp này, các tham số là ba biến có kiểu `address` (`from`, `to`) và `uint` (`amount`), đại diện cho địa chỉ của người gửi và người nhận và số lượng tài sản đang được gửi, tương ứng.

### Gọi 1 `event`

```jsx
emit sent (msg.sender, receiver, amount);
```

Đây là một dòng code được sử dụng để kích hoạt (hoặc "phát ra") một sự kiện. Trong trường hợp này, sự kiện được phát ra là sự kiện `sent` đã được định nghĩa trước đó trong code, và biến `msg.sender`, `receiver`, và `amount` đang được truyền vào làm tham số cho sự kiện.

Khi dòng code này được thực thi, nó sẽ kích hoạt sự kiện `sent` và ghi lại dữ liệu `msg.sender`, `receiver`, và `amount`. Điều này cho phép bạn theo dõi khi hàm `send` được gọi và dữ liệu nào được truyền vào sự kiện.

Từ khóa `emit` được sử dụng để kích hoạt một sự kiện trong Solidity. Tên của sự kiện (trong trường hợp này là `sent`) và dữ liệu liên quan (các biến `msg.sender`, `receiver`, và `amount`) được truyền vào như tham số cho từ khóa `emit`.

Quan trọng là sự kiện chỉ được kích hoạt khi chúng được gọi riêng biệt bằng từ khóa `emit`. Trong trường hợp này, sự kiện `sent` chỉ sẽ được kích hoạt khi hàm `send` được gọi và câu lệnh `emit` được thực thi.

**Thêm đoạn code trên vào function send**

```jsx
function send (address receiver, uint amount) public {
    require (amount <= balances[msg.sender], "Insufficient balance");

    balances[msg.sender] -= amount;
    balances[receiver] += amount;

    emit sent (msg.sender, receiver, amount);
}
```

## Đoạn code hoàn chỉnh

```jsx
// SPDX-License-Identifier: MIT
pragma solidity >= 0.7.0 <0.9.0;

contract FirstConstract {
    address public minter;
    mapping (address => uint) public balances;

    event sent(address from, address to, uint amount);

    constructor () {
        minter = msg.sender;
    }

    function  mint (address receiver, uint amount) public {
        require (msg.sender == minter);
        require (amount < 1e60)

        balances [receiver] += amount;
    }

    function send (address receiver, uint amount) public {
        require (amount <= balances[msg.sender], "Insufficient balance");

        balances[msg.sender] -= amount;
        balances[receiver] += amount;

        emit sent (msg.sender, receiver, amount);
    }
}
```
