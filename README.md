# Tìm hiểu về Keystone

##Mục lục:
####[1.Tổng quan chung về Keystone](#tongquan)

-[1.1.Khái niệm](#khainiem)
-[1.2.Công dụng](#congdung)

####[2.Cấu trúc của Keystone] (#cautruc)

-[2.1.Kiến trúc của Keystone] (#kientruc)
-[2.2.Tác dụng của các thành phần] (#thanhtphan)

####[3.Cơ chế hoạt động của Keystone trong Openstack] (#coche)
####[4.Các cơ chế xác thực trong Keystone] (#xacthuc)


<a name="tongquan"></a>
####1.Tổng quan chung về Key stone:

<a name="khainiem"></a>
#####1.1.Khái niệm:
Keystone là 1 dịch vụ trong Openstack nó cung cấp các Identity, Token, Catalog và Policy service để sử dụng 1 cách cụ thể cho từng Project trong Openstack

<a name="congdung"></a>
#####1.2.Công dụng:
Về cơ bản thì Keystone có 2 chức nắng chính
<ul>
<li>Quản lý user: Quản lý người sử dụng các dịch vụ và những quyền mà họ được phép làm</li>
<li>Xác thực dịch vụ: Cung cấp 1 danh mục những dịch vụ có hiệu lực và nơi những API endpoint được đặt</li>
</ul>

<a name="cautruc"></a>
####2.Cấu trúc của Keystone:

<a name="kientruc"></a>
#####2.1.Kiến trúc của Keystone:
Về cơ bản Keytone có 4 thành phần, đó là: Identity, Token, Catalog, Policy
<img src="">

<a name="thanhphan"></a>
#####2.2.Công dụng của các thành phần:
- Identity:
<ul>
<li>Cung cấp các chứng chỉ xác thực và dữ liệu về người dùng, dữ liệu người dùng có thể đến từ nhiều nơi</li>
<li>Bao gồm nhưng không giới hạn SQL, LDAP, và Federated Identiy</li>
</ul>
- Token: 
<ul>
<li>Xác thực và quản lý sử dụng các token cho việ thẩm định yêu cầu 1 khi thông tin người dùng/ Tenant đã được xác định</li>
<li>Các token chứa thông tin xác thực người dùng như username, password, email..</li>
</ul>
- Catalog:
<ul>
<li>Chứa các URL và các endpoint</li>
<li>Cung cấp các endpoint đăng kí, sử dụng cho endpoint tìm kiếm</li>
<li>Nếu không có thành phần này thì các user và các ứng dụng sẽ không biết nơi để định tuyến, nơi để yêu cầu tạo ra các máy ảo hoặc storage object</li>
<li>Chia thành 1 danh sách các endpoint và mỗi endpoint chia thành các URL admin, URL nội bộ và public URL</li>
</ul>
- Policy: Cung cấp các chính sách của Keystone 
- Mỗi dịch vụ có thể chỉnh sửa để cho phép Keystone sử dụng 1 backend phù hợp với yêu cầu của môi trường. Backend của mỗi service dựa vào file keystone.conf
- Các backend trong Keystone:
<ul>
<li>KVS backend: Là 1 giao diện phụ trợ, hỗ trợ cho việc tra cứu các primary key</li>
<li>SQL backend: Sử dụng SQLAIchemy để lưu trữ dữ liệu 1 các liên tục</li>
<li>PAM backend: Là 1 backend cơ bản, sử dung dịch vụ PAM để xác thực, cũng cấp mối quan hệ 1-1 giữa user và tenant</li>
<li>LDAP backend: Lưu trữ các User và Tenant trong các subtrees</li>
<li>Template backend: Là 1 backend đơn giản được sử dụng để chỉnh sửa keystone</li>
</ul>

<a name="coche"></a>
####3.Cơ chế hoạt động của Keystone trong Openstack:
Keystone workflow:
<img src="https://docs.google.com/drawings/d/12xmhLS3Jwqr3IbDkXj9Ta223fH49vRcZSLl23rjtL8A/edit?hl=en_US&pli=1">

<a name="xacthuc"></a>
####4.Các cơ chế xác thực trong Keystone:


