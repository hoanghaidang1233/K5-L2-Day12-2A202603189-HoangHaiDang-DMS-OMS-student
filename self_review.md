# Tự đánh giá — Hoàng Hải Đăng · G05

## 1. Tự rà soát 4 cảnh

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S03 | ghế `rear_right` (trái ảnh) | `wrong_class` (cần xác nhận) | Mình `child_seat`, bạn `infant_seat`. Ghế chìm tối trên RGB, phải nhận theo hình dạng | Chưa, hỏi coach |
| S08 | `face` trong `child_seat` | `guess` | Thuộc tính ảnh `_ir` copy y hệt `_rgb`, chưa xét lại | Chưa, đã đưa vào câu hỏi leo thang |
| D03 | `face13` mắt phải | `ok` | Bị che, đoán chắc vị trí nên `occluded` (4.6) | Không cần |
| D05 | `face13` trái/phải | `ok` | Ảnh xoay 90°: `left_eye_*` có y lớn hơn (4.4-2) | Không cần, đã giải thích trong check_final.txt |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Copy shape từ `_rgb` sang `_ir` rồi giữ nguyên thuộc tính (cả 8 cặp S01–S08).

Vì sao mình mắc: Làm nhanh bằng Ctrl+C / Ctrl+V, quên mở DETAILS xét lại trên ảnh IR.

Lần sau tránh bằng cách: Dán xong thì mở DETAILS từng box, trường nào không thấy trên IR thì đổi `unknown` (4.4-1).

## 3. Ghi chú về disagreement_log

S01/S02, thuộc tính `face` OMS: mình gán `open`/`none`, bạn để `unknown`. Học được: trường không bắt buộc mà không chắc thì để `unknown`, đừng đoán (4.4-1).
