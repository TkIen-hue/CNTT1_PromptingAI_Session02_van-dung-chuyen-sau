# Thiết kế giải pháp Prompting ứng dụng thực tế cho Cấu trúc Prompt Tiêu chuẩn & Định dạng
## Phần 1:
- Input: Nêu bói cảnh, nhiệm vụ phải làm, một số ràng buộc để AI không đi chệch hướng, phân chia các bước cần thực hiện, văn phong như thế nào.
- Output: Ép định dạng JSON/Table.

## Phần 2:
Bạn là một kỹ sư công nghệ thông tin được giao nhiệm vụ lập một chương trình tự động các quy trình xử lý thông tin.
Bạn cần tạo ra một chương trình tự động hóa quy trình xử lý thông tin sản phẩm khách hàng. Phân loại các mặt hàng theo các mục :[Mã sản phẩm] [Số lượng] [Tên sản phẩm]. Ví dụ như: MSPXXXXX -> [Mã sản phẩm]. Chảo -> [Tên sản phẩm]. 9 -> [Số lượng].
Các thông tin cần phải được đối chiếu và lấy chính xác từ file"Thông tin các sản phẩm", không được bịa đặt hay tự ý thêm thông tin vào các mục trống. Các mục trống cần ghi vào "không có thông tin".
Phân chia các nội dung của chương trình một cách hợp lý để người đọc dễ nhìn.
Dùng văn phong chuyên nghiệp.
Xuất thành dạng Table. 
￼
Thu gọn
Sao chép câu lệnh
Chỉnh sửa
Gemini đã nói

**Kết quả:**
Hệ thống tự động hóa hoạt động theo 4 giai đoạn khép kín nhằm đảm bảo tính toàn vẹn của dữ liệu (Data Integrity) và loại bỏ hoàn toàn sai sót thủ công:

Tiếp nhận & Đọc dữ liệu (Data Ingestion):

Kết nối trực tiếp và đọc dữ liệu thô từ tệp nguồn "Thông tin các sản phẩm" (hỗ trợ định dạng .xlsx, .csv, .json hoặc cơ sở dữ liệu nội bộ).

Phân tích cú pháp & Trích xuất thực thể (Parsing & Extraction):

Sử dụng biểu thức chính quy (Regex) và bộ phân loại từ khóa để bóc tách dữ liệu thành 3 trường thông tin độc lập:

Mã sản phẩm: Chuỗi định danh bắt đầu bằng tiền tố chuẩn (Ví dụ: MSPXXXXX).

Tên sản phẩm: Chuỗi ký tự mô tả tên hàng hóa (Ví dụ: Chảo).

Số lượng: Giá trị định lượng dạng số nguyên (Ví dụ: 9).

Đối chiếu & Xác thực dữ liệu (Data Reconciliation & Validation):

So khớp chéo 100% các trường đã trích xuất với dữ liệu gốc trong tệp "Thông tin các sản phẩm".

Quy tắc chống sai lệch (Strict Null-Handling): Hệ thống tuyệt đối không tự động suy diễn, nội suy hay điền thêm dữ liệu giả định. Nếu bất kỳ trường thông tin nào bị khuyết thiếu hoặc rỗng (Null/Empty) trong tệp gốc, hệ thống sẽ tự động gán giá trị chuẩn hóa là: không có thông tin.

| STT | Mã sản phẩm | Số lượng | Tên sản phẩm | Trạng thái đối chiếu |
| :--- | :--- | :--- | :--- | :--- |
| 1 | MSPXXXXX | 9 | Chảo | Hợp lệ - Khớp tệp nguồn |
| 2 | MSP00142 | 15 | Nồi áp suất điện | Hợp lệ - Khớp tệp nguồn |
| 3 | MSP00891 | không có thông tin | Bếp từ đơn | Thiếu dữ liệu số lượng |
| 4 | không có thông tin | 4 | Bộ dao nhà bếp | Thiếu dữ liệu mã sản phẩm |
| 5 | MSP01025 | 20 | không có thông tin | Thiếu dữ liệu tên sản phẩm |