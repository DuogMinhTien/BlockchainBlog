---
sidebar_position: 7
---

# 7. Homepage

## Thêm component `FundCard`

```jsx
import React from "react";

import { tagType, thirdweb } from "../assets";

import { daysLeft } from "../utils";

const FundCard = ({
  owner,
  title,
  description,
  target,
  deadline,
  amountCollected,
  image,
  handleClick,
}) => {
  const remainingDays = daysLeft(deadline);

  return (
    <div
      className="sm:w-[288px] w-full rounded-[15px] bg-[#1c1c24] cursor-pointer"
      onClick={handleClick}>
      <img
        src={image}
        alt="fund"
        className="w-full h-[158px] object-cover rounded-[15px]"
      />

      <div className="flex flex-col p-4">
        <div className="flex flex-row items-center mb-[18px]">
          <img
            src={tagType}
            alt="tag"
            className="w-[17px] h-[17px] object-contain"
          />
          <p className="ml-[12px] mt-[2px] font-epilogue font-medium text-[12px] text-[#808191]">
            Education
          </p>
        </div>

        <div className="block">
          <h3 className="font-epilogue font-semibold text-[16px] text-white text-left leading-[26px] truncate">
            {title}
          </h3>
          <p className="mt-[5px] font-epilogue font-normal text-[#808191] text-left leading-[18px] truncate">
            {description}
          </p>
        </div>

        <div className="flex justify-between flex-wrap mt-[15px] gap-2">
          <div className="flex flex-col">
            <h4 className="font-epilogue font-semibold text-[14px] text-[#b2b3bd] leading-[22px]">
              {amountCollected}
            </h4>
            <p className="mt-[3px] font-epilogue font-normal text-[12px] leading-[18px] text-[#808191] sm:max-w-[120px] truncate">
              Raised of {target}
            </p>
          </div>
          <div className="flex flex-col">
            <h4 className="font-epilogue font-semibold text-[14px] text-[#b2b3bd] leading-[22px]">
              {remainingDays}
            </h4>
            <p className="mt-[3px] font-epilogue font-normal text-[12px] leading-[18px] text-[#808191] sm:max-w-[120px] truncate">
              Days Left
            </p>
          </div>
        </div>

        <div className="flex items-center mt-[20px] gap-[12px]">
          <div className="w-[30px] h-[30px] rounded-full flex justify-center items-center bg-[#13131a]">
            <img
              src={thirdweb}
              alt="user"
              className="w-1/2 h-1/2 object-contain"
            />
          </div>
          <p className="flex-1 font-epilogue font-normal text-[12px] text-[#808191] truncate">
            by <span className="text-[#b2b3bd]">{owner}</span>
          </p>
        </div>
      </div>
    </div>
  );
};

export default FundCard;
```

## Thêm component `DisplayCampaigns`

```jsx
import React from "react";
import { useNavigate } from "react-router-dom";

import { loader } from "../assets";

import FundCard from "./FundCard";

const DisplayCampaigns = ({ title, isLoading, campaigns }) => {
  const navigate = useNavigate();

  const handleNavigate = campaign => {
    navigate(`/campaign-details/${campaign.title}`, {
      state: campaign,
    });
  };

  return (
    <div>
      <h1 className="font-epilogue font-semibold text-[18px] text-white text-left">
        {title} ({campaigns.length})
      </h1>

      <div className="flex flex-wrap mt-[20px] gap-[26px]">
        {isLoading && (
          <img
            src={loader}
            alt="loader"
            className="w-[100px] h-[100px] object-contain"
          />
        )}

        {!isLoading && campaigns.length === 0 && (
          <p className="font-epilogue font-semibold text-[14px] leading-[30px] text-[#818183]">
            You have not created any campaigns yet
          </p>
        )}

        {!isLoading &&
          campaigns.length > 0 &&
          campaigns.map(campaign => (
            <FundCard
              key={campaign.id}
              {...campaign}
              handleClick={() => handleNavigate(campaign)}
            />
          ))}
      </div>
    </div>
  );
};

export default DisplayCampaigns;
```

## export 2 component trên bằng file index.js

```jsx
export { default as DisplayCampaigns } from "./DisplayCampaigns";
export { default as FundCard } from "./FundCard";
```

## Thêm 2 hàm `getCampaigns` và `getUserCampaigns`

```jsx title="getCampaigns ()"
const getCampaigns = async () => {
  const campaigns = await contract.call("getCampaigns");

  // console.log(campaigns);

  const parsedCampaigns = campaigns.map((campaign, i) => {
    return {
      owner: campaign.owner,
      title: campaign.title,
      description: campaign.description,
      target: ethers.utils.formatEther(campaign.target.toString()),
      deadline: campaign.deadline.toNumber(),
      amountCollected: ethers.utils.formatEther(
        campaign.amountCollected.toString()
      ),
      image: campaign.image,
      pId: i,
    };
  });

  return parsedCampaigns;
};
```

```jsx title="getUserCampaigns()"
const getUserCampaigns = async () => {
  const allCampaigns = await getCampaigns();

  const filteredCampaigns = allCampaigns.filter(
    campaign => campaign.owner === address
  );

  return filteredCampaigns;
};
```

\*\*Thêm 2 hàm đấy vào prop `value` StateProvider

```jsx
<StateContext.Provider
  value={{
    address,
    contract,
    connect,
    createCampaign: publishCampaign,
    getCampaigns,
    getUserCampaigns,
  }}>
  {children}
</StateContext.Provider>
```

## Bổ sung file `Home.jsx` trong folder `pages`

```jsx
import React from "react";

import { useStateContext } from "../context";

import { DisplayCampaigns } from "../components";

const Home = () => {
  const [isLoading, setIsLoading] = React.useState(false);
  const [campaigns, setCampaigns] = React.useState([]);

  const { address, contract, getCampaigns } = useStateContext();

  const fetchCampaigns = async () => {
    setIsLoading(true);
    const data = await getCampaigns();
    setCampaigns(data);
    setIsLoading(false);

    console.log(data);
  };

  React.useEffect(() => {
    if (contract) fetchCampaigns();
  }, [address, contract]);

  return (
    <DisplayCampaigns
      title="All campaigns"
      isLoading={isLoading}
      campaigns={campaigns}
    />
  );
};

export default Home;
```

## Bổ sung file `Profile.jsx` trong folder `pages`

```jsx
import React from "react";

import { useStateContext } from "../context";

import { DisplayCampaigns } from "../components";

const Profile = () => {
  const [isLoading, setIsLoading] = React.useState(false);
  const [campaigns, setCampaigns] = React.useState([]);

  const { address, contract, getUserCampaigns } = useStateContext();

  const fetchCampaigns = async () => {
    setIsLoading(true);
    const data = await getUserCampaigns();
    setCampaigns(data);
    setIsLoading(false);

    // console.log(data);
  };

  React.useEffect(() => {
    if (contract) fetchCampaigns();
  }, [address, contract]);

  return (
    <DisplayCampaigns
      title="All campaigns"
      isLoading={isLoading}
      campaigns={campaigns}
    />
  );
};

export default Profile;
```
