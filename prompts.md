# Khai báo dùng AI — Nguyễn Trường Giang

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity (AI assistant) | Rà soát thư mục bài làm theo `SUBMISSION.md` và `RUBRIC.md`, kiểm tra kết quả `cabin_check` và `disagreement_log`, cập nhật đồng bộ các file báo cáo (`class_list_v0.md`, `ir_log.csv`, `self_review.md`, `TEAM.md`, `exit_ticket.md`) dựa trên các file export XML tự gán nhãn trên CVAT. Không cung cấp bất kỳ ảnh cabin/tài xế hay ảnh chụp màn hình nào; không nhờ AI tự gán nhãn. | Sắp xếp lại đúng tên file nộp (`export_final.zip`, `export_final_v2.zip`), tạo lại `disagreement_log.csv` đúng thứ tự `mine` vs `peer` (36 dòng), và hoàn thiện đồng bộ nội dung các file văn bản/CSV theo đúng dữ liệu đã sửa trên CVAT. | Có — tự kiểm lại bằng `python scripts/cabin_check.py check` (đạt 0 lỗi ở cả `check_final.txt` và `check_final_v2.txt`), đối chiếu từng mục trong `GUIDE.md`, `SUBMISSION.md`, `RUBRIC.md` và kiểm tra trực tiếp trên CVAT. |
