# Khai báo dùng AI — Hoàng Hải Đăng

Không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho công cụ AI. AI chỉ đọc file markdown, file export XML (toạ độ + thuộc tính) và output của `cabin_check`.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (Claude Opus) | Đọc CHECKPOINTS.md / SUBMISSION.md, liệt kê việc đã làm và còn thiếu | Làm danh sách việc cần làm tiếp | Có, đối chiếu lại với SUBMISSION.md |
| 2 | Claude Code | Cách chạy Checkpoint A, B | AI chạy `cabin_check` với `--only S01,S02` và `--only D01,D02` trên export_final.zip, ra check_cpA.txt, check_cpB.txt | Có, đọc output: 0 lỗi, đúng số ảnh |
| 3 | Claude Code | Xử lý disagreement_log.csv | AI đề xuất verdict/guideline_rule/action cho 29 dòng quyết được chỉ bằng guideline và dữ liệu XML (modality, object trên D##, ghế S03, face S05–S06, light). Các dòng cần nhìn ảnh để trống, mình tự xem trên CVAT | Có, đọc lại từng dòng với mục GUIDE ghi ở cột guideline_rule |
| 4 | Claude Code | Lệnh git để push bài lên GitHub | Chạy theo các lệnh AI đưa | Có, kiểm `git status`, .gitignore chặn ảnh |
| 5 | Claude Code | Soạn nháp class_list_v0.md và khung TEAM.md | Dùng làm bản nháp, mình đọc lại và sửa câu chữ | Có, đối chiếu với bảng 4.1 trong GUIDE |
| 6 | Claude Code | Soạn nháp ir_log.csv và self_review.md | AI không xem ảnh: dựa vào nhãn trong export của mình (XML), kết quả diff với bạn và số liệu độ sáng từ `scripts/rgb_ir_diff.py` (chạy trên máy, AI chỉ đọc file CSV số liệu) | Cần đối chiếu từng dòng với ảnh trên CVAT trước khi nộp |
