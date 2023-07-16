<h2>Xây dựng Website bán hàng sử dụng công nghệ ReactJS & NodeJS ( API,Socket )</h2>
<h3>Website bao gồm các chức năng chính: </h3>
    <b>- Thêm, Xóa, Tìm Kiếm, Phân Trang, Phân Loại Sản Phẩm </b> </br>
    <b>- Đặt Hàng </b> </br>

    <b>- Gửi Email để xác nhận đơn hàng </b> </br>
    <b>- Giao Hàng ( Hiển thị và tính giá tiền của đơn hàng phụ thuộc vào quảng đường từ cửa hàng đến địa điểm nhận hàng của khách hàng và hiển thị bằng Google Map API )  </b> </br>
    <b>- Live Chat ( Tư Vấn Khách Hàng ) chưa hoàn thành </b> </br>
    <b>- Thanh Toán Paypal  chưa hoàn thành </b> </br>
---------------------------------------------------------------

## DESIGN DATABASE

- Product: _id, id_category, name_product, price_product, image, describe, gender, number
    + _id: id của sản phẩm,
    id_category: id của loại sản phẩm,
    name_product: tên của sản phẩm,
    price_product: giá của sản phẩm,
    image: hình ảnh của sản phẩm,
    describe: mô tả về sản phẩm,
    gender: sản phẩm thuộc giới tính,
    number: số lượng tồn,
    bảng Product có quan hệ một nhiều với bảng Category 
    bảng Product có quan hệ một nhiều với bảng Favorite
    bảng Product có quan hệ một nhiều với bảng Comment
    bảng Product có quan hệ một nhiều với bảng Detail_Oder
    + 1 product sẽ có 1 category
    + 1 product sẽ có nhiều favorite
    + 1 product sẽ có nhiều comment
    + 1 product sẽ có 1 detail_order
- Category: _id, category
    + _id: id của loại sản phẩm,
    category: tên của loại
    bảng Category có quan hệ một nhiều với bảng Product
    + 1 category sẽ có nhiều product
- User: _id, username, password, fullname, email, id_permission
    + _id: id của khách hàng,
    username: tên đăng nhập của khách hàng,
    password: mật khẩu đăng nhập của khách hàng,
    fullname: tên của khách hàng,
    email: email của khách hàng,
    id_permission: id permission thuộc bảng Permission,
    bảng User có quan hệ một nhiều với bảng Permission,
    bảng User có quan hệ một nhiều với bảng Comment,
    bảng User có quan hệ một nhiều với bảng Order,
    bảng User có quan hệ một nhiều với bảng Favorite
    + 1 user sẽ có nhiều comment
    + 1 user sẽ có 1 permission
    + 1 user sẽ có nhiều favorite
    + 1 user sẽ có nhiều order
- Permission: _id, permission
    + _id: id của quyền,
    permission: tên của quyền,
    bảng permission có quan hệ một nhiều với bảng user
    + 1 permission sẽ có nhiều user
- Order: _id, fullname, address, phone, total, status, id_user, id_payment, id_delivery
    + _id: id của đơn hàng,
    fullname: tên của người nhận hàng,
    address: địa chỉ của người nhận hàng,
    phone: số điện thoại của người nhận hàng,
    total: tổng tiền của đơn hàng,
    status: trạng thái của đơn hàng đó (1 - Chưa xác nhận, 2 - Đã xác nhận, 3 - Đang vận chuyển, 4 - Hoàn thành, 5 - Hủy đơn hàng),
    id_user: id của khách hàng,
    id_payment: id của phương thức thanh toán,
    id_delivery: id của phương thức vận chuyển,
    bảng Order có quan hệ một nhiều với bảng Detail_Order,
    bảng Order có quan hệ một một với Delivery vì hệ thông chỉ có 1 phương thức vận chuyển DRIVING,
    + Fullname, address, phone có thể được tùy chọn phụ thuộc vào người nhận hàng
    + 1 order sẽ có 1 delivery
    + 1 order sẽ có 1 payment
    + 1 order sẽ có nhiều detail_order
    + Nhiều order sẽ thuộc 1 user
- Detail_Order: _id, price_product, name_product, count, size, id_order, id_product
    + _id: id của chi tiết đơn hàng,
    price_product: giá của sản phẩm,
    name_product: tên của sản phẩm,
    count: số lượng của sản phẩm,
    size: kích thước của sản phẩm,
    id_order: id của đơn hàng,
    id_product: id của sản phẩm,
    bảng detail_order sẽ có quan hệ một một với bảng product
    bảng detail_order sẽ có quan hệ một nhiều với bảng order
    + Khi product mình thay đổi thì bên detail_order cũng sẽ thay đổi nên mình phải thêm 2 trường         name_product, price_product tránh trường hợp điều đó xảy ra.
    + 1 detail_order sẽ có 1 product
    + Nhiều detail_order sẽ thuộc 1 order
- Payment: _id, pay_name,
    + _id: id của phương thức thanh toán,
    pay_name: tên phương thức thanh toán,
    bảng payment sẽ có quan hệ một nhiều với bảng order
    + 1 payment sẽ có nhiều order
- Comment: _id, content, star1, star2, star3, star4, star5, id_user, id_product,
    + _id: id của bình luận,
    content: nội dung của đánh giá,
    star1, star2, star3, star4, star5: là số sao mà khách hàng đánh giá,
    id_user: id của khách hàng mà đánh giá,
    id_product: id của sản phẩm đánh giá,
    bảng comment sẽ có quan hệ một nhiều với bảng product
    bảng comment sẽ có quan hệ một nhiều với bảng user
    + Nhiều comment sẽ thuộc 1 user
    + Nhiều comment sẽ thuộc 1 product
- Favorite: _id, id_user, id_product
    + _id: là id của favorite,
    id_user: id của khách hàng,
    id_product: id của sản phẩm
    bảng favorite sẽ có quan hệ một nhiều với bảng product
    bảng favorite sẽ có quan hệ một nhiều với bảng user
    + Nhiều favorite thuộc 1 product
    + Nhiều favorite thuộc 1 user
- Note: _id, fullname, phone
    + _id: id của phương thức vận chuyện,
    fullname: tên của người nhận hàng,
    phone: điện thoại của người nhận hàng,
    bảng Note sẽ có quan hệ một một với bảng Order
    + 1 Note sẽ thuộc 1 order

- api/product : PRODUCT API ENDPOINT

    - router.get('/', Products.index)

    - router.get('/category', Products.category)

    - router.get('/:id', Products.detail)

    - router.get('/category/gender', Products.gender)

    - router.get('/category/pagination', Products.pagination)

    - router.get('/scoll/page', Products.scoll)
  
- api/user : USER API ENDPOINT

    - router.get('/', Users.index)

    - router.get('/:id', Users.user)

    - router.get('/detail/login', Users.detail)

    - router.post('/', Users.post_user)
   
- api/detail_order : DETAIL ORDER API ENDPOINT

    - router.get('/:id', Detail_Order.detail)

    - router.post('/', Detail_Order.post_detail_order)

- api/order : ORDER API ENDPOINT

    - router.get('/order/:id', Order.get_order)

    - router.get('/order/detail/:id', Order.get_detail)

    - router.post('/order', Order.post_order)

    - router.post('/email', Order.send_mail)

- api/delivery : DELIVERY API ENDPOINT

    - router.post('/', Delivery.post_delivery)

    - router.get('/:id', Delivery.get_delivery)

- api/comment : COMMENT API ENDPOINT

    - router.get('/:id', Comment.index)

    - router.post('/:id', Comment.post_comment)
    


```


## Get Started

``` bash
# install dependencies
npm install
```
``` bash
# run project
npm start
```

## Features
- Login, Register, Forgot Password, ResetPassword .
- CRUD Products, Users, Carts, Favorite, Order...
- Axios Products, Carts, Orders...
- Products url query
- Send email to user.
- Draw route between two location
- Payment on paypal
- Plugin Messenger of Facebook


## Technical details
- Nodejs, Reactjs.
- Express.
- Mongodb, Mongoose.

