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

## Thêm function `getDonators`

```cpp
function getDonators (uint256 _id) view public returns (address[] memory, uint256[] memory) {
  return (campains[_id].donators, campains[_id].donations);
}
```

Hàm dùng để lấy danh sách các tài khoản (địa chỉ) và số tiền mà các tài khoản đó đã quyên góp cho một chiến dịch quyên góp nào đó. Hàm này có tham số đầu vào là một số nguyên dương (\_id), một trong các chiến dịch quyên góp đã được tạo trước đó. Kết quả trả về là một cặp mảng (mảng địa chỉ và mảng số nguyên dương) chứa danh sách các tài khoản và số tiền mà các tài khoản đó đã quyên góp cho chiến dịch quyên góp có id là `_id`.

Trong hàm này, biến campains là một mảng các đối tượng `Campaign`, mỗi đối tượng `Campaign` chứa thông tin về một chiến dịch quyên góp, trong đó có hai thuộc tính là donators (mảng địa chỉ) và donations (mảng số nguyên dương). Do đó, khi gọi hàm return (`campains[_id].donators`, `campains[_id].donations`), nó sẽ trả về hai mảng `donators` và `donations` tương ứng với chiến dịch quyên góp có id là `_id`.

Các thuộc tính `view` và `public` trong hàm này có nghĩa là hàm này là một hàm xem (`view`) và có thể gọi từ bên ngoài hợp đồng (`public`).

## Thêm hàm `getCampaigns`

```cpp
function getCampaigns () public view returns (Campaign [] memory) {
  Campaign[] memory allCampaigns = new Campaign[] (numberOfCampains);

  for (uint i=0; i < numberOfCampains; i++) {
    Campaign storage item = campains[i];

    allCampaigns[i] = item;
  }

  return allCampaigns;
}
```

Hàm này dùng để lấy tất cả các chiến dịch quyên góp đã được tạo trước đó. Hàm này không có tham số đầu vào và trả về một mảng (`allCampaigns`) chứa các đối tượng `Campaign` tương ứng với tất cả các chiến dịch quyên góp.

Trong hàm này, biến `campains` là một mảng các đối tượng `Campaign`, mỗi đối tượng `Campaign` chứa thông tin về một chiến dịch quyên góp. Biến `numberOfCampains` là một biến số nguyên dương chứa số lượng chiến dịch quyên góp đã được tạo trước đó.

Trong vòng lặp for, ta duyệt từng phần tử trong mảng `campains` và lưu trữ từng phần tử này vào mảng `allCampaigns`. Các phần tử trong mảng `campains` được truy cập bằng từ khóa `storage`, để chỉ rằng các phần tử này là các biến trong `storage` và có thể được ghi đè bởi các hàm khác trong hợp đồng hoặc từ bên ngoài hợp đồng.

Cuối cùng, hàm sẽ trả về mảng `allCampaigns` chứa tất cả các đối tượng `Campaign` tương ứng với tất cả các chiến dịch.

## Mở `testnet` trên Metamask và chọn mạng thử nghiệm `GoerliETH`

- Sau khi chuyển sang mạng `GoerliETH`, vào trang [goerlifaucet](https://goerlifaucet.com/) để lấy 0.2
  GoerliETH free.

- Lấy `private key` vào thêm vào file .env trong `blockchainweb3`

```
PRIVATE_KEY=...
```

## Config lại file `hardhat.config.js`

```js
/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: {
    version: "0.8.9",
    defaultNetwork: "goerli",
    networks: {
      hardhat: {},
      goerli: {
        url: "https://rpc.ankr.com/eth_goerli",
        accounts: [`0x${process.env.PRIVATE_KEY}`],
      },
    },
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
};
```

Đường link `url` trong `goerli` được lấy từ địa chỉ này [ankr.com](https://www.ankr.com/rpc/eth)

## Deploy lên `thirdweb`

Chạy lệnh `npm run deploy` để deploy lên thirdweb3, sau khi deploy xong sẽ có 1 đường link đến dashboard confirm, chỉ cần connect với ví có chuyển sang mạng `goerli` và nhấn deploy chờ xác nhận là xong.

## cd đến folder `client` và bắt đầu cài package cần thiết

```
npx thirdweb create --app
```

Phần này sẽ sử dụng framework `Vite`, blockchain `EVM` và ngôn ngữ `Javascript`

```bash title="Cài đặt react router"
npm install react-router-dom
```

---

**Phần còn lại hãy xem video từ 32:00 đến 53:00**

<iframe width="100%" height="415" src="https://www.youtube.com/embed/BDCT6TYLYdI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>

## Cài đặt tailwind

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

update file `tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./index.html", "./src/**/*.{jsx,js,tsx,ts}"],
  theme: {
    extend: {
      fontFamily: {
        epilogue: ["Epilogue", "sans-serif"],
      },
      boxShadow: {
        secondary: "10px 10px 20px rgba(2,2,2,0.25)",
      },
    },
  },
  plugins: [],
};
```

Tạo file `index.css` trong thư mục `src` và dán đoạn code dưới vào

```css
@import url("https://fonts.googleapis.com/css2?family=Epilogue:wght@400;500;600;700&display=swap");

@tailwind base;
@tailwind components;
@tailwind utilities;

.linear-gradient {
  background: linear-gradient(
    179.14deg,
    rgba(32, 18, 63, 0) -7.14%,
    #000000 87.01%
  );
}

input[type="date"]::-webkit-calendar-picker-indicator {
  cursor: pointer;
  filter: invert(0.8);
}
```

## Thêm folder `asset`

Download [tại đây](https://minhaskamal.github.io/DownGit/#/home?url=https:%2F%2Fgithub.com%2Fadrianhajdin%2Fproject_crowdfunding%2Ftree%2Fmaster%2Fclient%2Fsrc%2Fassets) và dán vào folder `src`

## Thêm thư mục `constant`

Tạo file `index.js` và dán đoạn code dưới vào

```js
import {
  createCampaign,
  dashboard,
  logout,
  payment,
  profile,
  withdraw,
} from "../assets";

export const navlinks = [
  {
    name: "dashboard",
    imgUrl: dashboard,
    link: "/",
  },
  {
    name: "campaign",
    imgUrl: createCampaign,
    link: "/create-campaign",
  },
  {
    name: "payment",
    imgUrl: payment,
    link: "/",
    disabled: true,
  },
  {
    name: "withdraw",
    imgUrl: withdraw,
    link: "/",
    disabled: true,
  },
  {
    name: "profile",
    imgUrl: profile,
    link: "/profile",
  },
  {
    name: "logout",
    imgUrl: logout,
    link: "/",
    disabled: true,
  },
];
```

## Thêm thư mục `utils`

Tạo file `index.js` và dán đoạn code dưới vào

```js
export const daysLeft = deadline => {
  const difference = new Date(deadline).getTime() - Date.now();
  const remainingDays = difference / (1000 * 3600 * 24);

  return remainingDays.toFixed(0);
};

export const calculateBarPercentage = (goal, raisedAmount) => {
  const percentage = Math.round((raisedAmount * 100) / goal);

  return percentage;
};

export const checkIfImage = (url, callback) => {
  const img = new Image();
  img.src = url;

  if (img.complete) callback(true);

  img.onload = () => callback(true);
  img.onerror = () => callback(false);
};
```

## Thêm 1 số file vào folder `pages` nằm trong `src`

- Home.jsx

- CreateCampaign.jsx

- CampaignDetails.jsx

- Details.jsx

Gõ `rafce` và `tab` sau khi tạo mỗi file để tạo code mẫu trước.

Thêm file `index.js` và xuất những file này ra

```js
export { default as Home } from "./Home";
export { default as Profile } from "./Profile";
export { default as CreateCampaign } from "./CreateCampaign";
export { default as CampaignDetails } from "./CampaignDetails";
```

## Thêm 1 số file vào folder `components` nằm trong `src`

- Sidebar.jsx

- Navbar.jsx

Gõ `rafce` và `tab` sau khi tạo mỗi file để tạo code mẫu trước.

Thêm file `index.js` và xuất những file này ra

```js
export { default as Sidebar } from "./Sidebar";
export { default as Navbar } from "./Navbar";
```

## Code file `App.js` trong phần này

```jsx
import React from "react";
import { Route, Routes } from "react-router-dom";
import { Navbar, Sidebar } from "./components";

import { Home, CreateCampaign, CampaignDetails, Profile } from "./pages";

const App = () => {
  return (
    <div className="relative sm:-8 p-4 bg-[#13131a] min-h-screen flex flex-row">
      <div className="sm:flex hidden mr-10 relative">
        <Sidebar />
      </div>

      <div className="flex-1 max-sm:w-full max-w-[1280px] mx-auto sm:pr-5">
        <Navbar />
        <Routes>
          <Route path="/" element={<Home />} />
        </Routes>
      </div>
    </div>
  );
};

export default App;
```
