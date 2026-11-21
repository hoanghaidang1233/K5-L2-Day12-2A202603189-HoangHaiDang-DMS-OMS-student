# Danh sách lớp v0 — Hoàng Hải Đăng · G05

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` → `eye_left`, `eye_right` | open / closed / unknown | Phát hiện buồn ngủ: đếm thời gian mắt nhắm liên tục (PERCLOS), cảnh báo ngủ gật | Có, trừ khi đốm phản xạ LED trên kính hoặc kính râm che mắt (khi đó `unknown`) |
| 2 | `face` → `gaze_zone` | road / mirror_l / mirror_r / mirror_c / cluster / center / lap / other | Phát hiện mất tập trung: tài xế không nhìn đường quá N giây thì cảnh báo; biết tài xế có kiểm tra gương trước khi đổi làn | Có, nhờ hướng đầu và vị trí đồng tử trên ảnh NIR |
| 3 | `face13` (13 điểm mặt) | 13 điểm + cờ occluded / outside | Ước lượng hướng đầu (yaw/pitch) và độ mở mắt, miệng; làm đầu vào cho tính buồn ngủ, ngáp, hướng nhìn | Có; điểm bị phản xạ hoặc tay che phải đánh cờ, không đoán |
| 4 | `face` → `glasses` | unknown / none / clear / sun | Biết khi nào kết quả mắt kém tin cậy (kính phản xạ, kính râm) để hạ độ tin cậy cảnh báo buồn ngủ | Một phần: trên NIR kính râm có thể trông trong suốt, nên không chắc thì `unknown` |
| 5 | `hand` → `on_wheel`, `side`, `whose` | yes / no / unknown; left / right; driver / passenger | Hands-on-wheel detection: khi xe chuyển từ tự lái sang lái tay, kiểm tra tài xế đã đặt tay lên vô lăng | Có (hình dạng tay); vô lăng ngoài khung thì `unknown` |
| 6 | `phone` → `state` | held / mounted / lap | Phát hiện dùng điện thoại khi lái (cầm áp tai, nhắn tin trên đùi), cảnh báo phân tâm | Có nếu thấy cạnh, góc hoặc màn hình; vật tối áp tai mà không thấy hình dạng thì không vẽ |
| 7 | `person` (driver) → `seatbelt` | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây an toàn; phát hiện thắt sai (dây dưới nách) để điều chỉnh túi khí | Một phần: dây tối trên áo tối có thể chìm, khi đó `unknown` |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` → `seat` | driver / front_passenger / rear_left / rear_middle / rear_right | Đếm người theo từng ghế: nhắc dây an toàn đúng ghế, bật/tắt túi khí theo ghế có người | Có (hình dạng người) |
| 2 | `person` → `seatbelt` (hàng sau) | not_worn / unknown (SVIRO) | Nhắc hành khách ghế sau thắt dây (luật nhiều nước bắt buộc) | Một phần: dây chìm trong bóng tối thì `unknown` |
| 3 | `child_seat` → `seat`, `occupied` | theo ghế; no / unknown | Biết ghế nào có ghế trẻ em để tắt túi khí phía trước, nhắc kiểm tra trẻ khi khóa xe | Có (hình dạng ghế), bên trong ghế có thể khó thấy |
| 4 | `infant_seat` → `seat`, `facing` | theo ghế; rear / front | Ghế em bé quay ngược đặt ở ghế trước thì **bắt buộc tắt túi khí**; nhắc quên trẻ trong xe | Có (hình dạng), hướng quay nhìn qua tay cầm và phần lưng ghế |
| 5 | `object` → `seat`, `type` | theo ghế; bag / box / cup / other | Phân biệt vật với người để không báo nhầm "có người chưa thắt dây"; nhắc quên đồ khi rời xe | Có (hình dạng), không dựa vào màu |
| 6 | `face` (OMS, chỉ box) | box | Xác nhận đó là người thật (không phải túi hay gối); đầu vào cho đếm người và ước lượng chiều cao/tuổi sau này | Có |
| 7 | tag `frame_meta` → `camera_blocked`, `light`, `modality` | yes / no; day / night / unknown; rgb / ir | Biết khung nào không dùng được (camera bị che) để hệ thống báo lỗi thay vì kết luận "ghế trống" | Có |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Ghế trẻ em có hình dạng giống trẻ em ngồi bên trong thì gán thế nào? Guideline 4.8 ghi `child_seat` chỉ dùng `occupied = no/unknown` và GUIDE §3 ghi "thấy hình giống trẻ em thì báo coach, không gán", nhưng schema coach cập nhật lại cho phép `occupied = yes`. Có cần thêm box `face`/`person` cho trẻ không?
   Khung ví dụ: G05 S08_rgb / S08_ir, ghế `rear_left` (bên phải ảnh).
   Bạn tạm gán thế nào và vì sao: `child_seat`, `occupied = yes` theo schema coach cập nhật, kèm một box `face`; đã ghi để hỏi coach vì guideline v1.0 và schema mới mâu thuẫn.
2. Câu hỏi: Ảnh NIR thật (Drive&Act) có ghi `light = night` được không? Ảnh NIR không cho biết giờ trong ngày, và guideline chỉ nói rõ `light = unknown` cho SVIRO.
   Khung ví dụ: G05 D01–D06 (bạn review chéo ghi `night`, mình ghi `unknown`).
   Bạn tạm gán thế nào và vì sao: `unknown`, theo quy tắc 4.4-1 "không thấy thì unknown", vì không có dấu hiệu nào trong khung cho biết ngày hay đêm.
