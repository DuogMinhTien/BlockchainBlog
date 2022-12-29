---
sidebar_position: 1
---

# 1. Intro

- Connect wallet tại [thirdweb3](https://thirdweb.com/dashboard)

## Tạo dự án (BlockchainWeb3)

- Thêm folder `client` vào dự án

- Chạy lệnh sau tại thư mục root

```
npx thirdweb@latest create --contract
```

- Sau khi chạy lệnh sẽ có 4 câu hỏi:

  - Tên dự án

  - Chọn loại framework: Có 2 loại là `hardhat` và `forge`

    `Hardhat` là một khung phát triển để xây dựng và triển khai hợp đồng thông minh `Ethereum` và ứng dụng phân tán (`DApps`). Nó được xây dựng trên nền của `Máy Ảo Ethereum (EVM)` và cung cấp một tập hợp các công cụ và thư viện để phát triển, kiểm tra và triển khai các dự án `Ethereum`.

    `Forge` là một khung phát triển mã nguồn mở để xây dựng và triển khai các ứng dụng phân tán (`DApps`) trên `Ethereum`. Nó cung cấp một tập hợp các công cụ và thư viện để xây dựng, kiểm tra và triển khai các dự án `Ethereum`, và được thiết kế để linh hoạt và `modular`, cho phép lập trình viên lựa chọn các công cụ và thư viện phù hợp nhất với nhu cầu của họ.

    Cả `Hardhat` và `Forge` đều là các khung phát triển phổ biến cho việc xây dựng `DApps Ethereum`, nhưng chúng có một số khác biệt quan trọng. Hardhat định hướng vào cung cấp một tập hợp đầy đủ các công cụ và thư viện để xây dựng và triển khai các dự án `Ethereum`, trong khi `Forge` nổi bật hơn với sự linh hoạt và `modularity`, cho phép lập trình viên lựa chọn các công cụ họ cần. Ngoài ra, `Hardhat` có một cộng đồng lập trình viên lớn hơn và hoạt động hơn, với một phạm vi tài liệu và tài nguyên rộng hơn.

    Tuy nhiên, `Forge` có một điểm mạnh là nó cung cấp một tập hợp các công cụ và thư viện để xây dựng các `DApps Ethereum` một cách dễ dàng hơn, đặc biệt là cho những người mới bắt đầu. Nó cũng có một tài liệu và tài nguyên rộng lớn để hỗ trợ lập trình viên trong quá trình phát triển các dự án.

    Tóm lại, `Hardhat` và `Forge` là hai khung phát triển khác nhau cho việc xây dựng và triển khai các `DApps Ethereum`. `Hardhat` cung cấp một tập hợp đầy đủ các công cụ và thư viện cho việc phát triển và triển khai các dự án `Ethereum`, trong khi Forge nổi bật hơn với sự linh hoạt và `modularity`, cho phép lập trình viên lựa chọn các công cụ và thư viện phù hợp nhất với nhu cầu của họ.

  - Tên smart constract

  - Kiểu constract: Gồm 4 loại Empty Constract, ERC721, ERC20, ERC1155

    - `Empty Constract` là một loại hợp đồng thông minh không chứa bất kỳ mã hoặc logic nào. Nó là một placeholder cho một hợp đồng thông minh có thể được điền vào sau này với chức năng mong muốn.

    - `ERC721` là một tiêu chuẩn cho các token không đồng nhất (NFTs) trên blockchain Ethereum. NFTs là tài sản số duy nhất đại diện cho sở hữu của một mục tiêu hoặc một bức nội dung cụ thể, chẳng hạn như một đồ trang sức, một bức tranh nghệ thuật hoặc một tài sản số. `ERC721` định nghĩa một tập hợp quy tắc và hàm phải được thực thi bởi hợp đồng thông minh phát hành hoặc quản lý NFTs, cho phép chúng được tương thích với nhau và dễ dàng tích hợp vào các ứng dụng phân tán (DApps).

    - `ERC20` là một tiêu chuẩn cho các token đồng nhất trên blockchain Ethereum. Token đồng nhất là tài sản số đổi thay đổi, có nghĩa là một đơn vị của token bằng với một đơn vị khác. `ERC20` định nghĩa một tập hợp quy tắc và hàm phải được thực thi bởi hợp đồng thông minh phát hành hoặc quản lý token đồng nhất, cho phép chúng được tương thích với nhau và dễ dàng tích hợp vào DApps.

    - `ERC1155` là một tiêu chuẩn cho cả token đồng nhất và token không đồng nhất trên blockchain Ethereum. Nó cho phép hợp đồng thông minh quản lý một hợp đồng duy nhất có thể chứa cả hai loại token, cũng như định nghĩa quy tắc cho cách chúng có thể được kết hợp hoặc chia ra. Điều này có thể hữu ích trong những trường hợp khác nhau loại tài sản cần được gói lại cùng nhau, chẳng hạn như trong một trò chơi khi một người chơi có thể trao đổi một kết hợp các mục trò chơi khác nhau. Giống như `ERC721` và `ERC20`, `ERC1155` định nghĩa một tập hợp quy tắc và hàm phải được thực thi bởi hợp đồng thông minh phát hành hoặc quản lý những loại token này.

- Vào thư mục `blockchainweb3`, `cài đặt`dotenv` để chạy biến môi trường:

```
cd ./blockchainweb3
npm install dotenv
```

## VIDEO

<iframe width="100%" height="415" src="https://www.youtube.com/embed/BDCT6TYLYdI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen>
</iframe>
