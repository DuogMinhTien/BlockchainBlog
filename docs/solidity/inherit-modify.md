---
sidebar_position: 4
---

# Tính kế thừa, modifier

## `modifier` là gì?

Trong Solidity, modifier là một kiểu định nghĩa hàm đặc biệt mà bạn có thể sử dụng để thêm các điều kiện kiểm tra trước khi thực hiện hàm hoặc khối lệnh bên trong hàm. Nó cho phép bạn xác định các điều kiện mà phải được kiểm tra trước khi hàm được gọi hoặc khối lệnh bên trong hàm được thực thi.

Ví dụ, bạn có thể sử dụng modifier để kiểm tra xem người gọi hàm có quyền hợp pháp để gọi hàm đó hay không. Nếu không có quyền hợp pháp, hàm sẽ không được thực thi.

Để định nghĩa một modifier, bạn sử dụng từ khóa `modifier` và đặt tên cho modifier sau đó. Bên trong ngoặc nhọn, bạn định nghĩa các điều kiện kiểm tra và các khối lệnh mà sẽ được thực hiện nếu điều kiện đó được thỏa mãn.

```cpp title="Ví dụ"
modifier onlyOwner {
    require(msg.sender == owner);
    _;
}

function transferOwnership(address newOwner) public onlyOwner {
    owner = newOwner;
}
```

Trong ví dụ trên, modifier `onlyOwner` kiểm tra xem người gọi hàm có phải là chủ sở hữu hiện tại của hợp đồng hay không?

Bạn có thể sử dụng modifier bằng cách đặt tên của nó trong danh sách các từ khóa trước tên hàm hoặc phương thức. Nếu điều kiện được định nghĩa trong modifier không được thỏa mãn, hàm hoặc phương thức sẽ không được thực thi.

Bạn cũng có thể sử dụng nhiều modifier trong một hàm hoặc phương thức bằng cách đặt nhiều tên modifier trong danh sách các từ khóa trước tên hàm hoặc phương thức. Các modifier sẽ được thực thi theo thứ tự từ trái sang phải.

```cpp title="Ví dụ"
modifier onlyOwner {
    require(msg.sender == owner);
    _;
}

modifier onlyIfNotPaused {
    require(!paused);
    _;
}

function transferOwnership(address newOwner) public onlyOwner onlyIfNotPaused {
    owner = newOwner;
}
```

Trong ví dụ trên, hàm `transferOwnership` chỉ sẽ được thực thi nếu người gọi hàm là chủ sở hữu hiện tại và hợp đồng không bị tạm dừng.

Modifier cũng có thể có tham số. Để truyền tham số cho modifier, bạn có thể sử dụng cú pháp tương tự như trong hàm bình thường, bao gồm tên và kiểu dữ liệu của tham số. Khi sử dụng modifier, bạn cần truyền các giá trị tham số tương ứng theo cú pháp tương tự như khi gọi một hàm hoặc phương thức.

```cpp title="Ví dụ"
modifier onlyIf(uint _value) {
    require(value == _value);
    _;
}

function setValue(uint _value) public onlyIf(_value) {
    value = _value;
}
```

Trong ví dụ trên, modifier `onlyIf` có một tham số `_value` có kiểu `uint`. Khi sử dụng modifier trong hàm `setValue`, bạn cần truyền giá trị tham số tương ứng, trong trường hợp này là giá trị tham số `_value` của hàm `setValue`. Nếu giá trị của biến value bằng giá trị tham số `_value`, khối lệnh `value = _value`; sẽ được thực thi. Nếu không, khối lệnh sẽ không được thực thi.

Lưu ý rằng bạn không thể truyền tham số cho `_;` trong modifier. `_;` là một cú pháp đặc biệt được sử dụng để cho phép các khối lệnh bên trong hàm hoặc phương thức được thực thi sau khi kiểm tra các điều kiện trong modifier.
