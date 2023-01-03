---
sidebar_position: 5
---

# 5. Create Campaign

## Thêm component `FormField`

```jsx
import React from "react";

const FormField = ({
  labelName,
  placeholder,
  inputType,
  isTextArea,
  value,
  handleChange,
  styles,
}) => {
  return (
    <label className={`flex-1 w-full flex flex-col ${styles}`}>
      {labelName && (
        <span className="font-epilogue font-medium text-[14px] leading-[22px] text-[#808191] mb-[10px]">
          {labelName}
        </span>
      )}
      {isTextArea ? (
        <textarea
          required
          value={value}
          onChange={handleChange}
          rows={10}
          placeholder={placeholder}
          className="py-[15px] sm:px-[25px] px-[15px] outline-none border-[1px] border-[#3a3a43] bg-transparent font-epilogue text-white text-[14px] placeholder:text-[#4b5264] rounded-[10px] sm:min-w-[300px] "
        />
      ) : (
        <input
          required
          value={value}
          onChange={handleChange}
          type={inputType}
          step="0.1"
          placeholder={placeholder}
          className="py-[15px] sm:px-[25px] px-[15px] outline-none border-[1px] border-[#3a3a43] bg-transparent font-epilogue text-white text-[14px] placeholder:text-[#4b5264] rounded-[10px] sm:min-w-[300px]"
        />
      )}
    </label>
  );
};

export default FormField;
```

## Hoàn thiện `CreateCampaign` trong folder `page`

```jsx
import React from "react";
import { useNavigate } from "react-router-dom";

import { ethers } from "ethers";

import { money } from "../assets";
import { CustomButton, FormField } from "../components";
import { checkIfImage } from "../utils";

const CreateCampaign = () => {
  const navigate = useNavigate();

  const [isLoading, setIsLoading] = React.useState(false);

  const [form, setForm] = React.useState({
    name: "",
    title: "",
    description: "",
    target: "",
    deadline: "",
    image: "",
  });

  const handleFormChange = (fieldName, e) => {
    setForm({ ...form, [fieldName]: e.target.value });
  };

  const handleSubmit = e => {
    e.preventDefault();

    console.log(form);
  };

  return (
    <div className="bg-[#1c1c24] flex justify-center items-center flex-col rounded-[10px] sm:p-10 p-4">
      {isLoading && "Loader..."}
      <div className="flex justify-center items-center p-[16px] sm:min-w-[380px] bg-[#3a3a43] rounded-[10px]">
        <h1 className="font-epilogue font-bold sm:text-[25px] text-[18px] leading-[38px] text-white">
          Start a Campaign
        </h1>
      </div>

      <form
        onSubmit={handleSubmit}
        className="w-full mt-[65px] flex flex-col gap-[30px] ">
        <div className="flex flex-wrap gap-[40px]">
          <FormField
            labelName="Your Name *"
            placeholder="John Doe"
            inputType="text"
            value={form.name}
            handleChange={e => {
              handleFormChange("name", e);
            }}
          />
          <FormField
            labelName="Campaign Title *"
            placeholder="Write a title"
            inputType="text"
            value={form.title}
            handleChange={e => {
              handleFormChange("title", e);
            }}
          />
          <FormField
            labelName="Story *"
            placeholder="Write your story"
            isTextArea
            value={form.description}
            handleChange={e => {
              handleFormChange("description", e);
            }}
            styles="basis-full"
          />

          <div className="w-full flex justify-start items-center p-4 bg-[#8c6dfd] h-[120px] rounded-[10px]">
            <img
              src={money}
              alt="money"
              className="w-[40px] h-[40px] object-contain"
            />
            <h4 className="font-epilogue font-bold text-[25px] text-white ml-[20px]">
              You will get 100% of the raised amount
            </h4>
          </div>

          <FormField
            labelName="Goal *"
            placeholder="ETH 0.50"
            inputType="number"
            value={form.target}
            handleChange={e => {
              handleFormChange("target", e);
            }}
          />

          <FormField
            labelName="End Date *"
            placeholder="End Date"
            inputType="date"
            value={form.deadline}
            handleChange={e => {
              handleFormChange("deadline", e);
            }}
          />
        </div>

        <FormField
          labelName="Campaign image *"
          placeholder="Place image URL of your campaign"
          inputType="url"
          value={form.image}
          handleChange={e => {
            handleFormChange("image", e);
          }}
        />

        <div className="flex justify-center items-center mt-[40px]">
          <CustomButton
            btnType="submit"
            title="Submit new campaign"
            styles="bg-[#1dc071]"
            handleClick={handleSubmit}
          />
        </div>
      </form>
    </div>
  );
};

export default CreateCampaign;
```

## Thư viện `ethers` trong Javascript

Ethers là một thư viện JavaScript dành cho việc tương tác với mạng Ethereum. Nó cung cấp các công cụ cho phép bạn tạo và gửi giao dịch, truy vấn dữ liệu từ các smart contract, và thực hiện các hoạt động khác trên mạng Ethereum.

Ethers cũng cung cấp một tập hợp các tiện ích hữu ích cho việc xử lý các giao dịch và dữ liệu trên mạng Ethereum, bao gồm việc mã hóa và giải mã dữ liệu, tạo và xác minh chữ ký, và xử lý các địa chỉ Ethereum.

Ethers được thiết kế để đơn giản hóa việc tương tác với mạng Ethereum cho các lập trình viên, và cung cấp một giao diện API dễ sử dụng và đầy đủ tính năng. Nó cũng được thiết kế để được tích hợp vào các dự án web và các ứng dụng JavaScript khác.

```bash title="npm"
npm install ethers
```

```title="yarn"
yarn add ethers
```

Sau khi cài đặt xong, bạn có thể sử dụng Ethers trong dự án React của bạn bằng cách khai báo nó trong một tập tin JavaScript:

```jsx
import * as ethers from "ethers";
```

Xem docs [tại đây](https://docs.ethers.org/v5/getting-started/)
