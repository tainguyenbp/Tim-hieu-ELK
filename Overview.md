
#Overview
##1. Log

Log  file: là một tệp tin được tạo ra bởi một máy chủ web hoặc máy chủ proxy chứa tất cả các thông tin về các hoạt động trên máy chủ đó, như thông tin người truy cập, thời gian khách hàng viếng thăm, ip,.. 

Các file log hệ thống lưu những thông tin hoạt đông của các function thiết yếu trong hệ thống. Ví dụ bao gồm cơ chế ủy quyền, các tiến trình hệ thống, các message hệ thống, syslog...

Tác dụng của log:
<ul>
<li> Ghi lại liên tục các thông báo về hoạt động của cả hệ thống hoặc của các dịch vụ được triển khai trên hệ thống và file tương ứng. Log file thường là các file văn bản thông thường dưới dạng “clear text” tức là bạn có thể dễ dàng đọc vì thế có thể sử dụng các trình soạn thảo văn bản(vim, vi, nano,..) hoặc các trình xem văn bản thông thường( cat, tailf, head..) để đọc file log.</li>
<li> Các file log cung cấp bất cứ thứ gì bạn cần biết, để giải quyết các rắc rối miễn là biết ứng dụng nào, tiến trình nào được ghi vào log nào cụ thể.</li>
<li> Hậu hết các linux thì log được lưu tại **/var/log**.</li>
</ul>

File log (ubuntu 14.04) lưu trong /var/log/

<img src="https://github.com/trangnth/Syslog/blob/master/img/var-log.png">

* alternatives.log: thông tin update-alternatives đăng nhập vào file log này

* apt: Chứa các file log như:
 .swo	history.log	term.log	.history.log.swp	.swp

* auth.log: các log về xác thực

* kern.log: các log về nhân của hệ điều hành

* dpkg.log: chứa thông tin đăng nhập khi một phần mềm được cài đặt hoặc loại bỏ bằng lệnh dpkg

* boot.log: chứa thông tin đăng nhập khi khởi động hệ thống

* btmp: tập tin này chứa thông tin về attemps đăng nhập thất bại

* dmesg: các log mới 

* dmesg.0: các log cũ nhưng chưa đc nén

* dmesg.1.gz: các log cũ đã đc nén lại

* faillog: có người dùng không thành attemps đăng nhập. Sử dụng lệnh faillog để hiển thị các nội dung của tệp tin này.

* installer: chứa các file đã cài đặt

* rsyslog.log: rsyslog error messages

* syslog: Những thông báo được generated bởi bản thân syslogd

* mark : Những thông báo được generated bởi bản thân syslogd. Nó chỉ chứa một biến timestamp và một chuỗi --MARK--”.

* ftp: log liên quan đến dịch vụ ftp

* lpr: hệ thống in ấn

* cron: là một tiện ích cho phép thực hiện các tác vụ một cách tự động theo định kỳ, ở chế độ nền của hệ thống

* daemon: sử dụng bởi các tiến trình hệ thống và các daemons khác. deamon là một chương trình hoạt hoạt động liên tục. deamon khác với các ứng trình bình thường ở chỗ, các ứng trình bình thường dừng lại sau khi hoàn tác một chuỗi các thao tác.

* user: thông báo về tiến trình người dùng thông thường

* news: hệ thống tin tức, liên quan đến giao thức Network News Protocol (nntp)

###Log hệ thống :
**Authorization Log**

Authorization log lưu các thông tin về các hệ thống ủy quyền, các cơ chế ủy quyền các user, nhắc nhở về user password, ví dụ như hệ thống PAM (Pluggable Authentication Module), sudo command, các đăng nhập tới sshd. Các thông tin này được lưu lại trong /var/log/auth.log File log cung cấp các thông tin về đăng nhập user, việc sử dụng sudo command.

**Daemon Log**

Một daemon là một chương trình được chạy ở nền hệ thống, biểu diễn một vài hoạt động quan trọng của những hệ thống. Các tiến trình này lưu log trong /var/log/daemen.log và chứa các thông tin về việc hệ thống đang chạy như thế nào và các tiến trình ứng dụng như gdm (Gnome Display Manager) , hcid (Bluetooth HCI daemon), mysqld (MYSQL database)

**Debug Log**

Các file log về dubug lưu tại `/var/log/debug`, cung cấp thông tin cần thiết cho việc trouble-shooting các custom-build kernel.

**Kernal Ring Buffer**

Đây không hẳn là một file log, phần nào giống một khu vực trong một kernal đang chạy, bạn có thể query tới các message về việc kernal bootup thông qua *dmesg*. Để thấy các message, sử dụng :
`dmesg | less`

Hoặc tìm kiếm các dòng để cập đến Plug & Play systen, sử dụng grep như sau :
`dmesg | grep pnp | less`

**System log**

Log hệ thống thông thường chứa các thông tin mặc định của hệ thống, thường được lưu trong /var/log/syslog hoặc /var/log/message.

###Log rotation – xoay vòng file log
Thông thường khi nhìn vào thư mục /var/log sẽ thường có các file như syslog.1.gz, syslog.2.gz

<img src="https://github.com/trangnth/Syslog/blob/master/img/var-log.png">

Đó là các rotated log file. Hệ thống sẽ tự động nén các file log cũ lại sau 1 time-frame được định sẵn, và bắt đầu một file log gốc mới.

Mục đích của quá trình log rotation nhằm lưu trữ và nén các log cũ để tiết kiệm không gian ổ đĩa, nhưng vẫn sẵn sàng dùng lại khi cần thiết.

Thông thường, quá trình rotation được được cấu hình trong file /etc/logrotate.conf. Các file cấu hình rotate riêng cho một số file log như: rsyslog,dpkg, apache, apt,.. sẽ được thêm vào trong /etc/logrotate.d/

##2. Syslog
Syslog là một giao thức client/server là giao thứ để chuyển log và thông điệp đến máy nhận log. Máy nhận log thường được gọi là syslogd, syslog daemon hoặc syslog server.

Syslog có thể gửi qua ả UDP và TCP. Dữ liệu được gửi dưới dạng cleartext.

Dùng cổng 514.

*Các hành vi của syslogd được kiểm soát bởi file cấu hình /etc/syslog.conf*

Một số khái niệm cơ bản:
- Facility: giúp kiểm soát log đến dựa vào nguồn gốc được quy định như từ ứng dụng hay tiến trình nào. Syslog sử dụng facility để quy hoạch lại log như vậy có thể coi facility là đại diện cho đối tượng tạo ra thông báo (kernel, process, apps,..).
- Priority (level): mức độ quan trọng của log message được chỉ định.
- Selector (bộ chọn): Một sự kết nối của một hoặc nhiều phương tiện và mức độ. Khi một sự kiện mới đến kết nối với một bộ chọn, một hành động được thực hiện.
- Action (hành động): Điều gì xảy ra khi một thông tin mới đến kết nối với một bộ chọn. Các hành động có thể ghi thông tin tới file ghi log, phản xạ thông tin tới một bàn điều khiển hoặc thiết bị khác, ghi thông báo tới hệ thống ghi log của người sử dụng hoặc gửi thông báo cùng với máy chủ syslog khác.

###Các hành động ghi log trong linux
* Thông tin ghi log tới một file hoặc một thiết bị. Vd: `/var/log/lpr.log` hoặc `/dev/console`
* Gửi một thông báo tới một người sử dụng. Bạn có thể xác định nhiều tên sử dụng bằng việc ngăn cách chúng bởi dấu phẩy ( ví dụ root, amrood)
* Gửi một thông  báo tới tất cả người dùng. Trong trường hợp này, trường hành động bao gồm một dấu *
* Gửi một thông báo thông qua pipe tới một chương trình. Trong trường hợp này, chương trình được xác định sau ký hiệu pipe ( | )
* Gửi thông báo tới syslog trên một host khác. Trong trường hợp này, trường hành động bao gồm một tên host, được đặt trước bởi một dấu ký hiệu(vd: @tutorial.com)

<img src="https://github.com/trangnth/Syslog/blob/master/img/nguon-sinh-log.png">
<img src="https://github.com/trangnth/Syslog/blob/master/img/muc%20do%20canh%20bao.png">

Ngoài ra còn một mức đặc biệt được gọi là none, mức này sẽ disable facility đi cùng. Level định nghĩa một số lượng các bản ghi chi tiết trong log file. Dấu sao [*] có thể sử dụng để miêu tả cho tất cả các facilities hoặc tất cả các levels.

Một số ứng dụng quan trọng và các thư mục file log của chúng

|Ứng dụng |Thư mục |
|---------|---------|
|httpd |/var/log/http|
|samba |/var/log/samba|
|cron |/var/log/ |
|mail |/var/log/ |
|mysql | /var/log/ |

Đối với các file ghi log các bạn có thể dùng một số lệnh sau để giúp cho việc xem log

|Câu lệnh | Cú pháp |Ý nghĩa | Ghi chú thêm |
|---------|---------|--------|--------------|
|more | more [file] | Dùng xem toàn bộ nội dung của thư muc | Đối với câu lênh này nôi dung được xem theo từng trang. Bạn dung dấu "cách" để chuyển  trang |
|tail | tail [file] | In ra 10 dòng cuối cùng nội dung của file | thêm tùy chọn -n [số dòng] sẽ in ra số dòng theo yêu cầu |
|head | head [file] | In ra 10 dòng đầu tiên của nôi dụng file |
|tail -f | tail -f [file] | Dùng để xem ngay lâp tức khi có log đến | Đây là câu lệnh dùng phổ biến nhất nó giúp ta có thể xem ngay lập tức log mới đến, và nó sẽ in ra 10 dong cuối cùng trong nội dung file đó |

Hai cách ghi log cơ bản :
- [program] -> [log file]: 
	Chương trình đẩy log trực tiếp ra file nào mà nó muốn ( thường viết shell script )
- [program] ->syslog -> [log file]: 
	Chương trình đẩy log thồng qua dịch vụ syslog hệ thống để phân loại và chỉ định hành động.

###Định dạng chung của một gói tin syslog.
Định dạng hoàn chỉnh của một thông báo syslog gồm 3 phần chính như sau

<PRI> HEARDER MSG

Độ dài của một thông báo không được vượt quá 1024 bytes

####PRI
Phần này là một số 8 bit đặt trong ngoặc nhọn, 3 bit đầu thể hiện cho tính nghiêm trọng của thông báo, 5 bit còn lại đại diện cho cơ sở sinh ra thông báo.

Giá trị Priority được tính như sau: Cơ sở sinh ra log x 8 + Mức độ nghiêm trọng. Ví dụ, thông báo từ kernel (Facility = 0) với mức độ nghiêm trọng (Severity =0) thì giá trị Priority = 0x8 +0 = 0. Trường hợp khác,với "local use 4" (Facility =20) mức độ nghiêm trọng (Severity =5) thì số Priority là 20 x 8 + 5 = 165.

Vậy biết một số Priority thì làm thế nào để biết nguồn sinh log và mức độ nghiêm trọng của nó. Ta xét 1 ví dụ sau:

Priority = 191 Lấy 191:8 = 23.875 -> Facility = 23 ("local 7") -> Severity = 191 - (23 * 8 ) = 7 (debug)

#### HEADER
Gồm các phần chính sau:
+ time stamp: thời gian mà thông báo được tạo ra. Thời gian này được lấy từ thời gian hệ thống (Chú ý nếu như thời gian của server và client khác nhau thì thông báo ghi trên log được gửi lên server là thời gian của client)
+ hostname hoặc ip

#### MESSAGA 
Phần MSG chứa các thông tin về quá trình tạo ra thông điệp đó. Gồm hai phần chính là: tag field và content field.

Tag field là tên chương trình tạo ra thông báo. Content field chứa các chi tiết của thông báo

###Chi tiết file cấu hình của rsyslog

Trong UBUNTU file cấu hình là /etc/rsyslog.conf nhưng các rule được định nghĩa riêng trong /etc/rsyslog.d/50-defaul.conf . File rule này được khai báo include từ file cấu hình /etc/rsyslog.conf

##3. rsyslog
Rsyslog - "The rocket-fast system for log processing" được bắt đầu phát triển từ năm 2004 bởi Rainer Gerhards rsyslog là một phần mềm mã nguồn mở sử dụng trên Linux dùng để chuyển tiếp các log message đến một địa chỉ trên mạng (log receiver, log server) Nó thực hiện giao thức syslog cơ bản, đặc biệt là sử dụng TCP cho việc truyền tải log từ client tới server. Hiện nay rsyslog là phần mềm được cài đặt sẵn trên hầu hết hệ thống Unix và các bản phân phối của Linux như : Fedora, openSUSE, Debian, Ubuntu, Red Hat Enterprise Linux, FreeBSD…

##4. Sử dụng ELK để thu thập log
Dữ liệu gửi về cho Logstash; Logstash tiếp nhận và phân tích dữ liệu. Sau đó dữ liệu được gửi vào Elasticsearch; Elasticsearch nhận dữ liệu từ Logstash và lưu trữ, đánh chỉ mục; Kibana sử dụng các dữ liệu trong Elasticsearch để hiển thị và phân tích cú pháp tìm kiếm mà người dùng nhập vào để gửi cho Elasticsearch tìm kiếm.

Logstash: Là một phần mềm mã nguồn mở dùng để thu thập log 
- Nhận dữ liệu từ các beats, tiến hành phân tích dữ liệu
- Phân tích dữ liệu gửi từ filebeat bằng grok(filebeat: đọc file và lưu vị trí cuối cùng, khi có dữ liệu mới sẽ đọc tiếp và gửi)
- Grok là một dạng khai báo pattern sử dụng regular expression
- Grok đã được khai báo rất nhiều pattern có sẵn, bạn có thể sử dụng ngay.
- Nếu bạn có các loại dữ liệu đặc thù thì có thể dựa vào regular expression để khai báo các pattern theo yêu cầu
- Các dữ liệu packetbeat, topbeat thì có các template sẵn nên không cần khai báo filter, được logstach gửi thẳng vào Elasticsearch
- Dữ liệu được gửi sang cho Elasticsearch để lưu trữ

Elasticsearch: 
- Lưu trữ, đánh chỉ mục dữ liệu và tìm kiếm tất cả các log
- Sử dụng các template để lưu trữ dữ liệu
- Có thể cấu hình cluster, shard, replica để tăng tính an toàn, tính sẵn sàng, tăng hiệu năng đánh chỉ mục, tăng hiệu năng tìm kiếm dữ liệu
- Hỗ trợ tìm kiếm full keyword

Logstash và Elasticsearch được viết bằng java nên cần cài java trên máy chủ

Kibana: 
- Là một web interface dùng để hiện thị log
- Hiển thị dữ liệu theo thời gian thực 
- Hỗ trợ tìm kiếm dữ liệu theo nhiều kiểu
- Hiển thị dữ liệu theo nhiều dạng biểu đồ




