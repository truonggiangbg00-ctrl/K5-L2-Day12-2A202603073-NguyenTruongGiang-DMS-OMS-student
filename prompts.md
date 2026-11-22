# Khai báo dùng AI — Nguyễn Trường Giang

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity (AI assistant) | Rà soát thư mục bài làm theo `SUBMISSION.md` và `RUBRIC.md`, kiểm tra kết quả `cabin_check` và `disagreement_log`, rà soát lỗi schema (`cabin_schema.py`), hỗ trợ biên tập cấu trúc và diễn đạt cho các file báo cáo (`class_list_v0.md`, `ir_log.csv`, `self_review.md`, `TEAM.md`, `exit_ticket.md`). Không cung cấp bất kỳ ảnh cabin/tài xế hay ảnh chụp màn hình nào; không nhờ AI tự gán nhãn. | Sắp xếp lại đúng tên file nộp (`export_final.zip`, `export_final_v2.zip`), chạy lại `disagreement_log.csv` đúng thứ tự `mine` vs `peer` (36 dòng), khôi phục `scripts/cabin_schema.py` bản gốc, và hoàn thiện các file văn bản/CSV dựa trên dữ liệu XML tự gán nhãn trên CVAT. | Có — kiểm lại bằng `python scripts/cabin_check.py check` và `diff`, đối chiếu từng mục trong `GUIDE.md`, `SUBMISSION.md`, `RUBRIC.md`, và tự đối chiếu lại trực tiếp các khung ảnh `G02` trên CVAT (đặc biệt `ir_log.csv` và lỗi `child_seat.occupied` ở S03–S08). |
