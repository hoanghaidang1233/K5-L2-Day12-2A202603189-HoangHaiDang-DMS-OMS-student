# Danh sách lớp v1 — Hoàng Hải Đăng · G05

## DMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Thấy trên IR? |
|---|---|---|---|---|
| 1 | `eye_left`, `eye_right` | open / closed / unknown | Phát hiện buồn ngủ (mắt nhắm lâu) | Có, trừ khi kính phản xạ |
| 2 | `gaze_zone` | road / mirror_* / cluster / center / lap / other | Cảnh báo không nhìn đường | Có (hướng đầu, mắt) |
| 3 | `face13` | 13 điểm + cờ occluded / outside | Ước lượng hướng đầu, độ mở mắt/miệng | Có; điểm bị che thì đánh cờ |
| 4 | `glasses` | unknown / none / clear / sun | Biết khi nào kết quả mắt kém tin cậy | Một phần (kính râm có thể trông trong) |
| 5 | `hand` → `on_wheel` | yes / no / unknown | Kiểm tra tay trên vô lăng khi chuyển sang lái tay | Có |
| 6 | `phone` → `state` | held / mounted / lap | Cảnh báo dùng điện thoại khi lái | Có nếu thấy hình dạng máy |
| 7 | `seatbelt` (tài xế) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây, phát hiện thắt sai | Một phần (dây có thể chìm) |

## OMS

| # | Lớp / thuộc tính | Giá trị | Để làm gì | Thấy trên IR? |
|---|---|---|---|---|
| 1 | `person` → `seat` | driver / front_passenger / rear_* | Đếm người theo ghế, nhắc dây an toàn đúng ghế | Có |
| 2 | `seatbelt` (hàng sau) | not_worn / unknown | Nhắc hành khách ghế sau thắt dây | Một phần |
| 3 | `child_seat` → `occupied` | no / unknown | Tắt túi khí trước, nhắc kiểm tra trẻ khi khóa xe | Có (hình dạng) |
| 4 | `infant_seat` → `facing` | rear / front | Ghế quay ngược ở ghế trước thì phải tắt túi khí | Có (hình dạng) |
| 5 | `object` → `type` | bag / box / cup / other | Phân biệt vật với người; nhắc quên đồ | Có (hình dạng, không theo màu) |
| 6 | `face` (chỉ box) | box | Xác nhận là người thật, không phải túi/gối | Có |
| 7 | `frame_meta` → `camera_blocked` | yes / no | Báo camera bị che thay vì kết luận "ghế trống" | Có |

## 2 câu hỏi leo thang

1. Câu hỏi: Ghế trẻ em có hình giống trẻ em bên trong thì gán `occupied = yes` và thêm box `face` không? Guideline 4.8 cấm `yes` nhưng schema coach cập nhật lại cho phép.
   Khung ví dụ: G05 S08, ghế `rear_left`.
   Tạm gán: `occupied = yes` kèm box `face`, theo schema coach cập nhật.
2. Câu hỏi: Ảnh NIR thật có ghi `light = night` được không, khi ảnh không cho biết giờ trong ngày?
   Khung ví dụ: G05 D01–D06 (bạn ghi `night`, mình ghi `unknown`).
   Tạm gán: `unknown`, theo 4.4-1 (không thấy thì unknown).
