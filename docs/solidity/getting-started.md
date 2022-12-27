---
sidebar_position: 1
---

# Khởi động

Để chạy chương trình Solidity cần sử dụng trình biên dịch và deploy trên [Remix](https://remix.ethereum.org/)

![Ảnh khái quát về Remix](/img/remix.png)

## First Program

```jsx
// SPDX-License-Identifier: MIT
pragma solidity >= 0.7.0 <0.9.0;

contract FirstConstract {
    uint saveData;
    function set(uint x) public {
        saveData = x;
    }
    function get() public view returns (uint x) {
        return saveData;
    }
}
```

Đây là một hợp đồng Solidity đơn giản cho blockchain Ethereum.

- Dòng đầu tiên, `// SPDX-License-Identifier: MIT`, là một bình luận chỉ ra giấy phép mà hợp đồng được phân phối. Giấy phép MIT là một giấy phép mã nguồn mở cho phép người khác sử dụng, sửa đổi và phân phối mã nguồn mà không hạn chế.

- Dòng thứ hai, `pragma solidity >= 0.7.0 <0.9.0;`, chỉ ra phiên bản của trình biên dịch Solidity mà nên sử dụng để biên dịch hợp đồng. Dòng này chỉ ra rằng hợp đồng nên được biên dịch với phiên bản của trình biên dịch Solidity lớn hơn hoặc bằng 0.7.0 và nhỏ hơn 0.9.0.

- Hợp đồng được định nghĩa bằng từ khóa `contract`, sau đó là tên của hợp đồng, `FirstConstract`.

- Trong hợp đồng, có một biến trạng thái `saveData` của kiểu `uint`, tức là số nguyên không âm.

- Hợp đồng có hai hàm: `set` và `get`. Hàm `set` nhận một đầu vào `x` của kiểu `uint` và gán nó cho biến trạng thái `saveData`. Hàm `get` trả về giá trị của biến trạng thái saveData. Cả hai hàm được đánh dấu là `public`, tức là chúng có thể được gọi từ bên ngoài hợp đồng.

- Từ khóa `returns` trong hàm `get` chỉ ra đầu ra của hàm. Trong trường hợp này, hàm trả về một đầu ra duy nhất `x` của kiểu `uint`.

## Từ khóa `view` là gì?

```jsx title="Function get có sử dụng từ khóa view"
function get() public view returns (uint x) {
    return saveData;
}
```

Từ khóa `view` được sử dụng để cho biết rằng một hàm không thay đổi trạng thái của hợp đồng và không thực hiện bất kỳ hành động nào mà tốn phí gas. Điều này hữu ích cho tối ưu hóa hiệu suất của hợp đồng của bạn, bởi vì nó cho phép `Máy Ảo Ethereum (EVM)` tối ưu hóa thực thi của hàm.

Sử dụng từ khóa `view` cũng có thể làm cho hợp đồng của bạn an toàn hơn bằng cách rõ ràng định nghĩa các hàm không thay đổi trạng thái của hợp đồng và ngăn chặn các thay đổi trạng thái không mong muốn. Điều này có thể giúp ngăn chặn các lỗ hổng và vấn đề bảo mật có thể xảy ra từ sự thay đổi trạng thái không mong muốn.

Ngoài ra, đánh dấu một hàm là `view` cũng có thể làm cho nó dễ dàng hơn cho nhà phát triển hiểu hành vi dự định của hợp đồng và có thể giúp tăng độ dễ đọc và bảo trì của mã.

## Nếu không có từ khóa `view` thì sao?

Trong Solidity, từ khóa `view` được sử dụng để chỉ ra rằng một hàm không thay đổi bất kỳ trạng thái nào của hợp đồng. Điều này có nghĩa là hàm chỉ truy cập và trả về các giá trị từ các biến trạng thái hiện có, nhưng không thực hiện bất kỳ hành động ghi đè nào trên các biến trạng thái đó.

Nếu không sử dụng từ khóa `view`, hàm sẽ được coi là một hàm thông thường, tức là có thể thay đổi trạng thái của hợp đồng. Khi gọi một hàm thông thường, người dùng sẽ phải trả `phí` cho việc gửi giao dịch đến hợp đồng. Trong khi đó, khi gọi một hàm có từ khóa `view`, người dùng không phải trả `phí` để gửi giao dịch đến hợp đồng.

- Ví dụ, hàm sau sẽ không thay đổi trạng thái của hợp đồng và không tính `phí` khi được gọi:

```jsx
function getData() view public returns (uint x) {
    return data;
}
```

- Trong khi đó, hàm sau sẽ thay đổi trạng thái của hợp đồng và tính `phí` khi được gọi:

```sol
function setData(uint x) public {
    data = x;
}
```

## Kiểu dữ liệu thường nên dùng trong lập trình Blockchain

Nên thường dùng những kiểu dữ liệu `unsign`, nghĩa là `không âm`, ví dụ như `uint`,...

:::tip Giải thích
Vì liên quan đến tiền tệ blockchain thì sẽ không thể nào là số âm được, nên việc dùng những kiểu dữ liệu unsign sẽ giảm thiểu được nhiều rủi ro không đáng có.
:::
