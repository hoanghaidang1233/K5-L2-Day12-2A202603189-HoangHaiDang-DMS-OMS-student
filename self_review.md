# Tự đánh giá — Hoàng Hải Đăng · G05

## 1. Tự rà soát 4 cảnh

Rà lại 4 ảnh (2 S##, 2 D##), dựa trên file export của mình và kết quả `cabin_check diff` với bạn review chéo.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S03_rgb / S03_ir | ghế bên trái ảnh (`rear_right`) | `wrong_class` (cần xác nhận) | Mình gán `child_seat`, bạn review gán `infant_seat`. Ghế này chìm tối trên RGB (84% pixel gần đen), nên dễ nhận nhầm loại ghế. Phải nhận theo hình dạng: nôi xách có tay cầm, ghế trẻ em có lưng cao | Chưa, chờ xem lại trên CVAT / hỏi coach |
| S08_rgb / S08_ir | `face` trong `child_seat` (`rear_left`) | `guess` | Thuộc tính mắt và `gaze_zone` của ảnh `_ir` giống hệt ảnh `_rgb`: mình copy shape sang IR mà không xét lại từng trường. Hình giống trẻ em trong ghế thì GUIDE §3 bảo báo coach | Chưa, đã đưa vào câu hỏi leo thang |
| D03 | `face13` mắt phải | `ok` | 4 điểm mắt phải bị che, mình đặt ở vị trí ước lượng và bật `occluded`; bạn review bật `outside`. Theo 4.6, `occluded` là đúng nếu đoán chắc vị trí | Không cần sửa |
| D05 | `face13` trái/phải mắt | `ok` | `cabin_check` cảnh báo trái/phải theo trục x. Kiểm theo trục y: `left_eye_*` có y = 386–422 > `right_eye_*` y = 323–348, đúng quy tắc 4.4-2 cho ảnh xoay 90° | Không cần sửa, đã giải thích trong check_final.txt |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Copy shape từ ảnh `_rgb` sang `_ir` rồi giữ nguyên thuộc tính. Ở cả 8 cặp S01–S08, thuộc tính `face` (mắt, kính, `gaze_zone`) trên ảnh IR giống hệt ảnh RGB; riêng S04_rgb và S08_rgb thì bạn review còn để sai `modality`, cho thấy bước copy là chỗ dễ sai.

Vì sao mình mắc: Muốn làm nhanh theo mẹo GUIDE §6 (Ctrl+C / Ctrl+V) nên chỉ kéo lại box mà quên mở DETAILS xét lại từng trường trên ảnh IR.

Lần sau tránh bằng cách: Sau mỗi lần dán shape sang `_ir`, mở DETAILS từng box và tự hỏi "trên ảnh này có thấy không?"; trường nào không thấy thì đổi sang `unknown` (quy tắc 4.4-1: không suy từ ảnh kia của cùng cảnh). Kiểm tag `frame_meta.modality` theo tên file trước khi chuyển ảnh.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình nhiều khả năng đúng và mình sai (trong log ghi `ambiguous`, chờ coach chốt): S01/S02, thuộc tính `face` trên ảnh OMS. Mình gán `open` / `none` / `mirror_r`, bạn để `unknown` / `other`. OMS chỉ yêu cầu box `face` (4.1), mặt người ngồi sau trong SVIRO khá nhỏ nên ghi `unknown` an toàn hơn đoán `open`.

Mình học được gì từ dòng đó: Khi một trường không bắt buộc và mình không chắc, để `unknown`/`other` tốt hơn điền một giá trị cụ thể, vì giá trị đoán sẽ dạy sai model (4.4-1, 4.5).
