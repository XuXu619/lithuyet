## Tìm hiểu hoạt động của Github
> Tài liệu tham khảo: https://github.com/hocchudong/git-github-for-sysadmin
>
> Người thực hiện: Xu Xu
>
> Thực hiện lần cuối: **27/02/2018**

### Menu
[1. Tìm hiểu cách thức hoạt động của Github](#timhieu)

[2. Hiểu được các khái niệm](#khainiem)
 
[3. Tìm hiểu các bước để Setting up Git, Generate and add SSH key, cahing-your Github password in Git](#SettingupGit)	

[4. Tạo repository, thực hiện Clone repo đó về máy tính rồi tạo thư mục, tạo file báo cáo "README.md" sau đó thực hiện Commit và Push lên ](#repository)



<a name= "Demo"></a>

### 1. Tìm hiểu cách thức hoạt động của Github 

Github là một trang web, cho phép bạn lưu source code của mình lên đó. Dự kết hợp hoàn hảo giữa Git và Github mang lại sự thuận tiện không hề nhỏ cho người dùng.

Bạn có thể thay đổi đạon code của mình mọi lúc mọi nơi mà không sợ bị ghi đè lên hay bị mất dữ liệu do hỏng hóc dữ liệu vì dữ liệu của bạn được lưu trên trang web Github và máy cá nhân. Bạn cũng có thể phục hồi được code của mình về một thời điểm bất kì nào đó.

Github có bản free và mất phí, bạn phải trả phí nếu muốn tạo một kho code bí mật của mình. Ngược lại, bạn có thể thoải mái sử dụng và không phải lo về vấn đề chi phí.

Vậy làm thế nào để có thể sử dụng github? Chắc chắn rồi, bạn phải tạo một acc. Tiếp đến bạn nen học cách sử dụng ngôn ngữ markdown, vì nó sẽ mang lại sự tường minh cho bài viết của bạn. Tạo một repo tùy mục đích, clone về client và code.

Đó là tất cả những gì bạn cần làm.

<a name="Cackhainiem"></a>

### 2.Tìm hiểu các khái niệm 

- Tương ứng với 3 vị trí này ta có các hành động:
<ul>

<li>Add: lưu file thay đổi (mang tính cục bộ) - tương ứng với câu lệnh `git add`</li> 

<li>Commit: Ghi lại trạng thái thay đổi tại máy local (ví dụ như bạn có thể ấn Save nhiều lần với file README.md nhưng chỉ khi commit thì trạng thái của lần ấn Save cuối cùng trước đó mới được lưu lại) - tương ứng với câu lệnh `git commit`</li>

<li>Push: Đẩy những thay đổi từ máy trạm lên server - tương đương lệnh `git push`</li>

<li>Pull: đồng bộ trạng thái từ server về máy trạm - tương đương lệnh `git pull`</li> 
</ul>

### 3.Tìm hiểu các bước để Setting up Git, Generate and add SSH, cahing-your Github password in Git

- Setting up
<ul>
<li> Linux</li>
</ul> 

`apt-get install git`

- Thiết lập ban đầu:
<ul>

Bạn cần thiêt lập vào email của mình để mỗi khi commit lên server sẽ nhận biết được ai commit lên vì một repo có thể có nhiều người tham gia.

`git config --global user.name"Xu Xu"` 

`git config --global user.email Khietvu619@gmailcom` 


- Lựa chọn trình soạn thảo mặc định có thể là vi, vim, nano, ...
<ul>
  

`git config --global core.editor vi` 


- Liệt kê các thiết lập:
<ul>
 

`git config --list`

- Liên kết với tài khoản github bằng SSH
<ul>

`ssh-keygen =t rsa`

*Nếu bạn nhập pass thì hãy nhớ pass này!*

Kết quả: 

`Is !/.ssh/`

`id_rsa	id_rsa.pub	known_hosts`

- add SSH
<ul>

    ssh-agent -s

    ssh-add ~/.ssh/id_rsa

    cat ~/.ssh/id_rsa.pub

- copy đoạn mã này
<ul>

Truy cập đường link sau (Đảm bảo bạn đã đăng nhập vào github), chọn add SSh key, đặt tên cho key này tại ` Title` và pastse nội dung vừa copty vào ô `key`

<img src="https://imgur.com/a/cCxPI">

- Lúc này bạn có thể commit lên github tại máy local mà không nhập username và password.
<ul>

### 4. Tạo repository, thực hiện Clone repo đó về máy tính rồi tạo thư mục, tạo file báo cáo "README.md" sau đó thực hiện Commit và Push lên

*Tạo repository*

Tạo một repo mới trên trang github.com

<img src="https://imgur.com/a/LzA1Z">
<img src="https://camo.githubusercontent.com/33ade8c1712f9e55e9f9a91ee454b365204fb68c/687474703a2f2f692e696d6775722e636f6d2f4d4a5a6a594d6d2e706e67">


*clone repo*

`linux`

SSH: `git clone git@github.com:XuXu619/demo1.git /opt/demo` để clone vào thư mục/opt/demo

Để lấy link SSh, HTTPS này ta làm như sau: Click vào các hyperlink HTTPS hoặc SSH click Copy to clipboard

<img src="https://imgur.com/a/00Thb">

Ở đây sử dụng lệnh git clone `git@github.com:XuXu619/demo1.git`

Lúc này trong thư mục hiện tại sẽ có thêm thư mục demo1 chưa các file trong repo trên github.

Chuyển vào thư mục này 
`cd demo1`

`Is`

Lúc này sẽ thấy trong thư mục này cso fie `README.md`. Để sửa file này ta có thể sử dụng bất cứ trình soạn thảo nào, chẳng hạn vi, nano, getdit, ...

`vi README.md`

- Thêm vào nội dung như sau:
<ul>

`Hello word!`

Tạo một hư mục mới, chẳng hạn tên là script để chưa các script của tôi.

`mkdir script`

Tạo một script mới trong thư mục đó.

`vi script/script1.sh`

!/bin/sh
echo "Hello Python Vietnam"
sleep

bằng c tương tự các bạn có thể tạo thêm nhiều thư mục. file hướng dẫn. cấu hình, script, ... tùy ý

*commit, push*

Để thực hiện hành động commit file README.md ta thực hiện lệnh

`git commit README.md để add file README.md`

Hoặc `git commit *` để commit tất cả.

ta nên thêm tham só -m đẻ ghi lại một comment cho hành động đó

`git commit README.md =m"XuXu619 sua doi"`

Lúc này thay đổi của bạn đã lưu lại trên máy cục bộ. Để đồng bộ lên server Github ta thực hiện lệnh:

`git push origin master`

=> Nhập passphrase (nếu bạn đặt passphrare ở mục trên) với phương pháp clone ssh hoặc nhập username, passwword nếu clone bằng https

<img src="https://imgur.com/a/00Thb">

<img src="https://imgur.com/a/a5v7o">

- Một cách khác nếu bạn không muốn thực hiện clone về máy như bước trên thì bạn có thể làm như sau:
<ul>

	Tạo một repo mới trên github.com mà không tạo gile README.md( giả sử ở đây là repo demo2)
	Tại máy local tạo một thư mục để chưa repo mới này. Ví dụ:

`mkdir/opt/demo2`
`cd /opt/demo2`

	Thực hiện tạo các file, thư mục như ý muốn. Sau đó thực hiện add, commit, push. Tham khảo ví dụ: 

`vi README.md`
`git add README.md`
`git commit README.md`

- Hoặc `git commit README.md =m noi dung` 
<ul>

`git remote add origin git@github.com:XuXu619/demo2.git`

`git push origin master`

Sau đó nhập passphrase ( nếu cần) hoặc username + passwword ( nếu sử dụng SSH)

