# TEAM — Ngày 12

Mã bộ ảnh: **G02** · Hình thức: cá nhân (ghép cặp review chéo)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Nguyễn Trường Giang | 2A202603073 | có (đủ 22 ảnh cốt lõi S01–S08, D01–D06; thêm 4 ảnh S09–S10) | Lê Tuấn Khôi (`letuankhoi`) | 36/36 dòng trong `disagreement_log.csv` |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- `G02 D04.png` — `face` (`eye_left`, `eye_right`, `gaze_zone`): mine gán `closed` / `closed` / `other`, peer gán `open` / `open` / `road`.
- `G02 D01.png` — `face` (`eye_right`): sau khi siết lại box `face` (IoU=0.79), mine giữ `eye_right = unknown` (khớp với 4 điểm `right_eye_*` đều `occluded = 1`), còn peer gán `closed` (gây cảnh báo `cabin_check`).
- `G02 S03–S08` — `child_seat` (`occupied`): cả hai bạn từng chọn `yes`; cần xác nhận lại với coach cách xử lý theo Guideline §3 và §4.8 vì schema chỉ cho phép `no` hoặc `unknown`.

## Cá nhân

### NguyenTruongGiang-2A202603073
- Phần mình làm: Tự gán nhãn đầy đủ 22 ảnh cốt lõi (cùng 4 ảnh mở rộng S09–S10) bộ `G02` trên CVAT local, tự kiểm Checkpoint A/B/Final, đối chiếu chéo `cabin_check diff` với bạn Lê Tuấn Khôi (`letuankhoi`), phát hiện và góp ý bạn sửa lỗi đảo ghế `rear_left`/`rear_right` trên hàng ghế sau (S01–S06), đồng thời chỉnh sửa các box lỏng (`face`, `hand`, `phone`) và thuộc tính (`gaze_zone`, `eye_right`, cờ `occluded`) để xuất `export_final_v2.zip`.
- Điều mình học được hôm nay: Nắm vững quy ước gọi ghế theo xe (ngược với trái/phải ảnh OMS), quy ước trái/phải theo người trên ảnh DMS xoay 90° (`left_eye` có tọa độ `y` lớn hơn), phân biệt ảnh IR mô phỏng (SVIRO) với ảnh NIR thật (Drive&Act), và tầm quan trọng của việc tuân thủ đúng schema guideline thay vì đoán hoặc tự sửa script kiểm tra.
