---
sidebar_position: 3
---

# Kiểu dữ liệu

## Code mẫu

```cpp
pragma solidity >= 0.7.0 < 0.9.0;

contract datatypes {
    bool myBoolean = true; //false
    uint myUnsignedInteger = 10; // >= 0
    int myInteger = -10;
    string myString = "MyString";
    address myAddress = msg.sender;
}
```

## `memory` trong Solidity dùng để làm gì?

Trong ngôn ngữ lập trình Solidity, từ khóa `memory` được sử dụng để chỉ ra rằng một biến hoặc giá trị nào đó được lưu trữ trong bộ nhớ (memory) của hợp đồng. Bộ nhớ là một vùng dữ liệu tạm thời được sử dụng trong quá trình thực thi của hợp đồng, nó không lưu trữ lâu dài trên blockchain.

Ví dụ về 1 hàm có sử dụng `memory`:

```cpp
function updateUser(uint _index, string memory _name, uint _age, address _wallet) public {
    User memory user = users[_index];
    user.name = _name;
    user.age = _age;
    user.wallet = _wallet;
    users[_index] = user;
}
```

Trong hàm `updateUser`, biến `user` được khai báo với kiểu dữ liệu `struct User` và được lưu trữ trong bộ nhớ (`User memory user`). Biến này được gán giá trị bằng một phần tử `struct User` trong mảng `users` tại vị trí được truyền vào dưới dạng tham số `_index`. Sau đó, các giá trị mới cho các trường `name`, `age`, và `wallet` được gán cho biến `user` từ các tham số truyền vào. Cuối cùng, biến `user` được gán trở lại mảng `users` tại vị trí đó.

Khi sử dụng các biến lưu trữ trong bộ nhớ (memory variables) trong ngôn ngữ lập trình Solidity, có một số điều cần lưu ý:

- Các biến lưu trữ trong bộ nhớ chỉ tồn tại trong quá trình thực thi của hợp đồng. Khi hợp đồng kết thúc, các biến này sẽ bị xóa khỏi bộ nhớ.

- Các biến lưu trữ trong bộ nhớ không được lưu trữ trên blockchain và không thể được truy cập từ bên ngoài hợp đồng. Nó chỉ có thể được truy cập từ trong hợp đồng.

- Khi gán giá trị cho một biến lưu trữ trong bộ nhớ từ một biến khác, bạn phải sử dụng toán tử gán (`=`) thay vì toán tử so sánh bằng (`==`). Ví dụ, để gán giá trị của biến `a` cho biến `b`, bạn phải viết `b = a` thay vì `b == a`.

- Các biến lưu trữ trong bộ nhớ có thể được sử dụng để lưu trữ các giá trị tạm thời trong quá trình thực thi của hợp đồng. Ví dụ, bạn có thể sử dụng một biến lưu trữ trong bộ nhớ để lưu trữ tổng số Ether đã nhận được từ các giao dịch trong quá trình thực thi của hợp đồng, sau đó sử dụng giá trị của biến này để tính toán kết quả cuối cùng và trả về kết quả cho người gọi hợp đồng.

- Các biến lưu trữ trong bộ nhớ có thể được sử dụng để lưu trữ các giá trị được truyền vào từ người gọi hợp đồng. Ví dụ, bạn có thể sử dụng một biến lưu trữ trong bộ nhớ để lưu trữ số lượng Ether mà người gọi muốn chuyển cho hợp đồng, sau đó sử dụng giá trị của biến này để thực hiện giao dịch chuyển tiền.

- Các biến lưu trữ trong bộ nhớ có thể được sử dụng để lưu trữ các giá trị được trả về từ hợp đồng khác. Ví dụ, bạn có thể sử dụng một biến lưu trữ trong bộ nhớ để lưu trữ kết quả trả về từ hợp đồng khác, sau đó sử dụng giá trị của biến này để thực hiện các tác vụ khác trong hợp đồng hiện tại.

- Các biến lưu trữ trong bộ nhớ có thể được sử dụng để lưu trữ các giá trị tạm thời trong quá trình gọi các hàm đệ quy (recursive functions). Ví dụ, bạn có thể sử dụng một biến lưu trữ trong bộ nhớ để lưu trữ giá trị tạm thời trong quá trình gọi đệ quy của hàm, sau đó sử dụng giá trị của biến này để kiểm tra điều kiện dừng đệ quy và trả về kết quả cho người gọi hàm.

## `Array` trong Solidity

- Khai báo 1 array

```cpp
uint[] public myCoin;
```

- Thêm 1 phần tử vào array

```cpp
myCoin.push (yourCoin);
```

## Kiểu dữ liệu `address` là gì?

Trong ngôn ngữ lập trình Solidity, kiểu dữ liệu `address` được sử dụng để biểu diễn một địa chỉ Ethereum. Một địa chỉ Ethereum là một chuỗi ký tự duy nhất định danh một tài khoản Ethereum cụ thể.

Đây là một ví dụ về cách sử dụng kiểu dữ liệu `address` trong một hợp đồng Solidity:

```cpp
pragma solidity ^0.7.0;

contract MyContract {
    address public owner;

    constructor() public {
        owner = msg.sender;
    }

    function transferOwnership(address newOwner) public {
        require(msg.sender == owner, "Only the owner can transfer ownership.");
        owner = newOwner;
    }
}
```

Trong ví dụ này, biến `owner` được khai báo với kiểu dữ liệu `address` và được sử dụng để lưu trữ địa chỉ Ethereum của chủ sở hữu của hợp đồng. Hàm `transferOwnership` cho phép chủ sở hữu chuyển quyền sở hữu của hợp đồng sang một địa chỉ Ethereum mới bằng cách cập nhật giá trị của biến `owner`.

Kiểu dữ liệu `address` trong Solidity có một số hàm tích hợp sẵn mà có thể được sử dụng để tương tác với các địa chỉ Ethereum, như hàm `transfer` để gửi Ether đến một địa chỉ được cho trước, hoặc hàm `balance` để lấy số dư của một địa chỉ trong Ether. Bạn có thể tìm thêm thông tin về kiểu dữ liệu `address` và các hàm tích hợp của nó trong tài liệu Solidity.

## Kiểu dữ liệu `struct` trong Solidity

Trong ngôn ngữ lập trình Solidity, `struct` là một từ khóa dùng để khai báo một kiểu dữ liệu tùy biến, gồm nhiều trường dữ liệu có các kiểu dữ liệu khác nhau. Một kiểu dữ liệu `struct` có thể được sử dụng như một biến để lưu trữ nhiều giá trị cùng lúc, hoặc như một tham số hoặc kết quả trả về của một hàm.

Ví dụ, bạn có thể khai báo một kiểu dữ liệu `struct` để lưu trữ thông tin về một người dùng như sau:

```cpp
pragma solidity ^0.7.0;

struct User {
    string name;
    uint age;
    address wallet;
}
```

Trong ví dụ này, kiểu dữ liệu `struct User` có ba trường: `name` là một chuỗi ký tự (`string`), `age` là một số nguyên không âm (`uint`), và `wallet` là một địa chỉ Ethereum (`address`). Bạn có thể tạo một biến `struct User` và gán giá trị cho các trường của nó như sau:

```cpp
User alice;
alice.name = "Alice";
alice.age = 30;
alice.wallet = 0x1234567890abcdef;
```

Bạn cũng có thể sử dụng một kiểu dữ liệu `struct` như một tham số hoặc kết quả trả về của một hàm, ví dụ:

```cpp
function createUser(string memory _name, uint _age, address _wallet) public returns (User memory) {
    User memory newUser;
    newUser.name = _name;
    newUser.age = _age;
    newUser.wallet = _wallet;
    return newUser;
}
```

Trong hàm `createUser`, kiểu dữ liệu `struct User` được sử dụng như một kết quả trả về (`returns (User memory)`). Hàm này nhận ba tham số: `_name` là một chuỗi ký tự, `_age` là một số nguyên không âm, và `_wallet` là một địa chỉ Ethereum. Nó tạo ra một biến `struct User` mới và gán các giá trị cho các trường của nó từ các tham số truyền vào, sau đó trả về biến này.

Bạn có thể gọi hàm `createUser` và lưu kết quả trả về vào một biến `struct User` khác như sau:

```cpp
User memory bob = createUser("Bob", 25, 0x0987654321fedcba);
```

Bây giờ, biến `bob` có các giá trị sau: `bob.name` là "Bob", `bob.age` là 25, và `bob.wallet` là địa chỉ Ethereum 0x0987654321fedcba.

## Kiểu dữ liệu `enum`?

Trong ngôn ngữ lập trình Solidity, `enum` là một kiểu dữ liệu đặc biệt cho phép bạn khai báo một tập hợp các giá trị có tên (named values). Mỗi giá trị trong tập hợp đó được gắn với một giá trị số nguyên duy nhất.

Ví dụ, bạn có thể khai báo một enum các màu sắc như sau:

```cpp
enum Colors {
    Red,
    Green,
    Blue
}
```

Trong ví dụ trên, `Colors` là tên của `enum`, và `Red`, `Green`, `Blue` là các giá trị trong tập hợp. Mỗi giá trị sẽ được gắn với một giá trị số nguyên duy nhất từ 0 trở lên, với `Red` được gắn với giá trị 0, `Green` được gắn với giá trị 1, và `Blue` được gắn với giá trị 2.

Để sử dụng một `enum` trong một hợp đồng Solidity, bạn cần khai báo một biến có kiểu dữ liệu là `enum`. Ví dụ, bạn có thể khai báo một biến `favoriteColor` có kiểu dữ liệu là `Colors` như sau:

```cpp
Colors favoriteColor;
```

Sau đó, bạn có thể gán giá trị cho biến này bằng cách sử dụng tên của giá trị trong `enum`, ví dụ:

```cpp
favoriteColor = Colors.Red;
```

Bạn cũng có thể sử dụng các giá trị của `enum` trong các điều kiện điều kiện và các hàm khác. Ví dụ, bạn có thể sử dụng một câu lệnh `if` để kiểm tra xem biến `favoriteColor` có bằng với giá trị `Colors.Red` hay không:

```cpp
if (favoriteColor == Colors.Red) {
    // Do something
}
```

Bạn cũng có thể truy cập giá trị số nguyên tương ứng của mỗi giá trị trong `enum` bằng cách sử dụng toán tử `.`, ví dụ:

```cpp
uint8 value = uint8(favoriteColor);
```

Trong ví dụ trên, biến `value` sẽ được gán giá trị 0 nếu `favoriteColor` bằng `Colors.Red`, giá trị 1 nếu `favoriteColor` bằng `Colors.Green`, và giá trị 2 nếu `favoriteColor` bằng `Colors.Blue`.

Bạn cũng có thể khai báo một `enum` với các giá trị số nguyên tùy ý bằng cách định nghĩa giá trị số nguyên cho mỗi giá trị trong tập hợp. Ví dụ, bạn có thể khai báo một `enum` các loại vé như sau:

```cpp
enum TicketTypes {
    Economy = 10,
    Business = 20,
    FirstClass = 30
}
```

Trong ví dụ trên, `Economy` sẽ được gắn với giá trị số nguyên 10, `Business` sẽ được gắn với giá trị số nguyên 20, và `FirstClass` sẽ được gắn với giá trị số nguyên 30.

Bạn cũng có thể khai báo một `enum` với các giá trị kí tự bằng cách sử dụng các dấu ngoặc kép `""` để bao quanh các giá trị kí tự. Ví dụ, bạn có thể khai báo một `enum` các loại đồ uống như sau:

```cpp
enum DrinkTypes {
    "Coffee",
    "Tea",
    "Water"
}
```

Trong ví dụ trên, `"Coffee"` sẽ được gắn với giá trị số nguyên 0, `"Tea"` sẽ được gắn với giá trị số nguyên 1, và `"Water"` sẽ được gắn với giá trị số nguyên 2.

Để lấy giá trị kí tự từ một biến có kiểu dữ liệu là `enum`, bạn có thể sử dụng hàm `string` của Solidity. Hàm này trả về giá trị kí tự tương ứng với giá trị của biến `enum`.

```cpp title="Ví dụ"
string favoriteDrinkString = string(favoriteDrink);
```

Trong ví dụ trên, biến `favoriteDrinkString` sẽ được gán giá trị "Coffee" nếu `favoriteDrink` bằng `DrinkTypes.Coffee`, giá trị "Tea" nếu `favoriteDrink` bằng `DrinkTypes.Tea`, và giá trị "Water" nếu `favoriteDrink` bằng `DrinkTypes.Water`.

Lưu ý rằng hàm `string` chỉ hoạt động với các `enum` có các giá trị là các chuỗi kí tự. Nó không hoạt động với các `enum` có các giá trị là các số nguyên.
