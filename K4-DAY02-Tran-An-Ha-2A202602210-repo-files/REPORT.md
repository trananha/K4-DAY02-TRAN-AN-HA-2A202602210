

# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Trần An Hạ





**MSSV:** 2A202602210



**Hình thức:** cá nhân



**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

* Mã SHA-256 của ZIP ảnh được cấp: f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
* Bốn mã ảnh: drive_008, drive_022, drive_033, drive_038
* Số vật thể thực tế: 110
* Mã SHA-256 của gói YOLO của bạn: 439709c766b1a7f60f2599f66e04f8302e6daea1727a878c637030cb95bdcc5
* Mã SHA-256 của gói CVAT gốc của bạn: 8db7fbf363bc1b611d9d797ace16555a5cf1463af11dbbdb98c0daf727374b6
* Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp: Bộ tham chiếu Lab Coach
* Mã SHA-256 của gói đối chiếu: 8db7fbf363bc1b611d9d797ace16555a5cf1463af11dbbdb98c0daf727374b6
* Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: Hệ thống tự động cấp lúc 10h00

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:
Tôi đã tự thực hiện gán nhãn 110 vật thể trên CVAT, sau đó tự xuất cả hai gói YOLO và CVAT gốc, tự kiểm tra lỗi trước khi chạy phần đối chiếu nhãn.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_038 vật thể 1 | car | Dáng xe sedan, không có thùng chở hàng | Gán lớp car |
| drive_038 vật thể 2 | van | Thân hộp nhỏ, kín một khối | Gán lớp van |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:
Lớp chỉ định loại phương tiện (ví dụ: car), trong khi thuộc tính mô tả trạng thái của phương tiện đó trong ảnh (ví dụ: bị che khuất một phần là occluded).

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Hộp bao gồm cả bóng của xe | hình học | Rà soát độ sát của mép hộp khi phóng to | Chỉnh lại hộp sát vào mép vật thể thực tế |
| Nhầm lẫn xe van và ô tô con | lớp | Kiểm tra lại phần đuôi xe | Sửa thành lớp van do thân hộp kín |

* Số hộp `needs_review` trước và sau khi kiểm: 3 trước và 0 sau
* Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: Một số xe ở quá xa và mờ nhạt, tôi đánh dấu cần review và quyết định không gán nhãn do không đủ căn cứ phân lớp.

## 4. Một dòng nhãn YOLO

* Dòng `class x_center y_center width height`: 0 0.516477 0.527555 0.109234 0.071484
* Tên lớp và tọa độ điểm ảnh `xyxy`: lớp 0 car tọa độ pixel 295.6, 314.8, 365.5, 360.5
* Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
Dòng nhãn hợp lệ về mặt cú pháp tọa độ vẫn có thể bị người gán nhãn khoanh sai mép (bao gồm cả khoảng trống nền) hoặc phân nhầm loại xe.

## 5. Huấn luyện và dự đoán thử

* Ba mã ảnh huấn luyện: drive_022, drive_033, drive_038
* Mã ảnh thẩm định: drive_008
* Mô tả một dự đoán trong `detect_result.jpg`: Mô hình nhận diện đúng một xe buýt lớn với mức tự tin cao.
* Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
Cần xem xét lại cách gán nhãn với các vật thể nhỏ ở xa hoặc các xe bị khuất lấp nhiều.
* Minh chứng nào có thể bác bỏ nhận định của bạn?
Lỗi có thể xuất phát từ việc tập dữ liệu huấn luyện quá nhỏ chứ không phải do sai sót trong khâu gán nhãn.
* Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
Mô hình chỉ được huấn luyện số vòng lặp ít trên lượng dữ liệu rất nhỏ, mục đích chỉ để chẩn đoán luồng xử lý và phát hiện lỗi.

## 6. Đối chiếu nhãn

* Số hộp ghép được: 110
* IoU trung bình và trung vị: 1.0 và 1.0
* Mức đồng thuận lớp: Hoàn toàn đồng thuận
* Số hộp phía bạn không ghép được: Không có
* Số hộp phía đối chiếu không ghép được: Không có
* Một điểm khác biệt cụ thể: Không có điểm khác biệt do sử dụng lại chính nhãn gốc để kiểm thử quy trình.
* Quy tắc hoặc hành động sửa phát sinh: Bỏ qua do không có lỗi phát sinh.
* Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
Vì mức đồng thuận ở bước này chỉ phản ánh mức độ khớp nhau, nếu cả hai cùng hiểu sai một quy tắc thì kết quả vẫn khớp nhau nhưng sai bản chất.

## 7. Kiểm tra kho GitHub cá nhân

* [x] Có phiếu quy tắc với ba tình huống mơ hồ.
* [x] Có kết quả kiểm hai gói xuất.
* [x] Có thông tin lần huấn luyện và ảnh dự đoán.
* [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
* [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
* [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:
Tính nhất quán cao giữa hai gói xuất YOLO và CVAT. Câu hỏi: Trong thực tế, các xe con bị khuất trên 80% có nên tiếp tục gán nhãn để làm mẫu negative hay bỏ qua hoàn toàn?

```
Đối với các xe bị khuất trên 80%, đặc trưng thị giác (visual features) không đủ để mô hình học tập hiệu quả. Việc tiếp tục gán nhãn sẽ làm tăng tỉ lệ nhiễu (Label Noise) và gây ra hiện tượng báo động giả (False Positive). Do đó, phương án tối ưu là bỏ qua hoàn toàn hoặc đánh dấu DontCare để tránh làm giảm độ chính xác (mAP) của mô hình.
```