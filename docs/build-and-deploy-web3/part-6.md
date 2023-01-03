---
sidebar_position: 6
---

# 6. Web3 Context

## Tạo file `index.jsx` vào folder `context`

Thêm context `StateContext`

```jsx
import React, { useContext } from "react";
import {
  useAddress,
  useContract,
  useMetamask,
  useContractWrite,
} from "@thirdweb-dev/react";
import { ethers } from "ethers";

const StateContext = React.createContext();

export const StateContextProvider = ({ children }) => {
  const { contract } = useContract(
    "0x53e17DCDAC71bE52E7187512Aaf5f452d14fe736"
  );

  const {
    mutateAsync: createCampaign,
    isLoading,
    error,
  } = useContractWrite(contract, "createCampaign");

  const address = useAddress();

  const connect = useMetamask();

  const publishCampaign = async form => {
    try {
      const data = await createCampaign([
        address, //owner
        form.title, //title
        form.description, //description
        form.target, //target
        new Date(form.deadline).getTime(), //deadline,
        form.image,
      ]);
      console.log("contract call success: ", data);
    } catch (err) {
      console.log("contract call failure: ", err);
    }
  };

  return (
    <StateContext.Provider
      value={{
        address,
        contract,
        connect,
        createCampaign: publishCampaign,
      }}>
      {children}
    </StateContext.Provider>
  );
};

export const useStateContext = () => {
  return useContext(StateContext);
};
```

- Địa chỉ contract: `0x53e17DCDAC71bE52E7187512Aaf5f452d14fe736` lấy từ dashboard của contract đấy trong `thirdweb`

![Nơi lấy địa chỉ contract](/img/contract_address_dashboard.png)

- `useAddress` là một hook sử dụng để lấy địa chỉ của người dùng hiện tại.

- `useContract` là một hook sử dụng để lấy thông tin về một hợp đồng đã được kết nối với dApp (ứng dụng điện toán đám mây).

- `useMetamask` là một hook sử dụng để kiểm tra xem người dùng hiện tại có đang sử dụng trình duyệt Metamask hay không.

- `useContractWrite` là một hook sử dụng để gửi các yêu cầu ghi vào hợp đồng.

## Bổ sung `StateContextProvider` vào file `main.jsx`

```jsx title="import StateContextProvider"
import { StateContextProvider } from "./context";
```

```jsx title="Bọc component App lại bằng StateContextProvider"
<StateContextProvider>
  <App />
</StateContextProvider>
```

```jsx title="Code full"
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter as Router } from "react-router-dom";
import { ChainId, ThirdwebProvider } from "@thirdweb-dev/react";

import { StateContextProvider } from "./context";

import App from "./App";
import "./index.css";

const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(
  <ThirdwebProvider desiredChainId={ChainId.Goerli}>
    <Router>
      <StateContextProvider>
        <App />
      </StateContextProvider>
    </Router>
  </ThirdwebProvider>
);
```

## Update lại file `Navbar.jsx`

```jsx title="import useStateContext"
import { useStateContext } from "../context";
```

```jsx title="Sử dụng useStateContext"
const { connect, address } = useStateContext();
```

```jsx title="Thay function connect tại CustomButton phần Connect"
<CustomButton
  btnType="button"
  title={address ? "Create a campaign" : "Connect"}
  styles={address ? "bg-[#1dc071]" : "bg-[#8c6dfd]"}
  handleClick={() => {
    if (address) {
      navigate("create-campaign");
    } else {
      connect();
    }
  }}
/>
```

_Có 2 chỗ_

## Bổ sung file `CreateCampaign.jsx`

```jsx title="import những phần cần thiết"
import { ethers, utils } from "ethers";

import { useStateContext } from "../context";
```

```jsx title="Sử dụng useStateContext lấy createCampaign"
const { createCampaign } = useStateContext();
```

```jsx title="Hoàn thiện hàm handleSubmit ()"
const handleSubmit = async e => {
  e.preventDefault();

  checkIfImage(form.image, async exists => {
    if (exists) {
      setIsLoading(true);
      await createCampaign({
        ...form,
        target: ethers.utils.parseUnits(form.target, 18),
      });
      setIsLoading(false);
      navigate("/");
    } else {
      alert("Provide valid image URL");
      setForm({ ...form, image: "" });
    }
  });
};
```

`ethers.utils.parseUnits` là một hàm của thư viện Ethers.js dùng để chuyển đổi giá trị có đơn vị cho trước sang đơn vị cơ bản.

Ví dụ:

- Giá trị `"1.5"` với đơn vị `"ether"` (18 định dạng số) sẽ được chuyển đổi thành `"1500000000000000000"`.

- Giá trị `"0.001"` với đơn vị `"gwei"` (9 định dạng số) sẽ được chuyển đổi thành `"1000000"`.

Cú pháp của hàm như sau:

```jsx
ethers.utils.parseUnits(value: string, decimals: number): string
```

Trong đó:

- `value`: Giá trị cần chuyển đổi.

- `decimals`: Số lượng định dạng số của đơn vị. Ví dụ: 18 định dạng số cho `ether`, 9 định dạng số cho `gwei`.

Hàm trả về một chuỗi là giá trị đã được chuyển đổi sang đơn vị cơ bản.

**Vì sao cần đến hàm này?**

Trong các giao dịch trên mạng Ethereum, các giá trị được giao dịch thường được xác định bằng đơn vị cơ bản, ví dụ: `wei`, `gwei`, `ether`. Tuy nhiên, người dùng thường không muốn phải nhập giá trị bằng đơn vị cơ bản mà họ thường muốn nhập bằng đơn vị hợp lý hơn, ví dụ: `ether`, `finney`, `szabo`.

Vì vậy, hàm `ethers.utils.parseUnits` được sử dụng để chuyển đổi giá trị với đơn vị cho trước sang đơn vị cơ bản, để có thể sử dụng trong các giao dịch trên mạng Ethereum.

Ví dụ, nếu người dùng nhập giá trị `"1.5"` với đơn vị `"ether"` (18 định dạng số), hàm `ethers.utils.parseUnits` sẽ chuyển đổi giá trị này sang đơn vị cơ bản là `"1500000000000000000"`. Khi đó, giá trị này có thể được sử dụng trong các giao dịch trên mạng Ethereum.

**Danh sách định dạng số được hỗ trợ**

| Đơn vị         | Định dạng số |
| -------------- | ------------ |
| wei            | 0            |
| kwei / ada     | 3            |
| mwei / babbage | 6            |
| gwei / shannon | 9            |
| szabo          | 12           |
| finney         | 15           |
| ether          | 18           |
