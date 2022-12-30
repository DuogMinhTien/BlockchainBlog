---
sidebar_position: 5
---

# Thời gian

Trong Solidity, `time` là một biến toàn cục đặc biệt mà bạn có thể sử dụng để lấy thời gian hiện tại theo đồng hồ Unix. `time` là một biến kiểu `uint` và là số giây tính từ ngày 1 tháng 1 năm 1970.

Bạn có thể sử dụng `time` để lấy thời gian hiện tại trong hợp đồng của bạn và sử dụng nó để thực hiện các tác vụ liên quan đến thời gian, chẳng hạn như kiểm tra xem một hợp đồng có hết hạn hay không hoặc thực hiện một tác vụ sau một khoảng thời gian nhất định.

```cpp title="Ví dụ"
function isExpired() public view returns (bool) {
    return time > expirationTime;
}
```

Trong ví dụ trên, hàm `isExpired` kiểm tra xem biến `expirationTime` có lớn hơn thời gian hiện tại hay không. Nếu lớn hơn, hàm sẽ trả về true, nếu không sẽ trả về false.

Lưu ý rằng `time` chỉ có thể được sử dụng để đọc giá trị, bạn không thể gán giá trị mới cho nó.

### `block.timestamp` trong Solidity

Trong Solidity, `block.timestamp` là một biến toàn cục đặc biệt mà bạn có thể sử dụng để lấy thời gian hiện tại của khối hiện tại theo đồng hồ Unix. `block.timestamp` là một biến kiểu `uint` và là số giây tính từ ngày 1 tháng 1 năm 1970.

Bạn có thể sử dụng `block.timestamp` để lấy thời gian hiện tại của khối hiện tại trong hợp đồng của bạn và sử dụng nó để thực hiện các tác vụ liên quan đến thời gian, chẳng hạn như kiểm tra xem một hợp đồng có hết hạn hay không hoặc thực hiện một tác vụ sau một khoảng thời gian nhất định.

```cpp title="Ví dụ"
function isExpired() public view returns (bool) {
    return block.timestamp > expirationTime;
}
```

Trong ví dụ trên, hàm `isExpired` kiểm tra xem thời gian hiện tại của khối hiện tại có lớn hơn biến `expirationTime` hay không. Nếu lớn hơn, hàm sẽ trả về `true`, nếu không sẽ trả về `false`.

Lưu ý rằng `block.timestamp` chỉ có thể được sử dụng để đọc giá trị, bạn không thể gán giá trị mới cho nó.

Bạn cũng nên lưu ý rằng `block.timestamp` có thể bị sai sót đôi chút vì nó phụ thuộc vào người gửi giao dịch. Do đó, nếu bạn cần sử dụng thời gian chính xác trong hợp đồng của mình, bạn có thể muốn sử dụng một kỹ thuật khác, chẳng hạn như sử dụng một hợp đồng thời gian hoặc sử dụng một dịch vụ thời gian bên ngoài để lấy thời gian chính xác hơn.

Để sử dụng một hợp đồng thời gian, bạn có thể khai báo một hợp đồng con và gọi hàm trong hợp đồng đó để lấy thời gian hiện tại. Ví dụ:

```cpp
contract TimeContract {
    function getCurrentTime() public view returns (uint) {
        return now;
    }
}

contract MyContract {
    TimeContract timeContract = TimeContract(0x12345...);

    function isExpired() public view returns (bool) {
        return timeContract.getCurrentTime() > expirationTime;
    }
}
```

Trong ví dụ trên, hợp đồng `MyContract` khai báo một hợp đồng con `TimeContract` và gọi hàm `getCurrentTime` trong hợp đồng đó để lấy thời gian hiện tại.

Để sử dụng một dịch vụ thời gian bên ngoài, bạn có thể sử dụng một hợp đồng Oraclize hoặc một hợp đồng khác được tích hợp với một dịch vụ thời gian bên ngoài.

```cpp title="Ví dụ với Oraclize"
import "https://github.com/oraclize/ethereum-api/oraclizeAPI.sol";

contract MyContract is usingOraclize {
    uint public currentTime;

    function updateTime() public {
        oraclize_query(60, "URL", "json(https://time.api.com).time");
    }

    function __callback(bytes32 _queryId, string _result) public {
        currentTime = parseInt(_result);
    }
}
```

Trong ví dụ trên, hợp đồng `MyContract` sử dụng Oraclize để lấy thời gian hiện tại từ một dịch vụ thời gian bên ngoài. Bạn có thể gọi hàm `updateTime` để yêu cầu Oraclize lấy thời gian hiện tại và trả về cho hợp đồng thông qua hàm `__callback`. Giá trị thời gian hiện tại sẽ được lưu trữ trong biến `currentTime`.

Lưu ý rằng bạn cần có một tài khoản Oraclize để sử dụng tính năng này và cần trả phí cho mỗi lần gọi Oraclize. Bạn cũng cần phải đảm bảo rằng dịch vụ thời gian bên ngoài mà bạn sử dụng có được cung cấp bởi một nguồn tin cậy và đáng tin cậy. Bạn còn cần đảm bảo rằng hợp đồng của bạn có thể đáp ứng được các lỗi có thể xảy ra trong quá trình gọi Oraclize, chẳng hạn như lỗi mạng hoặc lỗi trong dịch vụ thời gian bên ngoài,...
