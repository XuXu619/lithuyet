## Nhập môn C( tiếp theo)

> Tài liệu tham khảo: 
- http://bit.ly/2dwYc2u 
- http://bit.ly/1DPNtpD
- http://bit.ly/1FkfILU
> 
> Người thực hiện: Xu Xu
>
> Cập nhật lần cuối: **28/02/2018**
>

### Mục lục

[1. Tổng quan về quá trình cấp phát và giải phóng bộ nhớ](#Tongquan)

-	[1.1 Dẫn nhập](#dannhap)

-	[1.2 Cấp phát bộ nhớ](#capphat)

-	[1.3 Giải phóng bộ nhớ](#giaiphong)

-	[1.4 Memory leak](#leak)

- 	[1.5 Cấp phát bộ nhớ cho con trỏ](#tt)

-		[1.5.1 Tổ chức bộ nhớ trong máy tính](#tochuc)

-		[1.5.2 Tại sao cần phải cấp phát bộ nhớ cho con trỏ?](#Capphatbonhochocontro)

-		[1.5.3 Trong C có mấy cơ chế cấp phát bộ nhớ?](#coche)

-		[1.5.4 Tại sao cần phải giải phóng bộ nhớ?](#why)
[2. Tìm hiểu bản chất con trỏ từ cơ bản tới nâng cao](#contro)

-	[2.1 Bộ nhớ](#memory)

-	[2.2 Tổng quan](#tongquan)

-	[2.3 Khai báo](#khaibao)

-	[2.4 Khởi tạo](#khoitao)


[3. Viết báo cáo chi tiết các phần tìm hiểu](#baocao)

[4. Bài tập](#baitap)



<a name="Capphatvagiaiphonggbonho"></a>

### 1. Tổng quan về quá trình cấp phát và giải phóng bộ nhớ


#### 1.1 Dẫn nhập 

- Bộ nhớ là một tài nguyên quan trọng trong hệ thóng, được phân phối, điều phối phức tạp.</ul>

#### 1.2 Cấp phát bộ nhớ 

- Bộ  nhớ quan trọng nên cần thận trọng khi sử dụng, rất nhiều tiến trình cần tới bộ nhớ.
</ul>

Cấp phát bộ nhớ bản chất là `Trao quyền sử dụng`

	Khi ta xin cấp phát xong thì vùng nhớ đó hoàn toàn có giá trị ngẫu nhiên

	Khi ta cấp phát đi cùng với ép buộc sẽ được vùng nhớ có giá trị như mong muốn

#### 1.3 Giải phóng bộ nhớ

- Giải bộ nhớ bản chất là trả `Trả lại quyền sử dụng`</ul> 

Trả  lại thì đương nhiên khổ chủ sẽ không được phép sử dụng hay can thiệp vào bộ nhớ. 

#### 1.4 Memory leak 

- Bộ nhớ bị rò rỉ khi người dùng không ` Trả lại quyền sử dụng ` Nhiều lần như thế, hệ thống sẽ không còn bộ nhớ để cấp phát.</ul>

#### 1.5 Cấp phát bộ nhớ cho con trỏ 

##### 1.5.1 Tổ chức bộ nhớ trong máy tính

Trước hết hãy tìm hiểu bộ nhớ trong máy tính được tổ chức thế nào. Dưới đây là hình ảnh minh họa cho thứ tự các phân vùng trên bộ nhớ ảo:

<img src="http://minhhn.com/wp-content/uploads/2017/07/To_Chuc_Bo_Nho_May_Tinh_minhhn.com_.png">

- COde Segment </ul>

Code segement (text segement): Là nơi lưu trữ mã máy dạng nhị phân. Có nghĩa là các chương trình mà chúng ta code là code trên ngôn  ngữ tự nhiên, nhưng khi ở phân vùng này nó sẽ ở dạng mã máy nhị phân. Code segement chỉ chịu sự chi phối của hệ điều hành, người lập trình không thể can thiệp trực tiếp đến phân vùng này.

- Data Segement </ul>

Data segement (intialized data segement): là nơi chứa các biến kiểu static, biến toàn cục (global variable)

- BSS Segement </ul>

BSS Segement (uninitialixed data segement) cũng được dùng để lưu trữ các kiểu biến static, biến toàn cục (global variable) nhưng chưa được khởi tạo giá trị cụ thể.

- Heap </ul>

Là vùng nhớ không do CPU quản lí, người lập trình phải tự quản lí vùng nhớ này. Nó được sử dụng khi thực hiện cấp phát bộ nhớ động dùng cho con trỏ.

- Stack

Call stack là vùng nhớ do CPU quản lí, người lập trình không thể can thiệp vào vùng nhớ này. Nếu cố tình can thiệp sẽ bị lỗi ( code bên dưới, bạn chạy thử xem nó hiện thông báo lỗi như thế nào).
Vùng nhớ Stack được dùng để cấp phát bộ nhớ cho tham số của các hàm (function parameters) và biến cục bộ (local variables).

<img src="http://minhhn.com/lap-trinh-c/cap-phat-va-giai-phong-bo-nho-trong-lap-trinh-c/">

##### 1.5.2 Tại sao cần cấp phát bộ nhớ cho con trỏ

- Vì nếu không cấp ph bộ nhớ cho con trỏ thì chúng ta *không thể nhập dữ liệu trực tiếp cho con trỏ  * </ul>

Có thể hiểu khi muốn chỉ cho con trỏ,không có bộ nhớ thì không có chỗ để nhập dữ liệu.

Trong trường hợp ta chỉ cho con trỏ, con trỏ đến ô nhớ của biến khác n không cần phải cấp phát bộ nhớ nữa. vì khi đó con trỏ đã có ô nhớ để nhập d liệu cho nó rồi.ên


##### 1.5.3 Trong C có mấy cơ chế cấp phát bộ nhớ

Trong C có 3 cơ chế cấp phát:

	malloc
	calloc
	realloc

Cơ chế malloc và calloc:

| Mô tả | Cơ chế malloc | Cơ chế calloc |
| Cú pháp| void* malloc(size_t size);| void* calloc(size_t num, size_t s)|
|Ý nghĩa | Cấp phst một vùng nhớ có kích thước là size | Cấp phát một vùng nhớ chứa đủ num phần tử, mỗi phần tử có kích thước size|
| Tham số | 1 | 2 |
|Kết quả trả về | Con trỏ sẽ trỏ tới vùng nhớ vừa cấp phát nếu thành công, con trỏ null nếu cấp phát thất bại|
| Giá trị khởi tạo| Giá trị rác | Giá trị khởi tạo bằng 0|
| Sử dụng | Khi sr dụng phải tính toán (Tổng số ) kích thước vùng nhớ cần cấp phát trước rồi truyền vào cho malloc | Khi sử dụng calloc chỉ cần truyền vào số phần tử và kích thước một phần tử, thì calloc sẽ tự động tính toán và cấp phát vùng nhớ cần thiết|
|Ví dụ | Cấp phát mảng 10 phần tử kiểu int|
|Cách viết | int*a=(int*)malloc(10*sizeof*( int ));|int*a=(int*)calloc( 10, sizeof(int));


*Lưu ý: Vì giá trị khởi tạo cho vùng nhớ sau khi cấp phát thành công của hai cơ chế malloc và calloc là khác nhau. Do đó, tùy vào mục tiêu sử dụng mà dùng cơ chế phù hợp. Nếu bạn không quan tâm đến giá mặc định của vùng nhớ được cấp thì dùng malloc còn nếu muốn tất cả giá trị của bộ ô nhớ sau khi được cấp là 0 thì dùng calloc.*

	Khai báo cấp hoạt động bằng cơ chế malloc
	int *a = ( int* )calloc(10, sizeof(int));
	Tương đương set giá trị mặc định cho toàn bộ o nhớ sau khi cấp phát bằng 0
	memset (a, 0, 10 * sizeof( int ));


#### 1.5.4 Tại sao cần giải phóng bộ nhớ?

- Vì bộ nhớ khi cấp phát cho con trỏ thuộc vùng nhớ HEAP (là vùng nhớ CPU không quản lý. người lập trình phải tự quản lí vùng nhớ này) nếu như chúng ta không giải phóng thì những ô nhớ đấy sẽ không bao giờ được giải phóng, do đó có thể đên một lúc nào đó sẽ xảy ra tình trạng tràn bộ nhớ, deeanx đến máy bị đứng bị treo máy. Maý tính bây giờ cấu hình mạnh nên ít gặp. Tuy nhiên bộ nhớ có hạng, hãy tiết kiệm vì chính bạn, sử dụng xong thì nên giải phóng. </ul>

- Vậy có nghĩa là khi giải phóng bộ nhớ thì giá trị của con trỏ đang có sẽ bị mất? Điều này không đúng. Nếu như ngay khi chúng ta khai báo giải phóng mà có một tiến trình khác chiếm hữu ô nhớ đó thì giá trị hiện tại trên ô nhớ đó sẽ không còn nữa, còn nếu như không có tiến trình thì giá trị vẫn còn. </ul>

	Bản chất giải phsng bộ nhớ là thông báo chương trình biết là vùng nhớ đã sử dụng xong rồi không còn dùng nữa, hệ điều hành có thể sử dụng nó cho một tiến trình khác. Còn việc giá trị của vùng nhớ sau khi được giải phóng còn hau không thì chúng ta không biết được.

### 2 Tìm hiểu bản chất của con trỏ từ cơ bản tới nâng cao

(#memory)

1. Bộ nhớ ảo là gì?

Virtual memory là một kĩ thuật cho phép một chương trình ứng dụng tưởng rằng mình đang có một bộ nhớ liên tục (Một không gian địa chỉ) trong khi bô nhớ này có thể bị phân mảnh trong bộ nhớ vật lí và thâm chí có thể lưu trữ cả trong đĩa cứng. SO với các hệ thống không dùng kĩ thuật bộ nhớ ảo. các hệ thống dùng kĩ thuật này cho phép các lập trình các ứng dung lớn được dễ dàng hơn và sử dụng bộ nhớ vật lí thực ( ví dụ RAM) hiệu quả hơn.

Lưu ý rằng khái niệm virtual memory khong chỉ có nghĩa sử dụng không gian để mở rộng kích thước bộ nhớ vật lý nghĩa là chỉ mở  rộng hệ thống bộ nhớ để bao gồm cả đĩa cứng. Viêc mở rộng bộ nhớ tới các ổ đĩa hỉ là một hệ quả thông thường của việc sử dụng các kĩ thuật của bộ nhớ ảo. Trong khi đó, việc mở rộng này có thể được thực hiện bằng các phwong pháp khác như kĩ thuật overlay hoặc chuyển toàn bộ các chương trình cùng dữ liệu của chúng ra khỏi bộ nhớ khi các chương trình này không ở trạng thái họat động. Định nghĩa của bộ nhớ ảo có nên tảng là không gian địa chỉ bằng một dải liên tục các địa chỉ bộ nhớ ảo để đánh lừa các chương trình là chúng đang dùng khối lớn các địa chỉ liên tục.

2. Địa chỉ ảo là gì? 

Trong vùng bộ nhớ ảo kia, để cho tiến trình dễ sử dụng hệ điều hành dễ hiểu, hai th này cùng nhau quy định rằng, chỉ nhỏ ra theo từng byte , và đánh số từ một đến hết.

	Mỗi tiến trình có một vùng nhớ ảo riêng
	Vùng nhớ ảo là một không gian địa chỉ ảo trải dài từ thấp đến cao (từ 0 x 0000 -> cao hơn)
	Ở trong win 32bit thì không gian địa chỉ ảo có địa chỉ từ 00000000h trải đến 7fffffffh
	Bạn cần hiểu nó là địa chỉ ảo, không phải vùng nào cũng có bộ nhớ vật lý thật đâu nhé.
	Khái niệm về bộ nhớ phân đoạn: segement offset bạn hãy bỏ qua đi nó quá cũ rồi.





#### 2.2 Tổng quan

1. Cái nhìn vấn đề 

Con trỏ chỉ là một biến nguyên bình thường, chưa cái mà được gọi là địa chỉ ảo. 

Trong win 32bit thì địa chỉ ảo có độ dài 32 bit, tương ứng với số haxa có 8 chữ số, nó chỉ có 32bit vì chừng đó là vừa đủ để chỉ trỏ hết vùng bộ nhớ ảo.

2. Con trỏ ... là gì? 

Với một đứa nhập môn như tôi thì khái niệm này khá gay go đấy. 

Cứ note một cái quan trọng, cực kì quan trọng (người ta nói): Con trỏ là một công cụ, là một kiểu dữ kiệu để ta cài đặt các giải thuật.


#### 2.3 Khai báo 

1. Cấu trúc khai báo

- Kiểu dữ liệu ở đây có thể là
<ul>
<li> Kiểu dữ liệu có sẵn (built-in data type): int, char, double, long, ....</li>
<li> Kiểu dữ liệu cấu trúc do người dùng định nghĩa (user-denied data type): struct, union</li>
<li> Kiểu dữ liệu dẫn xuất + kiểu con trỏ hàm (các chap adv nhé) </ul>

Kiểu dữ liệu của cái vùng nhớ mà nó trỏ đến

Khai báo biến con trỏ: Tùy thuộc kiểu dữ liệu ta có tương ứng một biến con trỏ kiểu đó.

Kiểu* Tên biến con trỏ;

`Quy đính vùng trỏ tới của con tr`

Ta dùng toán tử và đẻ lấy địa chỉ của một biến con trỏ và sau đó gán địa chỉ đó cho biến con trỏ.

	Tên con trỏ = &biến;

Ex: p=&a 

`Cách truy xuất`

Với con trỏ px bên trên ta có 2 phép truy xuất là:

	px: lấy địa chỉ mà nó lưu giữ (trỏ tới)
	*px: lấy giá trị trong vùng nhớ mà nó trỏ tới
Trong ví dụ lên ta có thể thấy sau phép gán px = &x thì ta biết:

	px sẽ tương đương với &x
	*px sẽ tương đương với x và ta có thể sử dụng *px trong các phép toán, biểu thức.

#### 2.4 Khởi tạo

1. Khởi tạo là gì?

khởi tạo hoàn toàn khác với khai báo, khi ta khai báo thì câu lệnh đầu tiên thiết lập giá trị cho biến đó thì đó là khởi tạo. 

2. Khởi tạo trong biến con trỏ
	Tên con trỏ= địa chỉ;

Trong đó tên con trỏ là tên của biến con trỏ
Địa chỉ là vùng địa chỉ mà ta muốn trỏ đến
 ` Bản thân p cũng là một biến nguyên, p cũng nằm trong bộ nhớ, cũng có địa chỉ riêng`
`Toán tử & ở đây chính xác phải gọi là unary operator &  toán tử & một ngôi, nó hoàn toàn khác với toán tử ^2 ngôi (bitwwise). Toán tử & ngôi hay dùng để lấy địa chỉ của một biến. Khi đông đến lú thuyết về con trỏ ta đã sử dụng toán tử này rồi đó: scanf("%d",&a)`

3. Có được gì sau khi khởi tạo như ví dụ trên

Khi giá trị nằm trong p là địa chỉ của a thì ta nói p trỏ vào a

Lúc này thì *p hoàn toàn tương đương với a, người ta coi *p là bí danh của a, thao tác với *p cũng như thao tác với a, thao tác với a cũng như thao tác với *p

`Lúc này lệnh scanf("%d", &a); có thể thay bằng scanf("%d",p);`


*Chú ý toán tử "*" * đây là toán tử một ngôi, tác dụng là truy xuất đến ô nhớ mà con trỏ đang trỏ đến
Để tránh những hiểu lầm không đáng có khi có sự đang nhập mà bạn không thể đoán được, hãy thêm cặp () (*p)++ 
a+(*p)*c // thêm vào cho nó sáng sủa code ra.








