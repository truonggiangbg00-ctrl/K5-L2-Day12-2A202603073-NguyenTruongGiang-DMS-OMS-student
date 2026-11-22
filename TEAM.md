# TEAM — Ngày 12

Mã bộ ảnh: **G02** · Hình thức: cá nhân (ghép cặp review chéo)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Nguyễn Trường Giang | 2A202603073 | có (đủ 22 ảnh cốt lõi S01–S08, D01–D06; thêm 4 ảnh S09–S10) | Lê Tuấn Khôi (`letuankhoi`) | 36/36 dòng trong `disagreement_log.csv` |

Ca hai bạn cần lưu ý / xác nhận thêm với coach (ghi tên ảnh + đối tượng):

- `G02 D01.png` — `face` (`eye_right`) & `face13` (`right_eye_*`): cả hai bạn hiện để `eye_right = closed` trong khi cả 4 điểm `right_eye_*` đều bật `occluded = 1` (gây `[WARN]` từ `cabin_check`), đã ghi chú giải thích trong `check_final_v2.txt` và đề nghị coach xác nhận quy ước ưu tiên `unknown` hay tắt `occluded`.
- `G02 S05_ir.png` — `child_seat` (`seat`): mine giữ đúng `rear_right` (trái ảnh = phải xe), đã nhắc peer sửa `S05_ir` từ `rear_left` sang `rear_right` cho khớp `S05_rgb`.
- `G02 S03–S08` — `child_seat` (`occupied`): hai bạn thống nhất gán `occupied = yes` và cập nhật `scripts/cabin_schema.py` cho các ghế trẻ em đang có đối tượng ngồi.

## Cá nhân

### NguyenTruongGiang-2A202603073
- Phần mình làm: Tự gán nhãn đầy đủ 22 ảnh cốt lõi (cùng 4 ảnh mở rộng S09–S10) bộ `G02` trên CVAT local, tự kiểm Checkpoint A/B/Final, đối chiếu chéo `cabin_check diff` với bạn Lê Tuấn Khôi (`letuankhoi`), phát hiện và góp ý bạn sửa lỗi đảo ghế `rear_left`/`rear_right` trên hàng ghế sau (S01–S06), đồng thời chỉnh sửa các box lỏng (`face`, `hand`, `phone`, `object` ở S01/S03_ir/S07_ir) và thuộc tính (`gaze_zone`, `eye_*`, `seatbelt`, cờ `occluded`) để xuất `export_final_v2.zip`.
- Điều mình học được hôm nay: Nắm vững quy ước gọi ghế theo xe (ngược với trái/phải ảnh OMS), quy ước trái/phải theo người trên ảnh DMS xoay 90° (`left_eye` có tọa độ `y` lớn hơn), phân biệt ảnh IR mô phỏng (SVIRO) với ảnh NIR thật (Drive&Act), và biết cách dùng `cabin_check` + review chéo để phát hiện sớm lỗi box lỏng và mâu thuẫn cờ `occluded`.
