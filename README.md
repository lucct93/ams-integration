# Giới thiệu tổng quan

_Tài liệu cung cấp cho đối tác các thông tin kết nối kỹ thuật tới hệ thống AMS VietnamWorks_
## Sơ đồ tích hợp
![ScreenShot](https://static.swimlanes.io/19169a3a958b1ebe6250405b8737f4d8.png)

#### Thông tin kết nối
Thông tin kết nối bao gồm các thông tin được VietnamWorks cung cấp nhằm xác thực thông tin đối tác

    CLIENT_ID: Số định danh đối tác do VietnamWorks cung cấp.
    PARTNER_CODE: Mã đối tác do VietnamWorks cung cấp.
    SECRET_KEY: Được dùng xác thực và tạo token

#### Môi trường kết nối


Môi trường | Domain 
--- | ---
Production | https://ms.vietnamworks.com
Development | https://ms.vietnamworks.com

### JWT Token
Token là mã xác thực luôn được gắn vào Header của mỗi Request nhằm xác thực Đối tác
#### Request tạo Token
Thuộc tính | Giá trị
--- | ---
Đường dẫn | /api/v1/auth
Phương thức | POST
Xác thực | Không

Các tham số
Tham số | Loại | Giải thích 
--- | --- | ---
clientId | number | CLIENT_ID được cung cấp
partnerCode | string | PARTNER CODE được cung cấp
secretKey | string | SECRET KEY được cung cấp
expiresIn | number | Thời gian hiệu lực của token (giây)

#### Dữ liệu trả về
Thuộc tính | Loại | Giải thích
--- | --- | ---
accessToken | number | Token truy cập
refreshToken | array | Token dùng để xin cấp lại sau khi Token truy cập hết hạn
expiresIn | number | Thời gian hiệu lực của token (giây)

#### Request Làm mới Token
Thuộc tính | Giá trị
--- | ---
Đường dẫn | /api/v1/auth/refreshToken
Phương thức | POST
Xác thực | Không

Các tham số
Tham số | Loại | Giải thích 
--- | --- | ---
refreshToken | string | refreshToken được cung cấp trước đó

#### Dữ liệu trả về
Thuộc tính | Loại | Giải thích
--- | --- | ---
accessToken | number | Token truy cập
refreshToken | array | Token dùng để xin cấp lại sau khi Token truy cập hết hạn
expiresIn | number | Thời gian hiệu lực của token (giây)

### Tích hợp xác thực

Trước khi kết nối, Đối tác cần chắc chắn rẳng đã xác thực và nhận được accessToken và refreshToken.
Token sẽ luôn được gắn vào Header với mối Request cần xác thực

Tham số | Loại | Giá trị 
--- | --- | ---
Content-Type | string | "application/json"
Authorization | string | "Bear <chuỗi accessToken>"

### Lấy Danh sách Việc làm
Trả về danh sách các Việc làm của đối tác
##### Thông tin kết nối
Các thuộc tính bao gồm:
Thuộc tính | Giá trị 
--- | ---
Đường dẫn | /api/v1/jobs
Phương thức | GET
Xác thực | Có
Loại | json

Các tham số
Tham số | Loại | Giải thích 
--- | --- | ---
status | Query | Trang thái việc làm. Bao gồm: ONLINE/EXPIRED/VIRTUAL
keyword | Query | Từ khóa sẽ được tìm kiếm trong tên Việc làm
page | Query | Số trang hiện tại
perPage | Query | Số lượng việc làm trên mỗi trang hiện tại

Mẫu ví dụ

https://ms.vietnamworks.com/api/v1/jobs?status=ONLINE&keyword=accountant&page=1&itemPerPage=20

##### Dữ liệu trả về
Thông tin chung
Thuộc tính | Loại | Giải thích
--- | --- | ---
total | number | Tổng số Việc làm đang hoạt động
items | array | Danh sách các việc làm
page | number | Trang hiện tại. Mặc định 1
perPage | number | Số lượng việc làm trên mỗi trang hiện tại. Mặc định 20

Thông tin mỗi Việc làm
Thuộc tính | Loại | Giải thích
--- | --- | ---
id | number | Định danh của một Việc làm
title | string | Tiêu đề
location | string | Vị trí tuyển dụng
expiredDate | string | Ngày hết hạn
totalApplications | number | Tổng số application
createdDate | string | Thời gian tạo
approvedDate | string | Ngày được Chấp nhận
originalLink | string | Đường dẫn gốc trên trang quản lý VietnamWorks
status | string | Trạng thái của Việc làm
process | array | Danh sách theo thứ tự Quy trình tuyển dụng

### Danh sách Ứng tuyển của Việc làm
Trả về danh sách các Ứng tuyển của một Việc làm
##### Thông tin kết nối
Các thuộc tính
Thuộc tính | Giá trị 
--- | ---
Đường dẫn | /api/v1/jobs/<jobId>/application
Phương thức | GET
Xác thực | Có
Loại | json

Các tham số
Tham số | Loại | Giải thích 
--- | --- | ---
jobId | Path | Được đặt trên đường dẫn, là Id của một Việc làm được tìm thấy trong Đầu nối Việc làm
keyword | Query | Từ khóa sẽ được tìm kiếm trong tên Việc làm
page | Query | Số trang hiện tại
perPage | Query | Số lượng việc làm trên mỗi trang hiện tại

Ví dụ: 
https://ms.vietnamworks.com/api/v1/jobs/xxxxxx/application?page=1&itemPerPage=20

##### Dữ liệu trả về
Dữ liệu chung
Thuộc tính | Loại | Giải thích
--- | --- | ---
total | number | Tổng số Ứng tuyển đang hoạt động
items | array | Danh sách các Ứng tuyển
page | number | Trang hiện tại
perPage | number | Số lượng việc làm trên mỗi trang hiện tại

Thông tin mỗi Ứng tuyển
Thuộc tính | Loại | Giải thích
--- | --- | ---
id | number | Định danh của một Ứng tuyển
avatar | string | Đường dẫn Hình đại diện
fullname | string | Họ và tên
email | string | Hộp thư điện tử
phone | string | Số điện thoại
downloadLink | string | Đường dẫn tải về Tệp ứng tuyển
originalLink | string | Đường dẫn gốc trên trang quản lý VietnamWorks
status | string | Trạng thái của Ứng tuyển
createdDate | string | Thời gian tạo
