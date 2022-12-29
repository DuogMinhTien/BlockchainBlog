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
