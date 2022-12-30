---
sidebar_position: 2
---

# 2. Smart Contract

Vào file `Contract.sol` trong thư mục `contracts` và bắt đầu code, đổi tên file thành `CrowdFunding`, đổi luôn cả tên class contract trong file.

## Thêm `struct` vào contract:

```cpp
struct Campaign {
    address owner;
    string title;
    string description;
    uint256 target;
    uint256 deadline; //deadline
    uint256 amountCollected; //So tien thu duoc
    string image; //<- url hình ảnh
    address[] donators;
    uint256[] donations;
}
```

- **owner**: địa chỉ của người tạo chiến dịch.
- **title**: tiêu đề của chiến dịch.
- **description**: mô tả chi tiết của chiến dịch.
- **target**: mục tiêu số tiền cần thu được trong chiến dịch.
- **deadline**: ngày kết thúc của chiến dịch.
- **amountCollected**: số tiền đã thu được trong chiến dịch.
- **image**: URL của hình ảnh liên quan đến chiến dịch.
- **donators**: mảng các địa chỉ của người đã quyên góp.
- **donations**: mảng các số tiền đã quyên góp.

## Thêm mảng campains bằng `mapping`

Đồng thời thêm 1 biến để đếm

```cpp
mapping (uint256 => Campaign) public campains;
uint256 public numberOfCampains = 0;
```

## Thêm function `createCampaign`

```cpp
function createCampaign (address _owner, string memory _title, string memory _description, uint256 _target, uint256 _deadline, string memory _image) public returns (uint256){
    Campaign storage campain = campains[numberOfCampains]; //dòng 1

    require(campaign.deadline < _deadline, "The deadline should be a date in the future.");

    campaign.owner = _owner;
    campaign.title = _title;
    campaign.description = _description;
    campaign.target = _target;
    campaign.deadline = _deadline;
    campaign.amountCollected = 0;
    campaign.image = _image;

    numberOfCampains ++;

    return numberOfCampains - 1;
}
```

- Giải thích **dòng 1**

  - Dòng mã trên đang khai báo một biến có tên là `campain` và kiểu dữ liệu là `Campaign storage`. Biến `campain` sẽ được khởi tạo với giá trị là một phần tử trong mảng `campains` có chỉ số là `numberOfCampains`.

    - `Campaign` là tên của một lớp (class) hoặc cấu trúc dữ liệu được khai báo trước đó trong mã.

    - `storage` là một từ khóa trong Solidity, nó được sử dụng để chỉ rằng biến `campain` sẽ được lưu trữ trên bộ nhớ của hệ thống (thay vì trên bộ nhớ đệm hay stack).

    - `campains` là một mảng (array) các đối tượng `Campaign`.

    - `numberOfCampains` là một biến có kiểu số nguyên (integer) chứa chỉ số của phần tử trong mảng `campains` mà muốn lấy ra.

  - Ví dụ, nếu `numberOfCampains` có giá trị là 2 và `campains` là mảng gồm 3 phần tử, thì biến `campain` sẽ được khởi tạo với giá trị là phần tử thứ 3 trong mảng `campains`.

- Hàm trả về giá trị là id của `campaign` được tạo ra.

## Thêm function `donateToCampaign`

```cpp
function donateToCampaign (uint256 _id) public payable {
    uint256 amount = msg.value;

    Campaign storage campaign = campains[_id];

    campaign.donators.push (msg.sender);
    campaign.donations.push (amount);

    (bool sent,) = payable(campaign.owner).call{value: amount} (""); //*

    if (sent) {
        campaign.amountCollected += amount;
    }
}
```

- Giải thích dòng **(\*)**

  Đây là một khai báo hàm trong ngôn ngữ Solidity, dùng để gọi hàm payable của đối tượng `campaign.owner` với tham số `value` là `amount` và truyền một chuỗi rỗng vào hàm này.

  Trong khai báo hàm này, `(bool sent,)` là một tuple được sử dụng để khai báo biến `sent` là kiểu boolean. Sau khi gọi hàm `payable`, kết quả trả về sẽ được gán cho biến `sent`.

  Cấu trúc của khai báo hàm trong Solidity giống như các ngôn ngữ khác, bao gồm tên hàm, danh sách tham số và nội dung của hàm. Trong trường hợp này, hàm `payable` có thể nhận một tham số `value`, biểu thị số tiền được chuyển đến hàm này khi được gọi. Hàm `call` được sử dụng để gọi hàm khác trong Solidity.

  Ví dụ, nếu bạn muốn gọi hàm `payable` của đối tượng `campaign.owner` với `amount` là 100 wei và truyền một chuỗi rỗng vào hàm này, bạn có thể viết như sau:

  ```cpp
  (bool sent,) = payable(campaign.owner).call{value: 100} ("");
  ```

  Để hiểu rõ hơn về cách sử dụng hàm `call` trong Solidity, bạn có thể tham khảo tài liệu của ngôn ngữ này [tại đây](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#calling-functions)
