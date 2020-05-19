# SNMP - Simple Network Management Protocol
Là giao thức ứng dụng của IETF dành cho quản lý mạng, dựa trên nền giao thức TCP/IP. Giao thức SNMP vận chuyển dữ liệu từ client(thiết bị mạng đang giám sát) đến nơi mà dữ liệu được lưu trong log file nhằm phân tích dễ dàng hơn.

![](https://user-images.githubusercontent.com/61723456/82040245-8364b280-96d0-11ea-9d1b-9c55770e6492.png)

Một số khả năng của SNMP:
+ Theo dõi tốc độ đường truyền của một router, biết được tổng số byte đã truyền/nhận.
+ Lấy thông tin máy chủ đang có bao nhiêu ổ cứng, mỗi ổ cứng còn trống bao nhiêu.
+ Tự động nhận cảnh báo khi switch có một port bị down.
+ Điều khiển tắt (shutdown) các port trên switch.

## Hoạt động của SNMP
Có 2 nhân tố chính trong SNMP: Manager và Agent.
+ Các SNMP agent sẽ gửi một cơ sở dữ liệu đực gọi là Management Information Base (MIB). Trong đó chứa các thông tin khác nhau về hoạt động của thiết bị agent đang giám sát.
+ Các phần mềm quản trị SNMP Manager sẽ thu thập thông tin này qua giao thức SNMP.
SNMP được thiết kế để có thể hoạt động độc lập với các kiến trúc và cơ chế của các thiết bị hỗ trợ giám sát. Các thiết bị khác nhau có hoạt động khác nhau nhưng đáp ứng SNMP là giống nhau.
![](https://user-images.githubusercontent.com/61723456/82040354-aee79d00-96d0-11ea-8599-fad7d6fba964.png)

## Các phiên bản của SNMP
SNMP có 4 phiên bản: SNMPv1, SNMPv2c, SNMPv2u và SNMPv3. Các phiên bản này chỉ khác nhau một chút ở định dạng bản tin và có thêm các cách thức bảo mật cho phiên bản từ v2c trở đi.

## Các thành phần trong SNMP
Kiến trúc SNMP gồm 2 phần: Network management stationnvaf network element.
+ Network management station thường là một máy tính chạy phần mềm quản lý mạng SNMP dùng để giám sát và điều khiển các thiết bị tập trung network element.
+ Network element là các thiết bị, máy tính, hoặc phần mềm tương thích SNMP và được quản lý bởi network management station. Như vậy element bao gồm device, host và application.
+ SNMP agent là một tiến trình (process) chạy trên network element, có nhiệm vụ cung cấp thông tin của element cho station, nhờ đó station có thể quản lý được element. Chính xác hơn là application chạy trên station và agent chạy trên element mới là 2 tiến trình SNMP trực tiếp liên hệ với nhau.

## Object ID
Một thiết bị hỗ trợ SNMP có thể cung cấp nhiều thông tin khác nhau, mỗi thông tin đó gọi là một object.
Ví dụ:
+ Máy tính có thể cung cấp các thông tin : tổng số ổ cứng, tổng số port nối mạng, tổng số byte đã truyền/nhận, tên máy tính, tên các process đang chạy, ...

+ Router có thể cung cấp các thông tin : tổng số card, tổng số port, tổng số byte đã truyền/nhận, tên router, tình trạng các port của router, ...

Mỗi object có một tên gọi và một mã số để nhận dạng object đó, mã số gọi là `Object ID` (OID). VD:
+ Tổng số port giao tiếp (interface) được gọi là ifNumber, OID là 1.3.6.1.2.1.2.1.

+ Địa chỉ Mac Address của một port được gọi là ifPhysAddress, OID là 1.3.6.1.2.1.2.2.1.6.

+ Số byte đã nhận trên một port được gọi là ifInOctets, OID là 1.3.6.1.2.1.2.2.1.10.

## Object access
Mỗi object có quyền truy cập là `READ_ONLY` hoặc `READ_WRITE`. Mọi object đều có thể đọc được nhưng chỉ những object có quyền `READ_WRITE` mới có thể thay đổi được giá trị.
VD: Tên của một thiết bị (sysName) là `READ_WRITE`, ta có thể thay đổi tên của thiết bị thông qua giao thức SNMP. Tổng số port của thiết bị (ifNumber) là `READ_ONLY`, ta không thể thay đổi số port của nó.

# Management Information Base (MIB)
`MIB` (cơ sở thông tin quản lý) là một cấu trúc dữ liệu gồm các đối tượng được quản lý (managed object), được dùng cho việc quản lý các thiết bị chạy trên nền TCP/IP. `MIB` là kiến trúc chung mà các giao thức quản lý trên TCP/IP nên tuân theo, trong đó có SNMP. MIB được thể hiện thành 1 file (MIB file), và có thể biểu diễn thành 1 cây (MIB tree). MIB có thể được chuẩn hóa hoặc tự tạo.
![](https://user-images.githubusercontent.com/61723456/82040512-e5251c80-96d0-11ea-82c4-8f4d501cf812.png)

Một node trong cây là một object, có thể được gọi bằng tên hoặc id. Ví dụ : 
+ Node iso.org.dod.internet.mgmt.mib-2.system có OID là 1.3.6.1.2.1.1, chứa tất cả các object liên quan  đến  thông  tin  của  một  hệ  thống  như  tên  của  thiết  bị  (iso.org.dod.internet.mgmt.mib-2.system.sysName hay 1.3.6.1.2.1.1.5).
+ Các OID của các hãng tự thiết kế nằm dưới iso.org.dod.internet.private.enterprise. Ví dụ : Cisco nằm dưới iso.org.dod.internet.private.enterprise.cisco hay 1.3.6.1.4.1.9, Microsoft nằm dưới iso.org.dod.internet.private.enterprise.microsoft hay 1.3.6.1.4.1.311. Số 9 (Cisco) hay 311 (Microsoft) là số dành riêng cho các công ty.

## Các phương thức của SNMP
Giao thức SNMPv1 có 5 phương thức hoạt động, tương ứng với 5 loại bản tin như sau :
|Bản tin|Mô tả|
|:-----:|:-----:|
|GetRequest|Manager gửi GetRequest cho agent để yêu cầu agent cung cấp thông tin nào đó dựa vào ObjectID(trong đó GetRequest có chưa OID).|
|GetNextRequest|Manager gửi GetnextRequest có chứa một ObjectID cho agent để yêu cầu cung cấp thông tin nằm kế tiếp ObjectID đó trong MIB.|
|SetRequest|Manager gửi SetRequest cho agent để đặt giá trị cho đối tượng của agent dựa vào ObjectID.|
|GetResponse|Agent gửi GetResponse cho Manager để trả lời khi nhận được GetRequest/GetNextRequest|
|Trap|Agent tự động gửi Trap cho Manager khi có một sự kiện xảy ra đối với object nào đó trong agent.|

Mỗi bản tin đều có chứa OID để cho biết object mang trong nó là gì. OID trong `GetRequest` cho biết nó muốn lấy thông tin của object nào. OID trong `GetResponse` cho biết nó mang giá trị của object nào. OID trong `SetRequest` chỉ ra nó muốn thiết lập giá trị cho object nào. OID trong Trap chỉ ra nó thông báo sự kiện xảy ra đối với object nào.

## Cơ chế bảo mật cho SNMP
Một SNMP management station có thể quản lý/giám sát nhiều SNMP element, thông qua hoạt động gửi `request` và nhận `trap`. Tuy nhiên một SNMP element có thể được cấu hình để chỉ cho phép các SNMP management station nào đó được phép quản lý/giám sát mình.

Cơ chế bảo mật đơn giản có thể dùng `community string`

### Community string 
`Community string` là một chuỗi ký tự được cài đặt giống nhau trên cả `SNMP manager` và `SNMP agent`, đóng vai trò như mật khẩu giữa 2 bên khi trao đổi dữ liệu. Community string có 3 loại : `Read-community`, `Write-Community` và `Trap-Community`.

Khi  manager  gửi `GetRequest`,  `GetNextRequest` đến agent  thì  trong bản tin gửi  đi  có  chứa  `Read-Community`. Khi agent nhận được bản tin request thì nó sẽ so sánh `Read-community` do manager gửi và `Read-community` mà nó được cài đặt. Nếu 2 chuỗi này giống nhau, agent sẽ trả lời; nếu 2 chuỗi này khác nhau, agent sẽ không trả lời.

`Write-Community` được dùng trong bản tin `SetRequest`. Agent chỉ chấp nhận thay đổi dữ liệu khi `write-community` 2 bên giống nhau.

`Trap-community` nằm trong bản tin trap của `trap sender` gửi cho `trap receiver`. `Trap receiver` chỉ nhận và lưu trữ bản tin trap chỉ khi `trap-community` 2 bên giống nhau, tuy nhiên cũng có nhiều `trap receiver` được cấu hình nhận tất cả bản tin trap mà không quan tâm đến `trap-community`.

Trên hầu hết hệ thống, `read-community` mặc định là `public`, `write-community` mặc định là private` và `trap-community` mặc định là `public`.

`Community string` chỉ là chuỗi ký tự dạng cleartext, do đó hoàn toàn có thể bị nghe lén khi truyền trên mạng. Hơn nữa, các community mặc định thường là `public` và `private` nên nếu người quản trị không thay đổi thì chúng có thể dễ dàng bị dò ra. Khi community string trong mạng bị lộ, một người dùng bình thường tại một máy tính nào đó trong mạng có thể quản lý/giám sát toàn bộ các device có cùng community mà không được sự cho phép của người quản trị.
