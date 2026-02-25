# ** Tìm hiểu các params của các component **

### **Cơ chế Đánh số và Trích xuất dữ liệu (Cực kỳ quan trọng)**

Khi bạn xây dựng một kịch bản phức tạp với nhiều bước giống nhau (ví dụ: dùng 2
nút Gemini để xử lý văn bản ở các bước khác nhau), NovaCore cung cấp một hệ
thống **Định danh duy nhất** tự động:

1. **Đánh số trên Canvas**: Ngay khi bạn kéo một nút thứ 2 cùng loại vào, hệ
   thống sẽ tự đặt tên là `[Tên Node] 2`. Việc này giúp bạn phân biệt rõ ràng
   các bước ngay trên màn hình.
2. **Đánh số trong Suggested Values**: Trong bảng gợi ý giá trị (hiện ra khi bạn
   nhấn vào biểu tượng biến), danh sách sẽ hiển thị `Node 1`, `Node 2`... để bạn
   chọn chính xác kết quả của từng bước.
3. **Cú pháp trích xuất**:
   - Sử dụng: `{{ $node["Tên Node 1"].output }}` để lấy kết quả của nút đầu
     tiên.
   - Sử dụng: `{{ $node["Tên Node 2"].output }}` để lấy kết quả của nút thứ hai.
   - _Mẹo_: Bạn cũng có thể dùng `$node` (số ít) hoặc `$nodes` (số nhiều) đều
     được. Hệ thống có cơ chế bảo vệ thông minh, nếu bạn gõ sai tên nút, nó sẽ
     không làm treo kịch bản mà sẽ báo lỗi rõ ràng trong Log để bạn sửa.

---
## **\- New tab:**

### **1\. URL (Đường dẫn)**

* **Mô tả:** Địa chỉ trang web bạn muốn mở ở tab mới.
* **Giá trị:** Có thể nhập trực tiếp (ví dụ: `https://google.com`) hoặc truyền biến (ví dụ: `{{target_url}}`).
* **Lưu ý:** Nếu để trống, trình duyệt thường sẽ mở một trang trắng (`about:blank`).

### **2\. Set Active (Kích hoạt Tab)**

* **Mô tả:** Quyết định xem trình duyệt có nhảy sang tab mới mở này để thao tác ngay lập tức hay không.
* **Lựa chọn:** \* `True` (Bật): Trình duyệt sẽ chuyển tiêu điểm (focus) sang tab mới. Các lệnh click, type phía sau sẽ tác động lên tab này.
  * `False` (Tắt): Tab mới sẽ mở dưới nền (background). Trình duyệt vẫn ở lại tab cũ.

### **3\. Wait Until (Trạng thái chờ tải)**

Thông số này quyết định khi nào thì component này được coi là "hoàn thành" để chạy lệnh tiếp theo:

* **Load:** Chờ đến khi toàn bộ trang web (bao gồm ảnh, script, css) tải xong hoàn toàn.
* **DOM Content Loaded:** Chỉ chờ mã nguồn HTML tải xong (thường nhanh hơn `Load`).
* **Network Idle 0:** Chờ đến khi không còn bất kỳ yêu cầu mạng (request) nào được gửi đi trong vòng 500ms (dùng cho các trang web nặng về Java Script/Ajax).
* **Network Idle 2:** Tương tự như trên nhưng cho phép tối đa 2 request đang chạy (phù hợp với các trang có script theo dõi/tracking chạy liên tục).

### **4\. Timeout (Thời gian chờ tối đa)**

* **Mô tả:** Thời gian tối đa (tính bằng miligiây \- ms) mà hệ thống chờ trang web tải theo điều kiện ở mục *Wait Until*.
* **Giá trị mặc định:** Thường là `30000` (30 giây). Nếu quá thời gian này trang chưa tải xong, kịch bản sẽ báo lỗi (Fail).

## **\- Activate tab:**

### **1\. Match Type (Loại đối chiếu)**

Đây là thông số để hệ thống biết bạn muốn tìm tab nào để quay lại. Thông thường có các tùy chọn:

* **Index (Chỉ số):** Chọn tab theo thứ tự mở (0 là tab đầu tiên, 1 là tab thứ hai, và cứ thế tiếp tục).
* **URL:** Chọn tab dựa trên địa chỉ trang web đang mở.
* **Title:** Chọn tab dựa trên tiêu đề (thẻ `<title>`) của trang web đó.

### **2\. Value (Giá trị đối chiếu)**

Tương ứng với **Match Type** bạn chọn ở trên:

* Nếu chọn **Index**: Nhập số (ví dụ: `0`, `1`). Bạn có thể dùng biến `{{tab_count}}`.
* Nếu chọn **URL**: Nhập một phần hoặc toàn bộ URL (ví dụ: `facebook.com`). Hệ thống sẽ tìm tab nào có URL chứa chuỗi này.
* Nếu chọn **Title**: Nhập tiêu đề trang (ví dụ: `Giỏ hàng`).

### **3\. Match Pattern (Kiểu khớp dữ liệu)**

Chỉ xuất hiện khi bạn chọn Match Type là URL hoặc Title:

* **Exact:** Phải khớp từng chữ cái.
* **Contains:** Chỉ cần chứa từ khóa đó là được (Khuyên dùng vì URL thường có các ký tự theo dõi dài phía sau).
* **Regex:** Sử dụng biểu thức chính quy (dành cho người dùng nâng cao).

### **4\. Wait for Tab (Chờ đợi tab)**

* **Timeout (ms):** Thời gian tối đa (ví dụ: `5000` ms) để hệ thống tìm kiếm tab đó. Nếu sau 5 giây không thấy tab thỏa mãn điều kiện, kịch bản sẽ báo lỗi.

## **\- Open url**

### **1\. URL (Đường dẫn)**

* **Mô tả:** Địa chỉ chính xác của trang web bạn muốn truy cập.
* **Giá trị:** Bắt đầu bằng `http://` hoặc `https://`.
* **Mẹo:** Bạn có thể sử dụng biến số như `{{target_site}}` nếu muốn dùng một kịch bản cho nhiều website khác nhau.

### **2\. Wait Until (Điều kiện hoàn thành)**

Đây là param quan trọng nhất để tránh lỗi "phần tử chưa xuất hiện". Nó có các tùy chọn:

* **Load:** Đợi đến khi toàn bộ website, bao gồm cả hình ảnh nặng và script quảng cáo, được tải xong hoàn toàn.
* **DOM Content Loaded:** Chỉ đợi mã HTML được tải và phân tích xong. Thích hợp cho các kịch bản cần tốc độ nhanh và không quan tâm đến hình ảnh.
* **Network Idle 0:** Đợi cho đến khi không còn bất kỳ yêu cầu mạng nào được gửi đi trong ít nhất 500ms. Dùng cho các trang web Single Page Application (như React, Angular) thường xuyên load dữ liệu ngầm.
* **Network Idle 2:** Tương tự như trên nhưng cho phép tối đa 2 request đang chạy (phòng trường hợp trang web có các script theo dõi/analytics chạy vô tận).

### **3\. Referer (Trang giới thiệu)**

* **Mô tả:** Giả lập rằng bạn truy cập URL này từ một trang web khác (ví dụ: bạn vào `shopee.vn` nhưng giả lập là click từ `google.com`).
* **Ứng dụng:** Giúp tăng độ tin cậy của profile (Trust score), tránh bị đánh dấu là truy cập trực tiếp bằng công cụ automation.

### **4\. Timeout (Thời gian chờ tối đa)**

* **Đơn vị:** Miligiây (ms). Mặc định thường là `30000` (30 giây).
* **Tác dụng:** Nếu quá thời gian này mà trang web chưa thỏa mãn điều kiện ở mục **Wait Until**, bước này sẽ bị báo lỗi và dừng kịch bản.

### **5\. Ignore HTTP Errors**

* **Mô tả:** Một ô check-box (True/False).
* **Tác dụng:** Nếu bật, ngay cả khi trang web trả về lỗi 404 (Không tìm thấy) hoặc 500 (Lỗi server), kịch bản vẫn sẽ tiếp tục chạy các bước sau thay vì dừng lại báo lỗi.

## **\- Close tab**

### **1\. Match Type (Loại đối chiếu)**

Tham số này xác định tab nào sẽ bị đóng. Bạn có các lựa chọn tương tự như lệnh `Activate Tab`:

* **Current Tab:** Đóng tab hiện tại mà trình duyệt đang tập trung (focus). Đây là tùy chọn phổ biến nhất.
* **Index:** Đóng tab theo số thứ tự (0, 1, 2...).
* **URL:** Đóng tab có địa chỉ khớp với chuỗi ký tự bạn nhập.
* **Title:** Đóng tab có tiêu đề trang khớp với yêu cầu.

### **2\. Value (Giá trị)**

Chỉ xuất hiện nếu bạn chọn Match Type là **Index**, **URL**, hoặc **Title**:

* **Với Index:** Nhập số thứ tự tab muốn đóng.
* **Với URL/Title:** Nhập từ khóa (Ví dụ: `facebook.com` hoặc `Giỏ hàng`).

### **3\. Match Pattern (Kiểu khớp)**

Dành cho URL và Title:

* **Exact:** Khớp hoàn toàn 100%.
* **Contains:** Chỉ cần chứa cụm từ đó (Ví dụ: URL chứa `login` là đóng).
* **Regex:** Sử dụng biểu thức chính quy để lọc tab phức tạp.

### **4\. Close Others (Đóng các tab khác)**

* **Mô tả:** Đây là một tùy chọn (Checkbox) hoặc một mode riêng trong một số phiên bản.
* **Tác dụng:** Nếu kích hoạt, nó sẽ đóng tất cả các tab **trừ** tab hiện tại hoặc tab thỏa mãn điều kiện. Cực kỳ hữu ích để "dọn dẹp" sạch sẽ trình duyệt trước khi kết thúc kịch bản.

## **\- Go back:**

### 1\. Wait Until (Điều kiện hoàn thành)

Đây là param chính của component này. Sau khi trình duyệt quay lại trang trước đó, hệ thống cần biết khi nào thì trang đó được coi là "sẵn sàng" để chạy tiếp lệnh tiếp theo:

* Load: Đợi trang trước đó tải xong toàn bộ (ảnh, CSS, Scripts).
* DOM Content Loaded: Chỉ đợi cấu trúc HTML tải xong.
* Network Idle 0: Đợi đến khi không còn request mạng nào phát sinh (phù hợp cho các trang web hiện đại load dữ liệu bằng Ajax/API).
* Network Idle 2: Cho phép tối đa 2 request chạy ngầm (thường dùng để bỏ qua các script theo dõi của Google/Facebook).

### 2\. Timeout

* Giá trị: Tính bằng miligiây (ms), mặc định thường là 30000 (30 giây).
* Tác dụng: Nếu trong khoảng thời gian này mà trang cũ không thể tải lại thành công (do mạng lỗi hoặc trang không còn tồn tại trong bộ nhớ cache), bước này sẽ báo lỗi.
---

### Các lưu ý "Xương máu" khi dùng Go Back:

1. Trạng thái của Trang cũ: Khi bạn nhấn Go Back, trang web có thể không giữ
   nguyên trạng thái cũ (ví dụ: các ô đã nhập liệu có thể bị trống, hoặc danh
   sách sản phẩm bị load lại từ đầu). Bạn nên kết hợp với lệnh Wait For Element
   ngay sau đó để chắc chắn trang đã hiển thị đúng mục mong muốn.
2. Lỗi "Confirm Form Resubmission": Nếu trang trước đó là kết quả của một lệnh
   POST (như sau khi bấm nút Thanh toán hoặc Đăng ký), lệnh Go Back có thể làm
   trình duyệt hiện thông báo xác nhận gửi lại dữ liệu. Trong trường hợp này,
   lệnh Go Back tự động có thể bị kẹt.
3. Lịch sử duyệt web (History): Nếu profile mới mở và chỉ có 1 URL đầu tiên,
   lệnh Go Back sẽ không có tác dụng vì không có lịch sử trang trước đó.
4. Mô phỏng người thật: Thay vì dùng lệnh Go Back hệ thống, bạn có thể dùng
   component Click vào nút "Back" có sẵn trên giao diện của chính website đó
   (nếu có) để tăng độ "Trust" cho tài khoản.

## \- Reload page

### **1\. Wait Until (Điều kiện hoàn thành)**

Đây là tham số quan trọng nhất để hệ thống biết khi nào trang đã tải xong sau
khi làm mới để thực hiện bước tiếp theo:

- **Load:** Đợi toàn bộ tài nguyên (hình ảnh, iframe, scripts) tải xong 100%.
- **DOM Content Loaded:** Chỉ đợi cấu trúc HTML cơ bản tải xong. Dùng khi bạn
  chỉ cần tương tác với text hoặc các nút bấm đơn giản mà không cần đợi ảnh.
- **Network Idle 0:** Đợi cho đến khi không còn request mạng nào chạy ngầm trong
  500ms. Phù hợp cho các trang Dashboard hoặc sàn TMĐT có nhiều dữ liệu load
  động.
- **Network Idle 2:** Cho phép tối đa 2 request chạy ngầm (thường là các script
  quảng cáo hoặc tracking). Tùy chọn này giúp kịch bản chạy nhanh hơn vì không
  phải đợi các script vô tận của bên thứ ba.

### **2\. Timeout (Thời gian chờ tối đa)**

- **Giá trị:** Tính bằng miligiây (ms), thường để mặc định là `30000` (30 giây).
- **Tác dụng:** Nếu quá thời gian này mà trang web không phản hồi hoặc không đạt
  được trạng thái ở mục **Wait Until**, kịch bản sẽ báo lỗi "Timeout".

### **3\. Hard Reload (Tải lại cứng)**

- **Tác dụng:** Tích chọn checkbox này để trình duyệt xóa bộ nhớ đệm (cache)
  trước khi tải lại trang, đảm bảo dữ liệu mới nhất được cập nhật từ máy chủ.

## \- Click

Component **Click** là một trong những lệnh quan trọng nhất để tương tác với
website. Trong NovaCore, các tham số của nó không chỉ đơn thuần là bấm nút mà
còn giúp bạn mô phỏng hành vi của người dùng thật để tránh bị các hệ thống quét
Bot phát hiện.

Dưới đây là các params chi tiết của component này:

### **1\. Selector (Định danh phần tử)**

- **Mô tả:** Đây là tọa độ hoặc mã định danh của nút/phần tử bạn muốn click.
- **Giá trị:** NovaCore hỗ trợ các loại Selector sau:
  - **CSS Selector:** (Ví dụ: `button.btn-login` hoặc `#submit-id`).
  - **XPath:** (Ví dụ: `//div[@class='container']//button`). XPath thường mạnh
    mẽ hơn khi cấu trúc web phức tạp.
- **Mẹo:** Bạn có thể dùng công cụ "Pick" (hình kính lúp/mũi tên) hoặc nhấn
  **"Inspect Browser"** trên menu chính để trỏ trực tiếp vào web và lấy Selector
  tự động. Tính năng này sẽ tự động khởi chạy trình duyệt nếu hồ sơ của bạn chưa
  mở.

### **2\. Button (Nút chuột)**

- **Mô tả:** Chọn loại nút chuột muốn thực hiện.
- **Giá trị:**
  - `Left`: Chuột trái (mặc định, dùng cho hầu hết các nút).
  - `Right`: Chuột phải (dùng khi muốn mở menu ngữ cảnh).
  - `Middle`: Chuột giữa (thường dùng để mở link trong tab mới).

### **3\. Click Count (Số lần click)**

- **Mô tả:** Số lần thực hiện lệnh nhấn.
- **Giá trị:**
  - `1`: Click đơn.
  - `2`: Double click (nhấn đúp, thường dùng để chọn văn bản hoặc mở file trong
    các trình quản lý).

### **4\. Delay (Thời gian nhấn giữ)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Khoảng thời gian từ lúc chuột "nhấn xuống" đến lúc "thả ra".
- **Tác dụng:** Người thật thường nhấn giữ khoảng 50ms \- 150ms. Nếu bạn để là
  0ms, các hệ thống chống bot như Cloudflare có thể nhận diện là thao tác của
  máy.

### **5\. Simulate Real Mouse (Mô phỏng chuột thật)**

- **Trạng thái:** Bật/Tắt (Checkbox).
- **Tác dụng:** Nếu bật, thay vì con trỏ chuột "nhảy cóc" tức thì đến điểm
  click, hệ thống sẽ tạo ra một **quỹ đạo di chuyển (Human-like path)**. Con trỏ
  sẽ di chuyển theo đường cong với tốc độ thay đổi nhẹ, giống hệt cách con người
  di chuyển chuột.
- **Cài đặt nâng cao (⚙️):**
  - **Button:** Chọn chuột Trái, Phải hoặc Giữa.
  - **Click Count:** Số lần bấm (1: Click đơn, 2: Click đúp).
  - **Delay:** Độ trễ giữa lúc nhấn xuống và thả ra (ms).

### **6\. Offset X / Offset Y (Độ lệch tọa độ)**

- **Mô tả:** Điều chỉnh điểm click lệch đi một chút so với tâm của phần tử.
- **Ứng dụng:** Để tránh việc hàng nghìn profile đều click vào đúng 1 tọa độ
  pixel giống hệt nhau trên nút (một dấu hiệu của bot). Bạn có thể dùng biến
  ngẫu nhiên (Random) cho thông số này.

## \- Press & Hold

### **1\. Selector (Phần tử mục tiêu)**

- **Mô tả:** Đối tượng mà bạn muốn đặt con trỏ chuột lên để nhấn giữ.
- **Giá trị:** CSS Selector hoặc XPath.
- **Mẹo:** Nếu bạn dùng để giải Captcha kéo trượt, Selector này thường là **nút
  trượt (slider button)**.

### **2\. Button (Nút chuột)**

- **Giá trị:** Thường mặc định là `Left` (Chuột trái). Các trường hợp giữ chuột
  phải (`Right`) rất hiếm khi dùng trong automation web.

### **3\. Duration / Hold Time (Thời gian giữ)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Tổng thời gian con chuột sẽ ở trạng thái "đè xuống" trên phần tử
  đó.
- **Lưu ý:** Nếu bạn dùng lệnh này kết hợp với lệnh di chuyển chuột (Move Mouse)
  để kéo Captcha, tham số này phải đủ dài để bao quát toàn bộ quá trình di
  chuyển.

### **4\. Pressure / Force (Lực nhấn \- Tùy chọn ở một số bản nâng cao)**

- **Mô tả:** Một số hệ thống chống bot cao cấp có thể cảm nhận được áp lực nhấn
  (đặc biệt trên các thiết bị cảm ứng). Tham số này giúp giả lập độ mạnh nhẹ của
  ngón tay/chuột.

## \- Mouse movement

### **1\. Destination Type (Loại đích đến)**

Bạn cần chọn cách xác định điểm mà chuột sẽ di chuyển tới:

- **Selector:** Di chuyển đến một phần tử cụ thể (nút bấm, hình ảnh, ô nhập
  liệu).
- **Coordinates (Tọa độ):** Di chuyển đến một điểm cụ thể trên màn hình dựa trên
  trục X và Y (ví dụ: X: 500, Y: 300).

### **2\. Selector (Nếu chọn Destination là Selector)**

- **Mô tả:** CSS Selector hoặc XPath của phần tử.
- **Hành vi:** Chuột sẽ di chuyển vào vùng không gian của phần tử đó.

### **3\. Coordinate X / Y (Nếu chọn Destination là Coordinates)**

- **Mô tả:** Tọa độ pixel trên trang web.
- **Mẹo:** Bạn có thể sử dụng biến ngẫu nhiên `{{random_x}}` để chuột không luôn
  luôn di chuyển đến cùng một điểm chết.

### **4\. Speed (Tốc độ di chuyển)**

- **Đơn vị:** Thường là một dải giá trị hoặc mức độ (Slow, Normal, Fast).
- **Ứng dụng:** Người thật không bao giờ di chuyển chuột với tốc độ đều tắp từ
  đầu đến cuối. NovaCore sẽ tự động điều chỉnh tốc độ nhanh ở giữa và chậm lại
  khi gần đến đích (gia tốc).

### **5\. Steps / Points (Số điểm trung gian)**

- **Mô tả:** Số lượng các điểm mà con trỏ sẽ đi qua trước khi đến đích.
- **Tác dụng:** Càng nhiều điểm, quỹ đạo di chuyển càng mịn nhưng có thể làm
  kịch bản chạy chậm hơn. Thông số này giúp mô phỏng đường cong thay vì đường
  thẳng.

### **6\. Human-like Path (Quỹ đạo mô phỏng người)**

- **Trạng thái:** Bật/Tắt (Checkbox).
- **Cơ chế:** Khi bật, hệ thống sẽ tạo ra một đường di chuyển hơi cong và có độ
  lệch nhẹ (jitter). Con người không bao giờ kéo chuột theo một đường thẳng
  tuyệt đối như máy tính.

## \- Hold & Drag

### **1\. Source (Điểm bắt đầu)**

Tham số này xác định vật thể bạn muốn "túm" lấy để kéo đi.

- **Selector:** CSS Selector hoặc XPath của vật thể (ví dụ: nút trượt của
  Captcha).
- **Coordinates:** Tọa độ (X, Y) nếu vật thể không có selector cố định.
- **Offset X/Y:** Độ lệch so với tâm của vật thể bắt đầu (giúp click vào các vị
  trí khác nhau trên nút để tránh bị phát hiện là bot).

### **2\. Destination (Điểm kết thúc)**

Tham số này xác định nơi bạn muốn "thả" vật thể ra.

- **Selector:** Kéo đến một phần tử đích (ví dụ: kéo sản phẩm vào giỏ hàng).
- **Coordinates (X, Y):** Kéo đến một vị trí pixel cụ thể. Đối với Slider
  Captcha, thường bạn sẽ giữ nguyên trục Y và chỉ thay đổi trục X.
- **Relative Position:** Kéo theo một khoảng cách tương đối (ví dụ: kéo sang
  phải 200px so với vị trí hiện tại).

### **3\. Steps (Số bước di chuyển)**

- **Mô tả:** Số lượng điểm trung gian mà con trỏ chuột sẽ đi qua.
- **Tác dụng:** \* Nếu `Steps` thấp: Chuột di chuyển rất nhanh và thẳng (dễ bị
  phát hiện là bot).
  - Nếu `Steps` cao: Chuột di chuyển mịn hơn, chậm hơn. Để vượt Captcha, bạn nên
    để số bước này lớn và kết hợp với dải ngẫu nhiên.

### **4\. Duration (Thời gian thực hiện)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Tổng thời gian từ lúc bắt đầu nhấn đến lúc di chuyển tới đích.
- **Ứng dụng:** Người thật thường mất khoảng 500ms \- 1500ms để kéo một mảnh
  ghép. Nếu kéo quá nhanh (dưới 100ms), hệ thống sẽ đánh dấu là hành vi bất
  thường.

### **5\. Human-like Path / Jitter (Độ rung lắc)**

- **Mô tả:** Thêm các sai số nhỏ vào quỹ đạo di chuyển.
- **Tác dụng:** Con người khi kéo chuột không bao giờ đi theo một đường thẳng
  tắp 100%. Sẽ có những lúc hơi lệch lên hoặc lệch xuống một vài pixel.
- **Cài đặt nâng cao (⚙️):**
  - **Hold Duration:** Thời gian nhấn giữ vật thể trước khi bắt đầu kéo (ms).
    Giúp mô phỏng hành động "cầm chắc" vật thể của người thật.

## \- Scroll

### **1\. Scroll Type (Loại cuộn)**

Đây là tham số xác định cách thức bạn muốn di chuyển trang web:

- **Amount (Số lượng):** Cuộn theo một khoảng cách pixel nhất định.
- **To Element (Đến phần tử):** Cuộn cho đến khi một phần tử cụ thể (nút bấm,
  hình ảnh) xuất hiện trên khung hình.
- **To Bottom (Đến cuối trang):** Cuộn một mạch xuống tận dưới cùng của trang
  web.
- **To Top (Lên đầu trang):** Cuộn về vị trí 0, 0 của trang.

### **2\. Scroll Amount (X / Y)**

Chỉ xuất hiện khi bạn chọn loại cuộn là **Amount**:

- **X:** Cuộn theo chiều ngang (hiếm khi dùng).
- **Y:** Cuộn theo chiều dọc. Số dương (ví dụ: `500`) để cuộn xuống, số âm (ví
  dụ: `-500`) để cuộn lên.
- **Mẹo:** Để mô phỏng người thật, hãy sử dụng biến ngẫu nhiên
  `{{random_scroll}}` thay vì một con số cố định.

### **3\. Selector (Nếu chọn To Element)**

- **Mô tả:** Nhập CSS Selector hoặc XPath của phần tử đích.
- **Ứng dụng:** Rất hữu ích khi bạn muốn click một cái nút ở dưới cùng trang web
  nhưng nút đó chỉ xuất hiện khi bạn cuộn tới nó.

### **4\. Smooth Scroll (Cuộn mượt)**

- **Trạng thái:** Bật/Tắt (Checkbox).
- **Tác dụng:** \* **Bật (True):** Trình duyệt sẽ cuộn từ từ, có gia tốc giống
  như bạn dùng ngón tay vuốt hoặc lăn chuột. Điều này giúp tránh bị các hệ thống
  quét bot phát hiện vì cuộn quá nhanh.
  - **Tắt (False):** Trang web sẽ nhảy ngay lập tức đến vị trí đích (Instant
    jump).

### **5\. Delay (Thời gian nghỉ)**

- **Mô tả:** Thời gian chờ (ms) sau khi thực hiện lệnh cuộn xong.
- **Ứng dụng:** Sau khi cuộn xuống, bạn nên để nghỉ khoảng `1000ms - 2000ms` để
  trang web kịp load dữ liệu mới (ảnh, danh sách sản phẩm).

## \- Press key

### **1\. Key (Phím nhấn)**

- **Mô tả:** Xác định phím cụ thể mà bạn muốn trình duyệt thực hiện nhấn.
- **Giá trị:** \* **Các phím văn bản:** `a`, `b`, `1`, `2`, `@`,...
  - **Các phím chức năng:** `Enter`, `Tab`, `Escape`, `Backspace`, `ArrowUp`,
    `ArrowDown`, `Space`, `F1` \- `F12`.
- **Lưu ý:** Tên phím thường phải tuân theo chuẩn **KeyboardEvent Key Values**
  của Javascript.

### **2\. Modifier Keys (Phím bổ trợ)**

- **Mô tả:** Các phím đặc biệt được nhấn giữ cùng lúc với phím chính để tạo
  thành tổ hợp phím.
- **Sử dụng:** Tích chọn vào các ô checkbox tương ứng trong bảng thuộc tính của
  node:
  - `Control` (Ctrl)
  - `Shift`
  - `Alt`
  - `Meta` (Phím Windows hoặc Command trên Mac)
- **Ví dụ:** Để thực hiện tổ hợp phím **Ctrl + V** (Dán), bạn nhập phím `v` vào
  ô **Keyboard Key** và tích chọn ô **Control**.

### **3\. Delay (Thời gian nhấn giữ phím)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Khoảng thời gian từ lúc phím được "nhấn xuống" (keydown) cho đến
  khi "thả ra" (keyup).
- **Ứng dụng:** Để mô phỏng người thật, bạn nên để khoảng `50ms - 100ms`. Nếu để
  0ms, một số trang web nhạy cảm có thể nhận diện là tốc độ của máy.

### **4\. Selection / Focus (Đối tượng nhận phím)**

- **Mô tả:** Trong một số phiên bản NovaCore, bạn có thể chọn gửi phím vào một
  **Selector** cụ thể hoặc gửi vào cửa sổ trình duyệt nói chung.
- **Mẹo:** Tốt nhất nên dùng lệnh **Click** hoặc **Focus** vào ô nhập liệu trước
  khi dùng lệnh **Press Key** để đảm bảo phím được gửi đúng chỗ.

## \- Type text

Component **Type Text** (hoặc **Fill Input**) là lệnh mô phỏng việc gõ bàn phím
để điền dữ liệu vào các ô nhập liệu (input, textarea). Đây là component quan
trọng nhất để nhập username, password hoặc viết bài đăng.

Dưới đây là các params chi tiết của component này:

### **1\. Selector (Phần tử nhận văn bản)**

- **Mô tả:** Định danh của ô nhập liệu mà bạn muốn điền văn bản vào.
- **Giá trị:** CSS Selector hoặc XPath.
- **Lưu ý:** Bạn nên đảm bảo phần tử này là một ô có thể nhập liệu (thẻ `input`,
  `textarea` hoặc các thẻ có thuộc tính `contenteditable="true"`).

### **2\. Text (Nội dung nhập)**

- **Mô tả:** Chuỗi ký tự bạn muốn điền vào.
- **Giá trị:** Có thể là văn bản cố định hoặc biến số (ví dụ: `{{password}}`).
- **Mẹo:** NovaCore hỗ trợ cả định dạng **Spintax** trong một số phiên bản (ví
  dụ: `{Chào|Hi|Hello}`) để tạo ra nội dung ngẫu nhiên cho mỗi profile.

### **3\. Delay between keys (Độ trễ giữa các phím)**

Đây là tham số cực kỳ quan trọng để "vượt rào" các hệ thống chống bot:

- **Giá trị:** Thường là một dải (Min \- Max) tính bằng miligiây (ms). Ví dụ:
  `50 - 200`.
- **Tác dụng:** Trình duyệt sẽ nghỉ một khoảng ngẫu nhiên giữa mỗi chữ cái.
  Người thật không bao giờ gõ với tốc độ đều tắp như máy.
- **Khuyến nghị:** Đối với các nền tảng gắt gao như Facebook hay Google, hãy để
  độ trễ cao một chút để giống người đang gõ văn bản thật.

### **4\. Clear Input (Xóa nội dung cũ)**

- **Trạng thái:** Checkbox (True/False).
- **Tác dụng:** Nếu bật, NovaCore sẽ xóa sạch các ký tự đang có sẵn trong ô nhập
  liệu trước khi bắt đầu gõ nội dung mới. Điều này giúp tránh việc dữ liệu mới
  bị ghi đè hoặc nối đuôi vào dữ liệu cũ.

### **5\. Simulate Human Typing (Mô phỏng gõ thật)**

- **Tác dụng:** Khi bật tính năng này, NovaCore sẽ mô phỏng cả các sự kiện
  `keydown`, `keypress`, và `keyup`. Một số website sử dụng JavaScript để theo
  dõi xem người dùng có thực sự gõ phím hay không (thay vì chỉ dán văn bản vào).

### **6\. Focus before typing (Tập trung trước khi gõ)**

- **Tác dụng:** Tự động thực hiện một lệnh Click hoặc Focus vào ô nhập liệu
  trước khi bắt đầu gửi các phím. Việc này đảm bảo con trỏ chuột đã nằm đúng vị
  trí để nhận văn bản.

## \- Element exists

### **1\. Selector (Phần tử cần kiểm tra)**

- **Mô tả:** Nhập CSS Selector hoặc XPath của đối tượng bạn muốn kiểm tra xem nó
  có nằm trong mã nguồn trang web hay không.
- **Ứng dụng:** Thường dùng để kiểm tra xem một Popup quảng cáo có hiện ra
  không, hoặc kiểm tra xem đã đăng nhập thành công chưa (bằng cách tìm nút "Đăng
  xuất").

### **2\. Variable (Biến lưu kết quả)**

- **Mô tả:** Tên biến bạn đặt để lưu giá trị trả về (Ví dụ: `is_popup_visible`).
- **Giá trị trả về:**
  - `true`: Nếu tìm thấy phần tử.
  - `false`: Nếu không tìm thấy phần tử.
- **Lưu ý:** Biến này sau đó sẽ được dùng làm đầu vào cho component **If
  Condition**.

### **3\. Check Visibility (Kiểm tra tính hiển thị)**

Đây là param cực kỳ quan trọng để phân biệt giữa việc "có trong code" và "người
dùng có thấy không".

- **Mô tả:** Một tùy chọn (Checkbox hoặc Dropdown).
- **Các mức độ:**
  - **In DOM:** Chỉ cần phần tử có trong mã HTML (dù nó đang bị ẩn bằng
    `display:none`).
  - **Visible:** Phần tử phải hiển thị trên màn hình, có kích thước
    (width/height \> 0\) và người dùng có thể tương tác được.

### **4\. Timeout (Thời gian chờ)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Thời gian tối đa hệ thống sẽ "đợi" để tìm phần tử đó trước khi kết
  luận là `false`.
- **Mẹo:** Nếu bạn chỉ muốn kiểm tra nhanh sự xuất hiện của một cái gì đó đã
  load xong, hãy để Timeout ngắn (ví dụ: `1000ms - 2000ms`).

## \- Multi element exists

### **1\. Elements List (Danh sách phần tử)**

Đây là phần quan trọng nhất, nơi bạn khai báo các Selector cần kiểm tra.

- **Selector Name:** Tên gợi nhớ cho từng phần tử (Ví dụ: `Nut_Dang_Nhap`,
  `Loi_Sai_Pass`, `Giao_Dien_Chinh`).
- **Selector Value:** Mã CSS Selector hoặc XPath tương ứng.
- **Logic:** Hệ thống sẽ quét toàn bộ danh sách này để xem những cái nào đang
  xuất hiện.

### **2\. Match Mode (Chế độ khớp)**

Tham số này quyết định điều kiện để lệnh này trả về kết quả thành công:

- **Match Any (OR):** Chỉ cần **ít nhất một** phần tử trong danh sách xuất hiện
  là lệnh trả về `true`.
- **Match All (AND):** Tất cả các phần tử trong danh sách **phải xuất hiện cùng
  lúc** mới trả về `true`.

### **3\. Check Visibility (Kiểm tra hiển thị)**

- **Mô tả:** Tương tự như lệnh đơn, bạn chọn kiểm tra xem phần tử chỉ cần có
  trong code (`In DOM`) hay phải thực sự hiển thị trên màn hình (`Visible`).
- **Ứng dụng:** Rất quan trọng khi trang web có nhiều popup chồng chéo nhưng chỉ
  có một cái đang hiện ra.

### **4\. Timeout (Thời gian chờ tổng)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Thời gian tối đa để hệ thống quét danh sách các phần tử này.
- **Lưu ý:** Khác với lệnh đơn, Timeout ở đây áp dụng cho việc quét toàn bộ danh
  sách.

### **5\. Output Variable (Biến đầu ra)**

Đây là điểm khác biệt lớn nhất so với lệnh đơn:

- **Result Variable:** Lưu kết quả `true/false`.
- **Found Selector Name:** Lưu **Tên (Name)** của phần tử đầu tiên được tìm
  thấy.
  - _Ví dụ:_ Nếu tìm thấy `Loi_Sai_Pass`, biến này sẽ mang giá trị là chuỗi
    `"Loi_Sai_Pass"`. Bạn dùng biến này để rẽ nhánh cực nhanh trong lệnh
    **Switch** hoặc **If**.

## \- Get URL

### **1\. Variable Name (Tên biến lưu trữ)**

- **Mô tả:** Đây là nơi bạn đặt tên cho biến sẽ chứa giá trị URL sau khi lấy
  được.
- **Ví dụ:** Bạn đặt tên là `current_link`. Sau bước này, bạn có thể gọi giá trị
  đó ra bằng cú pháp `{{current_link}}`.
- **Ứng dụng:** Dùng để so sánh trong các lệnh Logic hoặc ghi vào file
  Excel/Text làm báo cáo kết quả.

### **2\. Output Format (Định dạng đầu ra)**

Một số phiên bản nâng cao cho phép bạn chọn cách lấy URL:

- **Full URL:** Lấy toàn bộ đường dẫn (ví dụ:
  `https://shopee.vn/product/123?spm_id=456`).
- **Origin / Base URL:** Chỉ lấy tên miền gốc (ví dụ: `https://shopee.vn`).
- **Pathname:** Chỉ lấy phần sau tên miền (ví dụ: `/product/123`).

### **3\. Execution Delay (Thời gian chờ thực hiện)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Thời gian hệ thống sẽ nghỉ một chút trước khi thực hiện lấy URL.
- **Mẹo:** Nên để khoảng `500ms` để đảm bảo trình duyệt đã cập nhật xong URL mới
  sau một cú click hoặc chuyển trang.

## \- Get text

### **1\. Selector (Phần tử mục tiêu)**

- **Mô tả:** Định danh của phần tử chứa văn bản bạn muốn lấy.
- **Giá trị:** CSS Selector hoặc XPath.
- **Ví dụ:** Nếu bạn muốn lấy giá sản phẩm, bạn có thể dùng selector như
  `.product-price` hoặc `//span[@id='price']`.

### **2\. Variable Name (Tên biến lưu trữ)**

- **Mô tả:** Tên biến bạn đặt để chứa nội dung văn bản lấy được.
- **Ví dụ:** Bạn đặt tên là `product_name`. Sau khi lệnh chạy, bạn có thể gọi
  giá trị này bằng `{{product_name}}` trong các lệnh khác như _Type Text_ hoặc
  _Save to File_.

### **3\. Get Property / Attribute (Tùy chọn nâng cao)**

Mặc dù tên là "Get Text", nhưng trong nhiều phiên bản NovaCore, component này
tích hợp khả năng lấy các giá trị khác của phần tử:

- **InnerText:** Lấy toàn bộ văn bản hiển thị (thường dùng nhất).
- **TextContent:** Lấy văn bản bao gồm cả những phần bị ẩn bởi CSS.
- **Value:** Dùng để lấy nội dung người dùng đã nhập trong các ô `<input>` hoặc
  `<textarea>`.

### **4\. Trim (Cắt khoảng trắng)**

- **Trạng thái:** Checkbox (Bật/Tắt).
- **Tác dụng:** Tự động loại bỏ các dấu cách dư thừa hoặc ký tự xuống dòng ở đầu
  và cuối đoạn văn bản lấy được. Điều này cực kỳ hữu ích khi bạn lấy dữ liệu để
  so sánh logic hoặc lưu vào database.

### **5\. Timeout (Thời gian chờ)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Thời gian tối đa hệ thống đợi phần tử đó xuất hiện để lấy text. Nếu
  quá thời gian này mà phần tử không tồn tại, lệnh sẽ báo lỗi hoặc trả về giá
  trị rỗng tùy cấu hình.

## \- Get value

Component **Get Value** trong NovaCore thường khiến người mới bắt đầu nhầm lẫn
với lệnh _Get Text_. Tuy nhiên, về mặt kỹ thuật, nó được thiết kế riêng để lấy
dữ liệu từ các **ô nhập liệu (Form Fields)** mà người dùng hoặc hệ thống đã điền
vào.

Dưới đây là các params chi tiết của component này:

---

### **1\. Selector (Phần tử mục tiêu)**

- **Mô tả:** Định danh của phần tử bạn muốn lấy giá trị.
- **Đối tượng áp dụng:** Thường là các thẻ HTML có thuộc tính `value` như:
  - `<input>` (Ô nhập username, email, checkbox, radio).
  - `<textarea>` (Ô nhập nội dung bài viết, ghi chú).
  - `<select>` (Menu thả xuống \- lấy giá trị của option đang được chọn).
- **Lưu ý:** Nếu bạn dùng Selector này trỏ vào một thẻ `<div>` hoặc `<span>`,
  kết quả trả về thường sẽ bị rỗng (vì các thẻ này không có thuộc tính `value`).

### **2\. Variable Name (Tên biến lưu trữ)**

- **Mô tả:** Tên biến bạn đặt để chứa dữ liệu lấy được.
- **Ví dụ:** Bạn đặt tên là `saved_username`. Sau đó bạn có thể dùng
  `{{saved_username}}` để điền vào một trang web khác.

### **3\. Property Type (Loại thuộc tính \- Tùy chọn)**

Trong một số phiên bản, bạn có thể chọn chính xác loại dữ liệu cần lấy:

- **Value:** Giá trị hiện tại đang nằm trong ô nhập liệu (mặc định).
- **Checked:** Dùng cho Checkbox hoặc Radio button (trả về `true` nếu đã tích,
  `false` nếu chưa).

### **4\. Timeout (Thời gian chờ)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Thời gian tối đa hệ thống đợi ô nhập liệu đó xuất hiện trên trang.

## \- Get attribute

### **1\. Selector (Phần tử mục tiêu)**

- **Mô tả:** Định danh của phần tử chứa thuộc tính bạn muốn lấy.
- **Giá trị:** CSS Selector hoặc XPath.

### **2\. Attribute Name (Tên thuộc tính)**

Đây là nơi bạn chỉ định chính xác thông tin muốn trích xuất. Một số thuộc tính
phổ biến bao gồm:

- `href`: Lấy đường dẫn liên kết (link) của một thẻ `<a>`.
- `src`: Lấy link hình ảnh hoặc video của thẻ `<img>` hoặc `<video>`.
- `id` / `class`: Lấy mã định danh hoặc lớp của phần tử để dùng cho các bước
  logic tiếp theo.
- `placeholder`: Lấy văn bản gợi ý trong ô nhập liệu.
- `title`: Lấy nội dung hiển thị khi bạn di chuột qua phần tử.
- `style`: Lấy các định dạng CSS (ví dụ: lấy màu sắc hoặc trạng thái ẩn/hiện).

### **3\. Variable Name (Tên biến lưu trữ)**

- **Mô tả:** Tên biến bạn tự đặt để lưu giá trị lấy được (ví dụ: `image_url`
  hoặc `link_bai_viet`).
- **Sử dụng:** Sau đó bạn gọi biến bằng cú pháp `{{image_url}}`.

### **4\. Timeout (Thời gian chờ)**

- **Đơn vị:** Miligiây (ms).
- **Mô tả:** Thời gian tối đa hệ thống đợi cho đến khi phần tử đó xuất hiện
  trong mã nguồn.

## \- Select dropdown

### **1\. Selector (Phần tử Dropdown)**

- **Mô tả:** Định danh của thẻ `<select>`.
- **Lưu ý:** Selector phải trỏ đúng vào thẻ cha `<select>`, không phải trỏ vào
  các thẻ con `<option>` bên trong.

### **2\. Select By (Phương thức chọn)**

Đây là tham số quan trọng nhất để xác định cách bạn muốn tìm "lựa chọn" trong
menu:

- **Value:** Chọn dựa trên thuộc tính `value` ẩn trong code (thường là mã số
  hoặc từ viết tắt).
- **Text (Label):** Chọn dựa trên chữ hiển thị trên màn hình mà người dùng nhìn
  thấy.
- **Index:** Chọn theo thứ tự (0 là cái đầu tiên, 1 là cái thứ hai...).

### **3\. Value (Giá trị cần chọn)**

Tương ứng với phương thức bạn đã chọn ở trên:

- Nếu chọn **Value**: Nhập giá trị trong code (Ví dụ: `vn` cho Việt Nam).
- Nếu chọn **Text**: Nhập chữ hiển thị (Ví dụ: `Viet Nam`).
- Nếu chọn **Index**: Nhập số thứ tự.

### **4\. Wait Until (Điều kiện chờ)**

- **Mô tả:** Hệ thống sẽ đợi cho đến khi các lựa chọn (options) bên trong
  dropdown tải xong.
- **Ứng dụng:** Rất quan trọng cho các dropdown "động" (ví dụ: chọn Tỉnh/Thành
  phố xong thì Quận/Huyện mới hiện ra).

## \- Random

Component **Random** (Ngẫu nhiên) trong NovaCore là công cụ mạnh mẽ để tạo ra
các kịch bản không trùng lặp, giúp vượt qua các hệ thống phát hiện bot dựa trên
hành vi lặp đi lặp lại. Nó cho phép bạn tạo ra các số, văn bản hoặc thời gian
chờ ngẫu nhiên cho mỗi Profile.

Dưới đây là các loại Random phổ biến trong NovaCore:

---

### **1\. Random Number (Số ngẫu nhiên)**

Dùng để tạo ra một con số trong một khoảng nhất định.

- **Min (Tối thiểu):** Giá trị nhỏ nhất (ví dụ: `10`).
- **Max (Tối đa):** Giá trị lớn nhất (ví dụ: `100`).
- **Variable Name:** Tên biến để lưu số này (ví dụ: `random_wait`).
- **Ứng dụng:** Dùng làm thời gian nghỉ giữa các bước. Thay vì Profile nào cũng
  nghỉ đúng 5 giây, bạn để nghỉ từ `3000ms` đến `7000ms`.

### **2\. Random Text / Spintax (Văn bản ngẫu nhiên)**

Dùng để thay đổi nội dung tin nhắn, bình luận hoặc bài đăng.

- **Cú pháp Spintax:** `{Nội dung 1|Nội dung 2|Nội dung 3}`.
- **Cơ chế:** Mỗi lần chạy, hệ thống sẽ bốc ngẫu nhiên một trong các cụm từ
  trong ngoặc.
- **Ứng dụng:** Viết bình luận mồi cho bài viết. Ví dụ:
  `{Sản phẩm tuyệt vời|Shop phục vụ rất tốt|Giao hàng nhanh quá}`.

### **3\. Random Point (Tọa độ ngẫu nhiên)**

Dùng trong các lệnh di chuyển chuột (**Mouse Movement**) hoặc **Click**.

- **Mô tả:** Thay vì click vào đúng tâm của nút (ví dụ tọa độ 100, 100), bạn
  thêm một độ lệch ngẫu nhiên (Offset) khoảng \+/- 5 pixel.
- **Tác dụng:** Tránh việc hàng nghìn tài khoản cùng click vào một pixel duy
  nhất trên màn hình – một dấu hiệu cực kỳ rõ ràng của bot.

---

### **4\. Random Item from List (Chọn ngẫu nhiên từ danh sách)**

Dùng khi bạn có một danh sách dữ liệu sẵn (ví dụ: danh sách 1000 tên người dùng
hoặc 500 địa chỉ).

- **Input:** Đường dẫn file `.txt` hoặc mảng dữ liệu.
- **Cơ chế:** Chọn ra 1 dòng ngẫu nhiên từ file để sử dụng.
- **Ứng dụng:** Lấy ngẫu nhiên một "Câu hỏi bảo mật" hoặc một "Avatar" để cập
  nhật cho Profile.

## \- File upload

Component **File Upload** (Tải lên tệp) là một trong những lệnh "khó chịu" nhất
khi làm automation vì nó thường tương tác với cửa sổ chọn file của hệ điều hành
(Windows Explorer), thứ mà trình duyệt không thể điều khiển trực tiếp. Tuy
nhiên, NovaCore cung cấp các cơ chế để xử lý việc này một cách mượt mà.

Dưới đây là các params và phương thức thực hiện chi tiết:

---

### **1\. Selector (Phần tử Input File)**

- **Mô tả:** Định danh của phần tử HTML dùng để tải file.
- **Đặc điểm nhận dạng:** Thông thường đây là thẻ `<input type="file">`.
- **Lưu ý:** Nhiều trang web hiện đại ẩn thẻ này đi và thay bằng một nút bấm đẹp
  mắt hơn. Bạn phải tìm đúng thẻ `input` bị ẩn đó (thường có `display: none`
  hoặc `opacity: 0`) thì lệnh mới hoạt động ổn định.

### **2\. File Path (Đường dẫn tệp)**

- **Mô tả:** Đường dẫn tuyệt đối dẫn đến file trên máy tính của bạn (Ví dụ:
  `C:\Users\Admin\Desktop\avatar.jpg`).
- **Sử dụng:** Bạn có thể nhập trực tiếp hoặc sử dụng biến (Ví dụ:
  `D:\Photos\{{profile_id}}.png`).

### **3. Multiple Files (Tải lên nhiều tệp)**

- **Trạng thái:** Checkbox (Bật/Tắt).
- **Tác dụng:** Nếu trang web cho phép (ví dụ: đăng nhiều ảnh lên Facebook cùng
  lúc), bạn có thể liệt kê các đường dẫn file cách nhau bởi dấu phẩy hoặc xuống
  dòng.

---

## **- Nhóm node Dữ liệu (Automation Data)**

Nhóm node này giúp bạn tương tác với các tệp tin bên ngoài (CSV, Excel) và trích
xuất dữ liệu từ các nguồn khác.

### **1. Node: Chèn Dữ liệu (Insert Data)**

Node này được dùng để lưu trữ dữ liệu từ quy trình vào một tệp văn bản (thường
là CSV).

**Tham số & Cơ chế:**

- **Đường dẫn file**: Ví dụ `D:\data\output.csv`.
  - _Cơ chế thông minh_: Hệ thống tự động phân tích tên biến linh hoạt (chấp
    nhận cả `path`, `filePath` hoặc `destination`).
  - _An toàn file_: Tự động khởi tạo (Recursive mkdir) tất cả các thư mục cha
    nếu chúng chưa tồn tại, giúp dòng chảy quy trình không bị ngắt quãng bởi lỗi
    "Folder not found".
- **Dòng dữ liệu (CSV)**: Hỗ trợ mảng biến `{{$vars.list}}` (tự động nối bằng
  dấu phẩy) hoặc chuỗi văn bản thuần túy.

### **2. Node: Cập nhật Trường (Update Field)**

Dùng để sửa đổi một giá trị cụ thể trong file CSV hiện có mà không làm ảnh hưởng
đến các hàng khác.

**Các chế độ xử lý chuyên sâu:**

1. **Theo số hàng (Row Index)**: Truy cập hàng theo chỉ số (Hàng 1 là Index 0).
   Phù hợp khi bạn đang xử lý danh sách tuần tự.
2. **Theo giá trị khớp (Match Value)**:
   - **Cột tìm kiếm**: Vị trí cột chứa từ khóa (ví dụ: cột chứa ID tài khoản).
   - **Giá trị tìm kiếm**: Nội dung cần so khớp.
   - _Ứng dụng_: Tự động quét toàn bộ file, hễ dòng nào có ID khớp thì cập nhật
     trạng thái ở cột đích.

### **3. Node: Đọc Email (IMAP)**

Node này kết nối trực tiếp vào máy chủ Email để lấy dữ liệu (mã xác nhận, link).

**Logic trích xuất thông minh:**

- **Làm sạch sâu (Deep Clean)**: Tự động loại bỏ mã HTML bẩn, các thực thể lạ
  như `&nbsp;` và chuẩn hóa khoảng trắng. Điều này đảm bảo việc trích xuất OTP
  luôn chính xác kể cả với email có định dạng phức tạp nhất.
- **Ưu tiên OTP**: Thuật toán tìm dãy số 4-10 chữ số, nhưng sẽ **ưu tiên tuyệt
  đối mã có 6 chữ số** (định dạng OTP phổ biến nhất hiện nay). Nếu không có mã 6
  số, hệ thống sẽ lấy dãy số đầu tiên tìm thấy.
- **Cấu hình**: Mặc định chế độ trích xuất OTP là **BẬT**. Bạn chỉ cần truyền
  tên biến lưu mã (`otpCode`).

### **4\. Cơ chế thông minh (Smart Upload)**

Hệ thống sử dụng thuật toán **FileChooser Listener**. Thay vì chỉ cố gắng gán
giá trị vào thẻ input (thứ thường bị ẩn hoặc bảo mật gắt gao), hệ thống sẽ đứng
đợi hộp thoại chọn file của trình duyệt. Khi bạn click vào bất kỳ đâu trên nút
Tải lên, hệ thống sẽ tự động "bơm" file vào ngay khi hộp thoại vừa mở ra, giúp
tương thích mượt mà với 100% các website hiện đại.

## **- Giải Captcha tự động:**

### **1\. Dịch vụ (Provider)**

- **Manual:** Người dùng tự giải bằng tay khi trình duyệt dừng lại.
- **2Captcha / Anti-Captcha:** Tự động gửi lệnh giải qua API của nhà cung cấp
  dịch vụ.

### **2. API Key**

- Nhập mã khóa API của bạn từ trang quản trị dịch vụ. Mã này sẽ được mã hóa để
  đảm bảo an toàn.

### **3. Selector kiểm tra thành công**

- Phải điền một phần từ sẽ xuất hiện **sau khi giải xong** (ví dụ: nút
  Dashboard, hoặc thông báo "Success"). Hệ thống dựa vào đây để biết khi nào bạn
  đã vượt qua Captcha thành công.

---

## **- Gửi HTTP Request (httpRequest)**

Đây là phiên bản nâng cấp mạnh mẽ giúp bạn tương tác với các API bên ngoài
chuyên nghiệp như sử dụng Postman.

### **1. Thanh công cụ & cURL Import**

- **Method & URL**: Chọn phương thức (GET, POST, PUT, DELETE, PATCH) và nhập địa
  chỉ API ngay trên thanh tiêu đề.
- **Import from cURL (Biểu tượng ➕)**: Bạn có thể sao chép lệnh cURL từ trình
  duyệt (Tab Network -> Copy as cURL) và nhấn nút này để hệ thống tự động điền
  toàn bộ thông số: URL, Method, Headers và Body.

### **2. Các Tab cấu hình chi tiết**

- **Thông số (Params)**: Quản lý danh sách Query Parameters (ví dụ:
  `?key=value`) dưới dạng bảng Key-Value trực quan.
- **Xác thực (Auth)**:
  - **No Auth**: Không dùng xác thực.
  - **Bearer Token**: Nhập mã token để hệ thống tự thêm vào header
    `Authorization: Bearer <token>`.
  - **Basic Auth**: Nhập Username & Password để hệ thống tự mã hóa Base64 và
    thêm vào header `Authorization: Basic <base64>`.
- **Đầu bản tin (Headers)**: Quản lý các Request Headers tùy chỉnh dưới dạng
  bảng Key-Value.
- **Nội dung (Body)**: Nhập nội dung gửi đi (thường là JSON) cho các yêu cầu
  POST/PUT.
- **Cài đặt**:
  - **Timeout**: Thời gian chờ phản hồi (mặc định 30.000ms).
  - **Lưu kết quả phản hồi**: Bạn có thể ánh xạ (map) kết quả trả về vào các
    biến số trong kịch bản:
    - **Nội dung phản hồi (Body)**: Lưu toàn bộ dữ liệu trả về vào một biến (ví
      dụ: `api_data`).
    - **Mã trạng thái (Status)**: Lưu mã HTTP Code (ví dụ: 200, 404) vào một
      biến (ví dụ: `api_status`).
    - **Đầu bản tin (Headers)**: Lưu các response headers vào một biến (ví dụ:
      `api_headers`).
    - **Cookie phản hồi**: Lưu danh sách các cookie nhận được vào một biến (ví
      dụ: `api_cookies`).

  - **Trích xuất nâng cao (Advanced Mapping)**: Cho phép bạn "nhặt" chính xác
    các mẩu dữ liệu nhỏ từ phản hồi để gán vào biến:
    - **Body (JSON Path)**: Nhập đường dẫn đến dữ liệu trong JSON. Ví dụ:
      `data.user.id` hoặc `results[0].token`.
    - **Header**: Nhập tên Header cần lấy giá trị. Ví dụ: `X-Rate-Limit` hoặc
      `Content-Type`.
    - **Cookie**: Nhập tên Cookie cụ thể muốn trích xuất giá trị. Ví dụ:
      `sessionid`.

> [!TIP]
> Hệ thống sử dụng quy tắc dấu chấm (`.`) để truy cập các thuộc tính lồng nhau
> trong JSON. Nếu API trả về một danh sách, bạn có thể dùng `[0]` để lấy phần tử
> đầu tiên.

## **- Lệnh Hệ thống (cmd)**

Node này cho phép bạn thực thi các lệnh trực tiếp trên hệ điều hành của máy tính
(Shell Command). Điểm đặc biệt là node này hỗ trợ **đa nền tảng thông minh**,
giúp kịch bản của bạn chạy mượt mà trên cả Windows, macOS và Linux.

### **1. Các thông số cấu hình**

- **Lệnh chung (Mặc định)**: Câu lệnh sẽ được thực thi nếu không có cấu hình
  riêng cho từng hệ điều hành.
- **[Chế độ Nâng cao] Lệnh theo Hệ điều hành**:
  - **Lệnh Windows**: Chỉ chạy khi kịch bản được thực thi trên máy tính Windows.
  - **Lệnh macOS**: Chỉ chạy khi kịch bản được thực thi trên máy Mac.
  - **Lệnh Linux**: Chỉ chạy khi kịch bản được thực thi trên các bản phân phối
    Linux.

### **2. Cơ chế thực thi thông minh**

Khi kịch bản chạy đến node này, hệ thống thực hiện các bước sau:

1. Kiểm tra hệ điều hành của máy hiện tại.
2. Tìm xem bạn có nhập lệnh riêng cho hệ điều hành đó không (ví dụ: tìm ô **Lệnh
   Windows** trên máy Windows).
3. Nếu có, nó sẽ thực hiện lệnh đó và bỏ qua ô **Lệnh chung**.
4. Nếu không có lệnh riêng, nó sẽ sử dụng câu lệnh trong ô **Lệnh chung**.

### **3. Ví dụ cấu hình đa nền tảng**

Để một kịch bản "Liệt kê file" chạy được mọi nơi:

- **Lệnh chung**: `ls`
- **Lệnh Windows (Nâng cao)**: `dir`
- **Lệnh macOS/Linux (Nâng cao)**: `ls -la`

### **4. Kết quả trả về**

Hệ thống sẽ lưu kết quả (stdout, stderr) vào một đối tượng JSON. Bạn có thể dùng
`{{ $node["Tên Node"].stdout }}` để lấy nội dung trả về phục vụ cho các bước
sau.

> [!TIP]
> Sử dụng các màu sắc phân biệt trong giao diện nhập liệu (Amber cho lệnh chung,
> Blue cho Windows, Purple cho Mac, Green cho Linux) để giúp bạn dễ dàng quản lý
> kịch bản phức tạp.

## **- Tạo mã 2FA (otp2fa)**

Node này giúp bạn tự động sinh mã xác thực 2 lớp (TOTP - Time-based One-Time
Password) từ một khóa bí mật (Secret Key). Đây là giải pháp hoàn hảo để vượt qua
các bước xác minh danh tính khi đăng nhập vào Facebook, Google, Binance, v.v. mà
không cần dùng ứng dụng Google Authenticator trên điện thoại.

### **1. Các thông số cấu hình**

- **Khóa bí mật (Secret Key)**: Chuỗi ký tự bí mật mà trang web cung cấp cho bạn
  khi thiết lập bảo mật 2 lớp (thường có dạng như `JBSWY3DPEHPK3PXP`).
  - _Tính năng Ưu việt_: Bạn có thể **dán trực tiếp ảnh QR code** (Ctrl+V) vào ô
    này. Hệ thống sẽ tự động quét và trích xuất Secret Key cho bạn.
  - _Độ tương thích cực cao_: Thuật toán được tối ưu để hỗ trợ **ngoại lệ** các
    Secret Key có độ dài ngắn (ví dụ: 10-byte/80-bit của Google Authenticator)
    mà các thư viện thông thường hay báo lỗi "Secret too short".
  - _Lưu ý_: Bạn có thể dán cả link `otpauth://` hoặc dùng biến
    `{{secret_key}}`.
- **Tên biến lưu kết quả**: Tên biến bạn muốn đặt để chứa mã 6 số vừa sinh ra
  (ví dụ: `ma_2fa`).
  - _Cách dùng_: Sử dụng `{{ma_2fa}}` trong các node "Nhập văn bản" hoặc "Nhấn
    phím" tiếp theo.

### **2. Luồng xử lý "Vượt 2FA" điển hình**

1. **Bước 1**: Lấy Secret Key (từ kịch bản, từ file hoặc dán QR).
2. **Bước 2**: Thực hiện đăng nhập bằng Username/Password trên trình duyệt.
3. **Bước 3**: Khi web hiện ô nhập mã 6 số, hãy gọi node **Tạo mã 2FA**.
4. **Bước 4**: Dùng lệnh **Nhập văn bản (Type Text)** điền mã vừa sinh ra vào ô
   nhập liệu và hoàn tất đăng nhập.

### **3. Ưu điểm nổi bật**

- **Hoạt động Offline**: Việc tạo mã diễn ra ngay trên máy tính của bạn, không
  cần internet, đảm bảo tốc độ cực nhanh và bảo mật 100%.
- **Tính nhất quán**: Mã sinh ra luôn khớp 100% với ứng dụng Google
  Authenticator, Authy hay Microsoft Authenticator.
- **Tự động hóa hoàn toàn**: Không cần cầm điện thoại, không cần chờ đợi SMS.

> [!IMPORTANT]
> **Lỗi sai mã (Invalid Code)?** TOTP phụ thuộc hoàn toàn vào thời gian. Nếu mã
> sinh ra bị sai, hãy kiểm tra và **đồng bộ lại thời gian (Sync time)** trong
> cài đặt Windows/macOS của bạn để khớp với giờ quốc tế.

---

## \- Read file / variables

### **1\. Read File (Đọc tệp tin)**

Lệnh này cho phép bạn lấy dữ liệu từ các file văn bản (`.txt`, `.csv`) hoặc bảng
tính (`.xlsx`).

- **File Path (Đường dẫn):** Nơi lưu trữ file trên máy tính.
  - _Mẹo:_ Nên dùng biến `{{folder_path}}` để kịch bản linh hoạt hơn khi chuyển
    sang máy khác.
- **Read Mode (Chế độ đọc):**
  - **Read All:** Lấy toàn bộ nội dung file (thường dùng để lấy danh sách Proxy
    hoặc cấu hình chung).
  - **Read Line by Line:** Đọc từng dòng. Kết hợp với vòng lặp để mỗi profile xử
    lý một dòng dữ liệu khác nhau (Ví dụ: Profile 1 lấy dòng 1 \- User A,
    Profile 2 lấy dòng 2 \- User B).
- **Variable Name:** Tên biến sẽ chứa dữ liệu vừa đọc được.

---

### **2. Variables (Quản lý Biến)**

Biến là những "hộp chứa" thông tin tạm thời trong quá trình kịch bản chạy.

- **Set Variable (Gán giá trị):** Bạn có thể gán một giá trị cụ thể hoặc kết quả
  của một phép tính vào biến.
  - _Ví dụ:_ `counter = 0`.
- **Global Variables (Biến toàn cục):** Là những biến có thể sử dụng xuyên suốt
  tất cả các profile trong cùng một kịch bản (ví dụ: tổng số đơn hàng đã đặt
  thành công).
- **Local Variables (Biến cục bộ):** Chỉ có tác dụng trong duy nhất một profile
  đang chạy (ví dụ: tên đăng nhập của profile đó).

---

### **3. Kho: Lưu dữ liệu (Storage: Set)**

Node này dùng để ghi nhớ thông tin một cách **vĩnh viễn** vào ổ cứng (SQLite).

**Sự khác biệt giữa Biến và Kho lưu trữ:**

- **Biến (`Set Variable`)**: Bạn ghi vào bộ nhớ tạm. Tắt app là mất.
- **Kho lưu trữ (`Storage`)**: Bạn ghi vào ổ cứng. Ngày mai mở máy lại vẫn còn
  nguyên. Nó tự động gắn liền với `Profile ID` và `Workflow ID` để không bị nhầm
  lẫn giữa các nick khác nhau.

**Tham số:**

- **Khóa lưu trữ (Key)**: "Tên ngăn kéo" bạn muốn để đồ (ví dụ: `ngay_sinh`,
  `otp_vua_lay`).
- **Giá trị**: Nội dung bạn muốn cất vào ngăn kéo đó.

### **4. Kho: Lấy dữ liệu (Storage: Get)**

Dùng để mở ngăn kéo và lấy lại món đồ bạn đã cất lúc trước.

**Tham số:**

- **Khóa lưu trữ (Key)**: Nhập đúng tên Phím (Key) bạn đã dùng để lưu.
- **Cách dùng**: Sau khi lấy, dữ liệu sẽ được trả về kết quả của node. Bạn có
  thể dùng node "Đặt biến" để gán nó vào một biến và sử dụng cho các bước tiếp
  theo.

### **5. Kho: Liệt kê khóa (Storage: List Keys)**

Dùng để xem danh sách tất cả các món đồ đang có trong "két sắt" của bạn.

**Tại sao cần dùng?** Khi bạn lưu quá nhiều thứ và không nhớ hết tên ngăn kéo
(Key), hoặc muốn thực hiện một hành động lặp đi lặp lại trên tất cả dữ liệu (ví
dụ: xóa hết các token đã hết hạn).

**Tham số:**

- **Tên biến lưu kết quả**: Nhập tên biến bạn muốn chứa danh sách các Khóa (ví
  dụ: `all_keys`).
- **Kết quả trả về**: Một danh sách (List) các tên khóa. Bạn có thể kết hợp với
  node **Chia lô (Loop)** để xử lý từng khóa một.

### **6. So sánh: Biến (Variable) vs Kho lưu trữ (Storage)**

Để giúp bạn chọn đúng công cụ, hãy xem bảng so sánh dưới đây:

| Đặc điểm     | Biến (`Set Variable`)                             | Kho lưu trữ (`Storage`)                           |
| :----------- | :------------------------------------------------ | :------------------------------------------------ |
| **Nơi lưu**  | Bộ nhớ tạm (RAM)                                  | Ổ cứng (SQLite Database)                          |
| **Độ bền**   | **Mất sạch** khi kết thúc lượt chạy hoặc tắt app. | **Lưu vĩnh viễn** kể cả khi tắt máy, đổi máy.     |
| **Tốc độ**   | Cực nhanh.                                        | Nhanh (nhưng chậm hơn RAM một chút).              |
| **Phạm vi**  | Chỉ dùng trong flow hiện tại.                     | Dùng lại được cho các flow khác của cùng Profile. |
| **Ứng dụng** | Lưu kết quả trung gian, đếm vòng lặp.             | Lưu Token, Cookie, Tổng kết quả qua nhiều ngày.   |

**Về ký hiệu `{{ $vars.name }}`:** Đây là **Cú pháp gọi dữ liệu**. Bạn có thể
dùng cú pháp này ở **MỌI NƠI** (trong node Click, Type, hay cả trong node Kho).

- Nó không phải là tên của node Kho.
- Nó chỉ là cách bạn nói với hệ thống: "Hãy lấy giá trị của biến `name` và điền
  vào đây".

### **6. Tính độc lập của Dữ liệu (Sự khác biệt sống còn)**

**Câu hỏi**: Nếu tôi dùng node "Đặt biến" để sửa dữ liệu, thì giá trị trong "Kho
lưu trữ" có tự thay đổi theo không? **Trả lời**: **KHÔNG.**

Chúng hoàn toàn độc lập với nhau:

1. **Biến** nằm ở trên mặt bàn (RAM).
2.
   - **Storage (Kho lưu trữ)**: Lưu vào SQLite (Ổ cứng). Dữ liệu sẽ **tồn tại
     vĩnh viễn** cho đến khi bạn chủ động xóa hoặc ghi đè, ngay cả khi tắt máy
     hay khởi động lại NovaCore. Thích hợp để lưu: Token dài hạn, trạng thái đã
     cày (checkpoint), dữ liệu tích lũy qua nhiều ngày.

### Cách xem dữ liệu trong Kho (Storage)

Để biết trong "Két sắt" của một Profile đang có những gì, bạn có 2 cách:

1. **Tab Kho lưu trữ (Giao diện)**:
   - Ở bảng điều khiển phía dưới (Bottom Panel), bên cạnh tab **Nhật ký** và
     **Biến**, bạn sẽ thấy tab **Kho lưu trữ**.
   - Khi chọn một Profile, tab này sẽ liệt kê toàn bộ các cặp **Khóa (Key)** và
     **Giá trị (Value)** đang có trong SQLite của profile đó.
   - Bạn có thể nhấn nút **Refresh** (Làm mới) để cập nhật dữ liệu mới nhất nếu
     quy trình đang chạy và ghi dữ liệu vào kho.

2. **Node "Storage: List Keys" (Trong kịch bản)**:
   - Sử dụng node này nếu bạn muốn lấy danh sách tất cả các Khóa đã lưu để xử lý
     logic (ví dụ: lặp qua các khóa để xóa hoặc kiểm tra).
   - Kết quả trả về là một mảng (List) các tên Khóa.

### Quy trình Đọc - Xử lý - Ghi (Read-Process-Write)

Dữ liệu trong Kho lữu trữ và Biến là hoàn toàn độc lập. Để cập nhật một giá trị
trong kho (ví dụ tăng điểm số):

1. Dùng **Storage: Get** để lấy giá trị từ Kho vào một **Biến**.
2. Dùng **Set Variable** hoặc **Chạy Code JS** để cộng dồn/thay đổi giá trị của
   **Biến** đó.
3. Dùng **Storage: Set** để ghi giá trị mới từ **Biến** ngược trở lại **Kho**.

---

### **7. Đọc File (Read File)**

Node này dùng để đọc nội dung từ một tệp tin trên ổ cứng và đưa vào quy trình xử
lý.

**Tham số:**

- **Đường dẫn File**: Địa chỉ tuyệt đối dẫn tới tệp tin (ví dụ:
  `C:\data\input.txt`).
- **Tên biến lưu kết quả**: Nhập tên biến (ví dụ: `file_content`). Toàn bộ nội
  dung của tệp sẽ được lưu vào biến này để bạn có thể dùng cú pháp
  `{{ $vars.file_content }}` ở các node sau.

**Lưu ý kỹ thuật:**

- Hệ thống sẽ ưu tiên đọc file dưới dạng văn bản (UTF-8).
- Nếu nội dung file là định dạng JSON hợp lệ, hệ thống sẽ tự động chuyển đổi
  sang dạng đối tượng (Object) để bạn truy xuất từng trường dữ liệu dễ dàng.

---

Sau khi đọc file, dữ liệu thường ở dạng thô. Bạn cần "mổ xẻ" nó để lấy đúng
thông tin cần thiết.

## \- Write file

### **1\. File Path (Đường dẫn tệp)**

- **Mô tả:** Địa chỉ nơi tệp sẽ được tạo hoặc ghi đè.
- **Ví dụ:** `C:\NovaCore\Results\success_accounts.txt`
- **Mẹo:** Bạn nên dùng biến đường dẫn động để tự động tạo folder theo ngày hoặc
  theo tên kịch bản, giúp quản lý dữ liệu dễ dàng hơn.

### **2\. Content (Nội dung ghi)**

- **Mô tả:** Dữ liệu bạn muốn ghi vào file.
- **Giá trị:** Có thể là văn bản thuần túy hoặc các biến số đã thu thập được từ
  các bước trước.
- **Ví dụ:** `{{username}}|{{password}}|{{status}}|{{current_time}}`

### **3\. Write Mode (Chế độ ghi)**

Đây là tham số quan trọng nhất để tránh mất dữ liệu:

- **Ghi nối tiếp (Append):** Bật tùy chọn này nếu bạn muốn **thêm** nội dung mới
  vào cuối tệp hiện có mà không xóa đi dữ liệu cũ. (Rất hữu ích khi gom kết quả
  từ nhiều profile vào chung một file).
- **Ghi đè (Overwrite):** Tắt tùy chọn "Ghi nối tiếp". Hệ thống sẽ xóa sạch nội
  dung file cũ và thay bằng nội dung mới nhất.

**Tính năng thông minh:**

- **Tự động tạo thư mục**: Nếu đường dẫn bạn nhập chứa các thư mục chưa tồn tại,
  hệ thống sẽ tự động tạo chúng (Recursive) cho bạn. Bạn không cần phải tạo
  folder thủ công ngoài Windows.
- **Tự động xuống dòng**: Khi ở chế độ Append, hệ thống sẽ tự động thêm ký tự
  xuống dòng ở cuối mỗi lần ghi để các dòng không bị dính vào nhau.

### **4\. New Line (Xuống dòng)**

- **Trạng thái:** Checkbox (Bật/Tắt).
- **Tác dụng:** Tự động thêm ký tự xuống dòng sau mỗi lần ghi. Nếu bạn dùng chế
  độ **Append**, hãy luôn bật tính năng này để mỗi kết quả nằm trên một dòng
  riêng biệt, giúp file `.csv` hoặc `.txt` dễ đọc hơn.

### **5\. Encoding (Mã hóa)**

- **Giá trị:** Thường mặc định là `UTF-8`.
- **Tác dụng:** Đảm bảo các ký tự tiếng Việt hoặc ký tự đặc biệt không bị lỗi
  font khi bạn mở bằng Notepad hoặc Excel.

## \- Set variable

### **1\. Variable Name (Tên biến)**

- **Mô tả:** Tên định danh để bạn gọi lại ở các bước sau.
- **Quy tắc:** Nên đặt tên không dấu, không khoảng trắng, dùng dấu gạch dưới (ví
  dụ: `my_counter`, `temp_status`).
- **Cách gọi lại:** Sau khi Set, bạn dùng cú pháp `{{ten_bien}}` ở bất kỳ
  component nào khác.

### **2\. Variable Value (Giá trị biến)**

Đây là nơi bạn xác định nội dung muốn lưu vào "hộp chứa":

- **Giá trị cố định:** Chuỗi văn bản (ví dụ: `Thành công`) hoặc con số (ví dụ:
  `100`).
- **Giá trị từ biến khác:** Gán giá trị của biến A cho biến B (ví dụ:
  `{{current_url}}`).
- **Biểu thức (Expression):** Thực hiện các phép tính toán học cơ bản (ví dụ:
  `{{counter}} + 1`).

### **3\. Variable Scope (Phạm vi của biến)**

Một số phiên bản NovaCore cho phép chọn phạm vi ảnh hưởng:

- **Local (Cục bộ):** Biến chỉ tồn tại trong phiên chạy của 1 Profile đó. Hết
  kịch bản hoặc sang Profile khác, biến sẽ biến mất hoặc reset. (Thường dùng
  nhất).
- **Global (Toàn cục):** Biến có thể được chia sẻ giữa nhiều Profile đang chạy
  song song (ví dụ: dùng để đếm tổng số luồng đã hoàn thành).

## \- Cookies

### **1\. Get Cookies (Lấy Cookies)**

Lệnh này dùng để trích xuất toàn bộ dữ liệu phiên đăng nhập hiện tại từ trình
duyệt và lưu vào một biến.

- **Variable Name:** Tên biến để chứa chuỗi Cookie (ví dụ: `current_cookies`).
- **Format:** Thông thường Cookie sẽ được trả về dưới dạng chuỗi JSON (để máy
  hiểu) hoặc chuỗi văn bản (để người dùng copy-paste).
- **Ứng dụng:** Sau khi bạn thực hiện kịch bản "Đăng nhập thủ công" thành công,
  hãy dùng lệnh này để lấy Cookie và lưu vào file `.txt` hoặc database. Lần sau,
  bạn chỉ cần dùng Cookie này để vào lại mà không cần login nữa.

### **2\. Set Cookies (Nạp Cookies)**

Đây là lệnh ngược lại, dùng để "tiêm" một chuỗi Cookie có sẵn vào trình duyệt
trước khi mở trang web.

- **Cookie Value:** Giá trị chuỗi Cookie (có thể là biến `{{cookies}}` đọc từ
  file).
- **Domain:** Xác định Cookie này dành cho trang web nào (ví dụ:
  `.facebook.com`).
- **Ứng dụng:** Rất hữu ích cho các kịch bản "Login bằng Cookie". Trình duyệt sẽ
  nhận diện bạn đã đăng nhập và bỏ qua bước nhập mật khẩu.

## \- Save assets

Component **Save Assets** trong NovaCore là một tính năng cực kỳ hữu ích dành
cho việc quản lý dữ liệu tài khoản sau khi kịch bản chạy xong. Thay vì phải dùng
các lệnh thủ công như _Get Cookies_, _Get User-Agent_ rồi dùng _Write File_,
component này giúp bạn đóng gói và lưu trữ toàn bộ "trạng thái" của Profile trực
tiếp vào hệ thống quản lý của NovaCore hoặc xuất ra ngoài.

Dưới đây là các chi tiết kỹ thuật và ứng dụng:

---

### **1\. Các loại Assets có thể lưu trữ**

Khi sử dụng lệnh này, bạn có thể chọn lưu một hoặc nhiều thành phần sau:

- **Cookies:** Lưu toàn bộ phiên đăng nhập hiện tại.
- **LocalStorage / SessionStorage:** Lưu các dữ liệu web tùy chỉnh (thường dùng
  cho các trang web hiện đại hoặc ví điện tử).
- **User-Agent:** Lưu thông số trình duyệt đã sử dụng để đảm bảo lần sau mở lại
  sẽ nhất quán.
- **Proxy:** Lưu thông tin Proxy đi kèm với Profile đó.
- **Fingerprint:** Lưu các thông số về vân tay trình duyệt (Canvas, WebGL,
  Audio,...).

### **2\. Destination (Nơi lưu trữ)**

- **Update to Profile:** Đây là tùy chọn phổ biến nhất. Dữ liệu sẽ được ghi đè
  trực tiếp vào chính Profile đang chạy trong trình quản lý NovaCore. Lần sau
  khi bạn mở Profile đó lên, nó sẽ tự động có sẵn trạng thái đã đăng nhập.
- **Export to Variable:** Lưu chuỗi dữ liệu vào một biến (nhập `variable` vào ô
  Đích đến) để bạn có thể xử lý thêm (ví dụ: gửi qua API hoặc lưu vào database
  riêng).
- **Save to File (Mới):** Nếu bạn nhập một đường dẫn file tuyệt đối (ví dụ:
  `C:\backup\session.json`) vào ô Đích đến, hệ thống sẽ xuất toàn bộ dữ liệu ra
  file JSON đó. Hệ thống sẽ tự động tạo thư mục nếu chưa có.

### **3\. File Format (Nếu xuất ra file)**

Nếu bạn kết hợp với lệnh ghi file, bạn thường sẽ lưu dưới dạng:

- **JSON:** Định dạng chuẩn để máy có thể đọc lại dễ dàng.
- **Base64:** Một dạng mã hóa giúp chuỗi dữ liệu không bị lỗi khi chứa các ký tự
  đặc biệt.

## \- Spreedsheet

### **1\. Google Sheets (Tương tác qua API)**

Đây là cách chuyên nghiệp nhất để làm tool vì bạn có thể theo dõi kết quả chạy
của 100 máy tính khác nhau chỉ trên một trình duyệt duy nhất.

- **Spreadsheet ID:** Chuỗi ký tự nằm trong URL của Google Sheet (Ví dụ:
  1BxiMVs0XRA5nFMdKvB8...).
- **Sheet Name:** Tên của tab (trang tính) bạn muốn thao tác (Ví dụ: Sheet1).
- **Range (Vùng dữ liệu):** Vùng bạn muốn đọc hoặc ghi (Ví dụ: A1:D100).
- **Hành động chính:**
  - **Read Row/Cell:** Đọc dữ liệu để điền vào web.
  - **Update Cell:** Ghi trạng thái "Thành công" hoặc "Thất bại" ngay lập tức
    sau khi chạy xong.
  - **Append Row:** Thêm một dòng kết quả mới vào cuối bảng.

---

### **2\. Excel / CSV (Tương tác Offline)**

Nếu bạn không muốn thiết lập API phức tạp, bạn có thể sử dụng các tệp Excel
(.xlsx) hoặc CSV lưu trên máy.

- **File Path:** Đường dẫn đến tệp (Ví dụ: C:/Data/Accounts.xlsx).
- **Column Index/Name:** Chọn cột để lấy dữ liệu (Ví dụ: cột A hoặc cột có tiêu
  đề Username).
- **Row Index:** Thường kết hợp với biến vòng lặp {{loop\_index}} để mỗi lượt
  chạy kịch bản sẽ lấy dữ liệu của một hàng khác nhau.

---

### **3\. Các thông số (Params) quan trọng khi làm việc với Bảng tính**

| Tham số              | Ý nghĩa                                                                                                  |
| :------------------- | :------------------------------------------------------------------------------------------------------- |
| **Header**           | Nếu bảng có dòng tiêu đề (Dòng 1), hãy bật tùy chọn này để NovaCore nhận diện tên cột thay vì số thứ tự. |
| **Start Row**        | Hàng bắt đầu lấy dữ liệu (thường là hàng 2 nếu hàng 1 là tiêu đề).                                       |
| **Variable Mapping** | Ánh xạ cột trong bảng vào biến trong kịch bản (Ví dụ: Cột A \-\> {{username}}, Cột B \-\> {{password}}). |
| **Clear After Read** | Tùy chọn xóa dữ liệu hoặc đánh dấu đã đọc để tránh các Profile khác lấy trùng dữ liệu.                   |

## \- Insert data

### **1\. Chèn dữ liệu vào Google Sheets (Append Row)**

Đây là cách phổ biến nhất để lưu kết quả từ kịch bản vào một bảng tính trực
tuyến.

- **Spreadsheet ID:** Mã ID của tệp Google Sheet.
- **Sheet Name:** Tên trang tính (ví dụ: `Sheet1`).
- **Data to Insert (Dữ liệu chèn):** Một mảng hoặc chuỗi các biến cách nhau bởi
  dấu phẩy.
  - _Ví dụ:_ `{{username}}, {{password}}, {{status}}, {{time}}`
- **Cơ chế:** Lệnh này sẽ tự động tìm hàng trống đầu tiên ở cuối bảng và chèn
  một hàng mới vào đó.

### **2\. Chèn dữ liệu vào Excel/CSV (Write File \- Append Mode)**

Dùng khi bạn muốn lưu dữ liệu cục bộ trên máy tính.

- **File Path:** Đường dẫn tệp `.csv` hoặc `.xlsx`.
- **Content:** Nội dung dòng bạn muốn chèn.
  - _Mẹo:_ Để chèn đúng định dạng bảng, hãy dùng dấu phẩy (`,`) hoặc dấu gạch
    đứng (`|`) để phân tách các cột.
- **Append Mode:** Phải chọn chế độ **Append** (Ghi nối tiếp) để không làm mất
  dữ liệu cũ.

### **3\. Chèn dữ liệu qua HTTP Request (API Insert)**

Dành cho các hệ thống quản lý chuyên nghiệp (CRM, Database riêng).

- **Method:** Thường dùng `POST` hoặc `PUT`.
- **Body (JSON):** Cấu trúc dữ liệu gửi lên server.
  - _Ví dụ:_
    `{"user": "{{username}}", "action": "register", "result": "success"}`
- **URL:** Endpoint của API nhận dữ liệu.

## \- openAI

### **1\. Các thông số (Params) cần thiết để kết nối**

Để gửi một yêu cầu đến OpenAI, bạn cần cấu hình các thông số sau:

- **API Key:** Lấy từ trang quản trị của OpenAI. Đây là "chìa khóa" để NovaCore
  có quyền truy cập vào trí tuệ nhân tạo.
- **Model:** Chọn phiên bản AI (ví dụ: `gpt-4o` cho các tác vụ suy luận phức
  tạp, `gpt-4o-mini` để tối ưu chi phí, hoặc dòng reasoning `o1` để giải quyết
  các bài toán logic siêu cấp).
- **Prompt (Lời nhắc):** Đây là câu lệnh bạn gửi cho AI.
  - _Ví dụ:_ "Hãy viết một bình luận tích cực về sản phẩm giày thể thao này dựa
    trên nội dung sau: `{{product_description}}`".
- **Variable Name (Tên biến lưu kết quả):** Tên biến để chứa văn bản phản hồi từ
  AI (ví dụ: `ai_reply`). Bạn có thể dùng biến này trong các node tiếp theo bằng
  cách gọi `{{$vars.ai_reply}}`.
- **Max Tokens:** Giới hạn độ dài của câu trả lời để kiểm soát chi phí.
- **Temperature:** Độ sáng tạo (từ 0 đến 2). Số càng cao thì AI viết càng "bay
  bổng", số thấp thì AI trả lời nghiêm túc và thực tế.

---

### **2\. Các ứng dụng thực tế đỉnh cao**

1. **Tự động trả lời tin nhắn (Auto Reply):**
   - Dùng lệnh **Get Text** lấy tin nhắn của khách hàng.
   - Gửi tin nhắn đó qua OpenAI kèm prompt: "Bạn là nhân viên hỗ trợ, hãy trả
     lời khách này thật lịch sự".
   - Dùng lệnh **Type Text** để điền câu trả lời của AI và gửi đi.
2. **Viết nội dung không trùng lặp (Content Generation):**
   - Thay vì dùng Spintax thông thường, bạn yêu cầu OpenAI viết 100 bài đăng
     khác nhau về cùng một chủ đề. Điều này giúp tránh việc bị các nền tảng quét
     nội dung rác (spam).
3. **Vượt thử thách văn bản (Text-based CAPTCHA):**
   - Dùng OpenAI để giải các câu đố logic hoặc câu hỏi xác minh danh tính mà các
     tool giải captcha thông thường không làm được.
4. **Phân tích cảm xúc (Sentiment Analysis):**
   - Quét 100 bình luận về đối thủ, gửi cho AI để nó phân loại đâu là bình luận
     tích cực, đâu là tiêu cực, rồi lưu kết quả vào **Spreadsheet**.

---

### **3\. Cách cấu hình HTTP Request cho OpenAI (Dành cho dân chuyên)**

Nếu bạn muốn tùy biến sâu, hãy dùng lệnh **HTTP Request**:

- **URL:** `https://api.openai.com/v1/chat/completions`
- **Method:** `POST`
- **Headers:**
  - `Authorization`: `Bearer YOUR_API_KEY`
  - `Content-Type`: `application/json`
- **Body (JSON):**

JSON\
{\
"model": "gpt-4o",\
"messages": \[{"role": "user", "content": "Hãy viết 1 status Facebook về
{{topic}}"}\]\
}

## \- gemini

Trong hệ sinh thái NovaCore, việc tích hợp **Gemini** (mô hình ngôn ngữ lớn từ
Google) mang lại lợi thế cực lớn về tốc độ xử lý và khả năng hiểu ngữ cảnh hình
ảnh, giúp kịch bản tự động hóa của bạn đạt tới mức độ "siêu trí tuệ".

Dưới đây là các thông số và cách thức ứng dụng Gemini vào kịch bản:

---

### **1\. Các thông số cấu hình (Params)**

Tương tự như OpenAI, bạn có thể kết nối Gemini thông qua lệnh **HTTP Request**
hoặc tích hợp trực tiếp qua API Key:

- **API Key:** Lấy từ Google AI Studio (thường có gói miễn phí rất hào phóng cho
  nhà phát triển).
- **Model:**
  - `gemini-3.1-flash`: Thế hệ mới nhất, tốc độ ánh sáng, cực kỳ rẻ cho các tác
    vụ xử lý văn bản quy mô lớn.
  - `gemini-3.1-pro`: Trí tuệ đỉnh cao của Google, chuyên trị các logic đa
    phương thức và kịch bản agentic.
  - `gemini-3.1-think`: Chế độ tư duy sâu để giải quyết các vấn đề kỹ thuật khó
    nhất.
- **Prompt:** Câu lệnh gửi cho Gemini.
- **Variable Name:** Tên biến để lưu văn bản phản hồi từ Gemini.
- **Max Tokens & Temperature:** Tương tự như OpenAI, giúp kiểm soát độ dài và sự
  sáng tạo của câu trả lời.
- **System Instruction:** Thiết lập "tính cách" cho AI (Ví dụ: "Bạn là một
  chuyên gia hỗ trợ khách hàng trên Shopee").

---

### **2\. Điểm khác biệt vượt trội của Gemini**

So với các AI khác, Gemini có những khả năng đặc biệt mà dân làm Automation rất
cần:

- **Xử lý đa phương thức (Multimodal):** Gemini có thể đọc hiểu hình ảnh. Bạn có
  thể dùng lệnh **Screenshot** màn hình, gửi ảnh đó cho Gemini và hỏi: "Nút đăng
  ký nằm ở đâu?" hoặc "Giải mã ký tự trong hình ảnh này".
- **Cửa sổ ngữ cảnh (Context Window) lớn:** Bạn có thể đưa toàn bộ mã nguồn
  (HTML) của một trang web vào và yêu cầu Gemini: "Tìm cho tôi ID của tất cả các
  sản phẩm đang giảm giá".
- **Tích hợp hệ sinh thái Google:** Dễ dàng kết hợp với dữ liệu từ Google
  Sheets, Drive mà bạn đang sử dụng trong NovaCore.

---

### **3\. Các ứng dụng thực tế trong Automation**

1. **Vượt Captcha hình ảnh:** \* Chụp ảnh màn hình khu vực Captcha.
   - Gửi ảnh cho Gemini với Prompt: "Hãy trả lời chỉ bằng các ký tự có trong ảnh
     này".
   - Lấy kết quả trả về để điền vào ô nhập liệu.
2. **Tự động viết mô tả sản phẩm (Dropshipping):**
   - Crawl link ảnh sản phẩm từ trang đối thủ bằng lệnh **Get Attribute**.
   - Gửi link ảnh đó cho Gemini: "Dựa vào ảnh này, hãy viết một đoạn quảng cáo
     hấp dẫn bằng tiếng Việt".
3. **Phân tích dữ liệu đối thủ:**
   - Quét nội dung bài viết của đối thủ, dùng Gemini để tóm tắt các chiến lược
     giá hoặc khuyến mãi mà họ đang áp dụng.

---

### **4\. Cách thiết lập HTTP Request cho Gemini**

- **URL:**
  `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=YOUR_API_KEY`
- **Method:** `POST`
- **Body (JSON):**

JSON\
{\
"contents": \[{\
"parts":\[{"text": "Hãy viết 1 bình luận về: {{product\_name}}"}\]\
}\]\
}

## \- Grok (xAI)

Node **Grok** cho phép bạn tích hợp trí tuệ nhân tạo từ xAI của Elon Musk vào
quy trình NovaCore. Grok nổi tiếng với khả năng cập nhật thông tin thời gian
thực và phong cách trả lời độc đáo.

### **1\. Các thông số cấu hình (Params)**

- **API Key:** Lấy từ cổng dành cho nhà phát triển xAI.
- **Model:** Chọn phiên bản Grok tương ứng (ví dụ: `grok-2`, `grok-2-vision`,
  `grok-1.5`).
- **Prompt:** Nội dung bạn muốn Grok xử lý.
- **Variable Name:** Biến lưu câu trả lời (ví dụ: `grok_response`).
- **Max Tokens & Temperature:** Kiểm soát giới hạn tokens và độ ngẫu nhiên của
  câu trả lời.

### **2\. Tại sao chọn Grok cho Automation?**

- **Dữ liệu thời gian thực:** Grok có thể phân tích các xu hướng (trends) vừa
  mới xảy ra cách đây vài phút. Điều này cực kỳ hữu ích cho các tool săn tin
  tức, săn kèo crypto hoặc bắt trend TikTok/Facebook.
- **Phong cách ngôn ngữ:** Grok có xu hướng trả lời trực diện, cá tính và ít bị
  các rào cản "kiểm duyệt" quá đà như các AI khác, giúp nội dung bạn tạo ra
  trông "thật" và ít giống bot hơn.
- **Xử lý ngữ cảnh X (Twitter):** Nếu bạn làm tool chuyên về mảng X, Grok là sự
  lựa chọn số 1 để hiểu các tiếng lóng, ngữ cảnh và các cuộc hội thoại đang diễn
  ra trên nền tảng này.

---

### **2\. Các thông số cấu hình (Params)**

Hiện tại, cách ổn định nhất để dùng Grok trong NovaCore là thông qua **API (xAI
Console)** bằng lệnh **HTTP Request**:

- **Endpoint URL:** `https://api.x.ai/v1/chat/completions`
- **Method:** `POST`
- **Headers:**
  - `Authorization`: `Bearer YOUR_XAI_API_KEY`
  - `Content-Type`: `application/json`
- **Body (JSON):** Bạn có thể tùy chỉnh các tham số như `model` (ví dụ:
  `grok-beta`), `messages` (nội dung câu hỏi), và `temperature`.

---

### **3\. Các ứng dụng thực tế trong NovaCore**

1. **Săn Trend & Đăng bài tự động:**
   - Sử dụng Grok để hỏi về các chủ đề đang hot nhất trong 1 giờ qua.
   - Yêu cầu Grok viết một bài luận hoặc status về chủ đề đó.
   - Dùng kịch bản NovaCore để đăng lên các nhóm Facebook/Telegram/X để kéo
     tương tác.
2. **Phân tích thị trường Crypto/NFT:**
   - Dùng lệnh **Get Text** lấy các thông số giá hoặc tweet của các KOL.
   - Gửi dữ liệu đó cho Grok để nó phân tích tâm lý thị trường (Sentiment) và
     đưa ra quyết định: _Mua, Bán_ hoặc _Đứng ngoài_.
3. **Tương tác "Cà khịa" (Sarcastic Reply):**
   - Thiết lập Grok ở chế độ "Fun mode" để tự động đi bình luận trên các bài
     viết. Nội dung của Grok thường hài hước và có tính tương tác cao hơn các
     câu trả lời khuôn mẫu.

---

### **4\. Ví dụ cấu hình Body JSON cho Grok**

JSON\
{\
"model": "grok-beta",\
"messages": \[\
{\
"role": "system",\
"content": "Bạn là một chuyên gia về thị trường Crypto, hãy trả lời ngắn gọn và
hài hước."\
},\
{\
"role": "user",\
"content": "Phân tích giúp tôi xu hướng của {{coin\_name}} trong 24h qua dựa
trên dữ liệu này: {{market\_data}}"\
}\
\],\
"stream": false\
}

## \- Case Path

### **1\. Các thông số chi tiết (Params)**

- **Variable to Check (Biến cần kiểm tra):** Đây là biến chứa giá trị mà bạn
  muốn đưa ra "phán quyết".
  - _Ví dụ:_ {{status}}, {{current\_url}}, hoặc {{account\_type}}.
- **Case Values (Các trường hợp):** Bạn thiết lập các giá trị cụ thể để so sánh.
  - _Case 1:_ Nếu biến bằng Thành công \-\> Đi theo nhánh A.
  - _Case 2:_ Nếu biến bằng Lỗi \-\> Đi theo nhánh B.
  - _Case 3:_ Nếu biến bằng Checkpoint \-\> Đi theo nhánh C.
- **Default Path (Nhánh mặc định):** Nếu giá trị của biến không khớp với bất kỳ
  Case nào ở trên, kịch bản sẽ đi theo nhánh này. Điều này giúp kịch bản không
  bị dừng đột ngột (Crash).

---

### **2\. Sự khác biệt giữa If/Else và Case Path**

| Đặc điểm        | If / Else                             | Case Path (Switch Case)             |
| :-------------- | :------------------------------------ | :---------------------------------- |
| **Số nhánh**    | Chỉ có 2 (Đúng hoặc Sai)              | Nhiều nhánh (3, 4, 5... tùy ý)      |
| **Độ phức tạp** | Dễ gây rối nếu lồng nhiều If vào nhau | Rất gọn gàng, dễ quản lý và bảo trì |
| **Ứng dụng**    | Kiểm tra 1 điều kiện đơn giản         | Phân loại trạng thái đa dạng        |

## \- RegExp (Data extraction)

### **1\. Các thông số chi tiết (Params)**

- **Input String (Chuỗi đầu vào):** Văn bản gốc mà bạn muốn trích xuất dữ liệu
  (thường là một biến như {{source\_code}} hoặc {{email\_content}}).
- **RegExp Pattern (Biểu thức chính quy):** "Công thức" để tìm kiếm dữ liệu.
  - _Ví dụ:_ \\d{6} (Tìm một dãy có đúng 6 chữ số).
- **Match Index (Chỉ số khớp):** Nếu trong văn bản có nhiều kết quả trùng khớp:
  - 0: Lấy kết quả đầu tiên tìm thấy.
  - 1: Lấy kết quả thứ hai.
  - All: Lấy tất cả và lưu thành một danh sách (Array).
- **Variable Name (Tên biến lưu trữ):** Tên biến để chứa kết quả sau khi lọc (ví
  dụ: otp\_code).

---

### **2\. Các mẫu RegExp phổ biến cho dân Automation**

Dưới đây là những "công thức" bạn sẽ dùng tới 90% thời gian làm tool:

| Mục tiêu trích xuất | Biểu thức RegExp                       | Giải thích                                      |
| :------------------ | :------------------------------------- | :---------------------------------------------- |
| **Mã OTP (6 số)**   | \\d{6}                                 | Tìm chuỗi gồm 6 ký tự số liên tiếp.             |
| **Địa chỉ Email**   | \[\\w.-\]+@\[\\w.-\]+\\.\[a-zA-Z\]{2,} | Lọc ra các định dạng email chuẩn.               |
| **Số điện thoại**   | \`(0                                   | \+84)\\d{9,10}\`                                |
| **Link URL**        | https?://\[^\\s\\"'\]+                 | Lấy các đường dẫn bắt đầu bằng http hoặc https. |
| **Số tiền**         | \[\\d,.\]+                             | Lấy các con số có chứa dấu phẩy hoặc dấu chấm.  |

## \- IMAP (Read mail)

Component **IMAP (Read Mail)** trong NovaCore là công cụ cực kỳ quan trọng giúp
tự động hóa quy trình **vượt mã xác thực (OTP)**, kích hoạt tài khoản hoặc kiểm
tra thông báo mà không cần phải mở trình duyệt để đăng nhập vào hộp thư (như
Gmail, Outlook, Yahoo).

Việc đọc mail qua IMAP giúp kịch bản chạy nhanh hơn, ẩn danh tốt hơn và tránh bị
các nhà cung cấp dịch vụ email quét thiết bị.

---

### **1\. Các thông số cấu hình (Params) chi tiết**

Để kết nối với một hộp thư qua IMAP, bạn cần các thông tin sau:

- **IMAP Host:** Địa chỉ máy chủ IMAP.
  - _Gmail:_ `imap.gmail.com`
  - _Outlook/Hotmail:_ `outlook.office365.com`
  - _Mail cổ (Yahoo, v.v.):_ Tùy thuộc vào nhà cung cấp.
- **Port:** Cổng kết nối (thường là `993` cho bảo mật SSL/TLS).
- **Email / Username:** Địa chỉ email cần đọc.
- **Password / App Password:**
  - **Lưu ý:** Với Gmail hoặc Outlook hiện nay, bạn không dùng mật khẩu chính mà
    phải tạo **App Password** (Mật khẩu ứng dụng) trong phần bảo mật tài khoản
    để NovaCore có thể truy cập.
- **Search Criteria (Điều kiện tìm kiếm):** Giúp bạn lọc đúng thư cần đọc.
  - _Ví dụ:_ Tìm thư từ người gửi `noreply@facebook.com` hoặc thư có tiêu đề
    chứa chữ `OTP`.
- **Variable Name:** Biến lưu nội dung email trả về (thường lưu toàn bộ nội dung
  text hoặc HTML của thư).
- **Extract OTP (Tự động lấy mã):** Nếu bật, hệ thống sẽ tự động làm sạch nội
  dung thư (loại bỏ HTML, chuẩn hóa khoảng trắng) và tìm kiếm dãy 4-10 chữ số
  (ưu tiên mã 6 số) trong tiêu đề và nội dung. Kết quả sẽ được lưu vào biến OTP
  bạn chỉ định.

---

### **2\. Luồng xử lý OTP điển hình với IMAP**

Sự kết hợp giữa **IMAP** và **RegExp** là cặp bài trùng "bất bại":

1. **Bước 1 (Trên Web):** Click nút "Gửi mã OTP" trên trang đăng ký/đăng nhập.
2. **Bước 2 (NovaCore):** Dùng lệnh **IMAP** để kết nối vào hộp thư.
   - Thiết lập lệnh chờ (Wait) khoảng 10-20 giây để thư kịp về.
   - Lọc thư mới nhất từ dịch vụ đó.
3. **Bước 3 (RegExp):** Trích xuất mã 6 số hoặc 8 số từ nội dung thư vừa đọc
   được qua IMAP.
4. **Bước 4 (Trên Web):** Dùng lệnh **Type Text** điền mã vừa trích xuất vào ô
   nhập liệu.

---

### **3\. Ưu điểm so với việc mở Tab Email trên trình duyệt**

- **Tốc độ:** Truy cập dữ liệu trực tiếp từ server, không cần load hình ảnh,
  CSS, JS của trang web email.
- **Tiết kiệm tài nguyên:** Không cần mở thêm tab trình duyệt, giúp máy chạy
  được nhiều luồng (Threads) hơn.
- **Bảo mật:** Tránh việc trang web dịch vụ (như Facebook/Google) phát hiện bạn
  đang mở nhiều tài khoản email trên cùng một trình duyệt.

## \- Update field

### **1\. Cập nhật vào Google Sheets / Excel**

Lệnh này cho phép bạn sửa đổi nội dung của một ô (cell) hoặc một hàng dựa trên
tọa độ xác định.

- **Row Index (Chỉ số hàng):** Hàng bạn muốn cập nhật (thường dùng biến
  {{loop\_index}} để cập nhật đúng hàng đang xử lý).
- **Column Name / Index (Cột):** Tên cột (ví dụ: Status) hoặc số thứ tự cột (ví
  dụ: C).
- **Value (Giá trị mới):** Nội dung bạn muốn ghi vào.
  - _Ví dụ:_ Cập nhật cột "Trạng thái" từ Đang chạy thành Thành công.
- **Ứng dụng:** Đánh dấu tài khoản đã được "nuôi" xong hoặc cập nhật số dư ví
  sau khi quét.

---

### **2\. Cập nhật vào Cơ sở dữ liệu (Database) của NovaCore**

Nếu kịch bản của bạn sử dụng tính năng **Database** tích hợp sẵn, lệnh này sẽ
giúp thay đổi dữ liệu của một bản ghi (Record).

- **Key Field (Trường khóa):** Dùng để định danh bản ghi cần sửa (thường là ID
  hoặc Email).
- **Field to Update:** Chọn trường dữ liệu cụ thể (ví dụ: Cookie, Token,
  Last\_Login).
- **New Value:** Giá trị mới lấy từ các biến trong kịch bản.

---

### **3\. Cập nhật thông tin Profile (Update Profile Field)**

Một số phiên bản NovaCore cung cấp lệnh này để thay đổi thuộc tính của Profile
ngay khi đang chạy.

- **Field Type:** Các trường như Proxy, User-Agent, Note hoặc Name.
- **Ứng dụng:**
  - Tự động đổi tên Profile sau khi đăng ký tài khoản thành công để dễ quản lý
    (ví dụ: đổi từ Profile 1 thành Acc\_FB\_NguyenVanA).
  - Ghi chú (Note) lại các lỗi xảy ra để bạn có thể lọc ra các Profile lỗi sau
    này.

---

### **Sự khác biệt giữa Insert Data và Update Field**

| Tính chất      | Insert Data (Chèn)                         | Update Field (Cập nhật)                           |
| :------------- | :----------------------------------------- | :------------------------------------------------ |
| **Vị trí**     | Tạo một hàng mới ở cuối danh sách.         | Sửa dữ liệu tại một vị trí đã có sẵn.             |
| **Mục đích**   | Lưu kết quả mới, thu thập dữ liệu (Crawl). | Thay đổi trạng thái, cập nhật thông tin mới nhất. |
| **Dữ liệu cũ** | Được giữ nguyên.                           | Bị ghi đè bởi dữ liệu mới.                        |

## \- Nhóm node (Group Node)

Component **Nhóm node** là một công cụ tổ chức mạnh mẽ giúp bạn quản lý các kịch
bản dài và phức tạp bằng cách gom các bước liên quan lại vào một "khối" duy
nhất.

### **1\. Tác dụng chính**

- **Gọn gàng:** Thu nhỏ hàng chục node vào một khối nhỏ, giúp bạn dễ dàng nhìn
  tổng quan quy trình.
- **Tổ chức logic:** Gom các node có cùng chức năng (ví dụ: nhóm "Đăng nhập",
  nhóm "Quét dữ liệu") lại với nhau.
- **Sao chép nhanh:** Bạn có thể di chuyển hoặc sao chép cả một nhóm node thay
  vì phải làm từng cái một.

### **2\. Cách sử dụng**

1. **Tạo Nhóm:** Kéo node **Nhóm node** từ Sidebar vào màn hình thiết kế.
2. **Thêm Node vào Nhóm:** Kéo bất kỳ node nào khác và thả vào **bên trong**
   khung của Nhóm node. Khung nhóm sẽ tự động giãn nở để chứa các node con.
3. **Đặt tên:** Bạn có thể đổi tên Nhóm trong bảng Thuộc tính (ví dụ:
   `[1] Log-in Facebook` để dễ phân biệt).

### **3\. Tính năng Thu gọn / Mở rộng**

- Trên mỗi Nhóm node có một nút biểu tượng (dấu `+` hoặc dấu mũi tên).
- **Thu gọn:** Khi bạn nhấn thu gọn, toàn bộ các node con bên trong sẽ biến mất
  và nhóm chỉ còn là một khối nhỏ. Điều này rất hữu ích khi kịch bản của bạn có
  hàng trăm node.
- **Mở rộng:** Nhấn lại để hiện ra toàn bộ các node con và tiếp tục chỉnh sửa.

---

# Thẻ 5

- App đặt xe
- Người lái xe, người đặt xe\
  Thuật toán phù hợp bắn người đặt xe cho người lái, tìm kiếm xe, phân bổ lái xe
