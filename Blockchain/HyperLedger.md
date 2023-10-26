# HyperLedger

- Là một `Private Blockchain`
- Thường được sử dụng trong các mạng lưới doanh nghiệp
- HyperLedger đáp ứng các nhu cầu của doanh nghiệp như: bảo vệ dữ liệu người dùng, chỉ share dữ liệu nội bộ, giới hạn số lượng người tham gia vào mạng Blockchain, và đảm bảo mạng Blockchain của là Private

# HyperLedger Fabric

- Là một trong số những `Framework` phổ biến của HyperLedger
- HyperLedger Fabric chia các node con thành các mạng nhỏ hơn (giống chia subnets trong mạng), hay còn được gọi là `Channel`
- Nếu muốn gia nhập một Channel bất kì thì cần phải thông qua `MSP - Membership Service Provider`, người này sẽ đảm nhận việc xác nhận gia nhập Channel khi có người dùng gửi request yêu cầu gia nhập
- Smart contract trong HyperLedger Fabric được biết đến với tên gọi khác là `Chaincode`
- Chaincode thường được viết bằng JavaScript, Java, hoặc Golang, nhưng ngôn ngữ phổ biến nhất được dùng để tạo ra Chaincode là `Golang`
- `Ledger` trong HyperLedger Fabric được chia làm 2 phần: World State (dùng để lấy ra data hiện tại của một asset) and Transaction Log (dùng để truy ra lịch sử của asset đó)
  + Ví dụ: A(coin) -> X, X(coin) -> B. 
    - World State: B đang sở hữu coin
    - Transaction Log: A gửi coin cho X, X gửi coin cho B, B nhận được coin