# Tìm hiểu về Keystone

##Mục lục:
###[1.Tổng quan chung về Keystone](#tongquan)

#####[1.1.Khái niệm](#khainiem)
#####[1.2.Công dụng](#congdung)

###[2.Cấu trúc của Keystone] (#cautruc)

#####[2.1.Kiến trúc của Keystone] (#kientruc)
#####[2.2.Tác dụng của các thành phần] (#thanhtphan)

###[3.Cơ chế hoạt động của Keystone trong Openstack] (#coche)
###[4.Các cơ chế xác thực trong Keystone] (#xacthuc)
###[5.Một số khái niệm trong Identity, Authentication] (#authen)


<a name="tongquan"></a>
####1.Tổng quan chung về Key stone:

<a name="khainiem"></a>
#####1.1.Khái niệm:
Keystone là 1 dịch vụ trong Openstack nó cung cấp các Identity, Token, Catalog và Policy service để sử dụng 1 cách cụ thể cho từng Project trong Openstack

<a name="congdung"></a>
#####1.2.Công dụng:
Về cơ bản thì Keystone có 2 chức năng chính
<ul>
<li>Quản lý user: Quản lý người sử dụng các dịch vụ và những quyền mà họ được phép làm</li>
<li>Xác thực dịch vụ: Cung cấp 1 danh mục những dịch vụ có hiệu lực và nơi những API endpoint được đặt</li>
</ul>

<a name="cautruc"></a>
####2.Cấu trúc của Keystone:

<a name="kientruc"></a>
#####2.1.Kiến trúc của Keystone:
Keystone cung cấp các xác thực cho các project khác trong Openstack
<img src="http://26a0ff8ca8ba32139f7d-db711c577a50b6bdc946ea71aaca027d.r97.cf1.rackcdn.com/openstack-conceptual-arch-folsom.jpg">
Về cơ bản Keytone có 4 thành phần, đó là: Identity, Token, Catalog, Policy
<img src="http://1.bp.blogspot.com/-BLElS5LHrbI/VFcOwKqN7PI/AAAAAAAAAPw/sOi-hj4GJ-Q/s1600/keystone_backends.png">

<a name="thanhphan"></a>
#####2.2.Công dụng của các thành phần:
- Identity:
<ul>
<li>Cung cấp các chứng chỉ xác thực và dữ liệu về người dùng, dữ liệu người dùng có thể đến từ nhiều nơi</li>
<li>Bao gồm nhưng không giới hạn SQL, LDAP, và Federated Identiy</li>
</ul>
- Token: 
<img src="https://www.safaribooksonline.com/library/view/identity-authentication-and/9781491941249/assets/image011.png">
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
<img src="https://www.safaribooksonline.com/library/view/identity-authentication-and/9781491941249/assets/image013.png">
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
<img src="http://2.bp.blogspot.com/-bPiAf5VkWiM/VEoB4XbZRpI/AAAAAAAAAFs/ABl9iaxnyhQ/s1600/Keystone_identityMgr-diagram.png">


<a name="xacthuc"></a>
####4.Các cơ chế xác thực trong Keystone:
Có rất nhiều các để xác thực với dịch vụ Keystone. 2 cách phổ biến nhất hiện này là xác thực bằng cách cũng cấp 1 password hoặc 1 token.
- *Password*: Cách phổ biến nhất đối với một người sử dụng hoặc dịch vụ để xác thực là cung cấp một mật khẩu.
- *Token*: User có thể yêu cầu 1 token mới thông qua token hiện tại, payload của POST này ít hơn đáng kể so với password
- *Quản lý truy cập và ủy quyền*:
Quản lý truy cập và ủy quyền là các APIs 1 user sử dụng là 1 lí do rất quan trọng để Keystone là 1 thành phần quan trọng trong Openstack. Cách tiếp cận của Keystone cho vấn đề này là tạo ra một policy Role-Based Access Control (RBAC) được thực thi trên mỗi public API endpoint. Các policy này được lưu trong 1 tập tin trên đĩa và đặt tên là policy.json.

<a name="authen"></a>
###5.Một số khái niệm trong authentication:
#####5.1.Project:
- Mỗi Project chứa những tài nguyên có thể giống và khác nhau, User cũng được coi là 1 dạng tài nguyên
- Các Project có thể chứ nhiều User và mỗi User có thể nằm trong các Project khác nhau
- Các User có quyền hạn khác nhau trong mỗi Project

#####5.2.Domain:
- Domain Keystone được sinh ra nhằm tránh tình trạng xảy ra xung đột giữa các Project, quyền hạn của các User.
- Deer cho các tổ chức cùng 1 lúc sử dụng các dịch vụ mà không xảy ra xung đột.
- Users có quyền hạn khác nhau đối với mỗi projects, mỗi domain.

<img src="http://916c06e9a68d997cd06a-98898f70c8e282fcc0c2dba672540f53.r39.cf1.rackcdn.com/ss.png">

#####5.3.Actor
Actor được xem là các user và user group sử dụng Cloud, kể từ đó khi gán 1 role đó là những đối tượng mà Role giao đến
<img src="http://916c06e9a68d997cd06a-98898f70c8e282fcc0c2dba672540f53.r39.cf1.rackcdn.com/ss.png:>
- *Role*: Là những vai trò của User được phép thực hiện trong Project hoặc Domain

<img src="https://camo.githubusercontent.com/045ea89e310e40d8067e344f40811027246d6974/68747470733a2f2f6f70656e2e69626d636c6f75642e636f6d2f646f63756d656e746174696f6e2f5f696d616765732f557365724d616e6167656d656e745769746847726f7570732e676966">





