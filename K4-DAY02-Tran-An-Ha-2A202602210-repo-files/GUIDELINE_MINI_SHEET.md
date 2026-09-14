
# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Trần An Hạ





**MSSV:** 2A202602210



**Hình thức:** cá nhân



**Mã cặp:** SOLO

## 1. Phạm vi

* Chỉ gán phương tiện thuộc bốn lớp bên dưới.
* Mỗi phương tiện là một hộp; không gộp nhiều xe.
* Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
* Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| --- | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

* Vẽ sát phần vật thể nhìn thấy.
* Không ước lượng phần bị xe khác che.
* Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
* Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

* Ảnh và mã vật thể: drive_038 hộp 2
* Dấu hiệu nhìn thấy: Thân xe dài, có nhiều cửa sổ trải dài theo khoang hành khách
* Quy tắc áp dụng: Gán lớp bus thay vì van
* Quyết định: Lớp bus
* Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì: Gắn cờ cần review và chọn thuộc tính unclear

### Tình huống B — xe tải hay xe van/ô tô con?

* Ảnh và mã vật thể: drive_022 hộp 5
* Dấu hiệu nhìn thấy: Có phần thùng hàng hình khối hộp tách biệt rõ ràng phía sau
* Quy tắc áp dụng: Gán lớp truck
* Quyết định: Lớp truck
* Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì: Loại bỏ khỏi tập gán nhãn nếu mức độ mờ nhạt khiến dấu hiệu bị mất hoàn toàn.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

* Ảnh và mã vật thể: drive_033 hộp 8
* Dấu hiệu nhìn thấy khi phóng 100%: Xe bị cắt bởi mép khung hình và khuất một phần phía sau xe tải.
* Giá trị `visibility`: Bị che khuất
* Giá trị `boundary`: Bị mép ảnh cắt
* Trạng thái `review_state`: Tự tin
* Lý do: Vẫn nhận diện rõ phần mui xe con để gán lớp car.

## 6. Xác nhận tự kiểm tra

* [x] Đã rà đủ bốn ảnh.
* [x] Đã kiểm vật thể thiếu và trùng.
* [x] Đã kiểm lớp và hình học từng hộp.
* [x] Mỗi hộp có đủ ba thuộc tính.
* [x] Đã xử lý mọi hộp `needs_review`.
* [x] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
* [x] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
* [x] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
* [x] Số vật thể thực tế: 110 — 40–60 là mục tiêu khối lượng, không phải điểm cắt.

```

```