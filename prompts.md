# Khai báo dùng AI — Hoàng Hải Đăng

Không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho công cụ AI. AI chỉ đọc file markdown, file export XML (toạ độ + thuộc tính) và output của `cabin_check`.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Claude Code (Claude Opus) | Đọc CHECKPOINTS.md / SUBMISSION.md, liệt kê việc đã làm và còn thiếu | Làm danh sách việc cần làm tiếp | Có, đối chiếu lại với SUBMISSION.md |
| 2 | Claude Code | Cách chạy Checkpoint A, B | AI chạy `cabin_check` với `--only S01,S02` và `--only D01,D02` trên export_final.zip, ra check_cpA.txt, check_cpB.txt | Có, đọc output: 0 lỗi, đúng số ảnh |
| 3 | Claude Code | Lệnh git để push bài lên GitHub | Chạy theo các lệnh AI đưa | Có, kiểm `git status`, .gitignore chặn ảnh |
