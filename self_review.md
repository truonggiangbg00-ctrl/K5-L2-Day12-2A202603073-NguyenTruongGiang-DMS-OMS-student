# Tự đánh giá — Nguyễn Trường Giang · G02

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S01_rgb.png` | `person` (`rear_right`), `object` (`bag`, `rear_middle`), `infant_seat` (`rear_left`) | `box`, `missing` | Đã ánh xạ đúng ghế theo xe (`rear_right` bên trái ảnh) và siết lại box `bag` sát vật hơn sau review chéo; tuy nhiên người lớn OMS đang thiếu box `face` theo Guideline §4.1–§4.2. | Một phần (đã siết box `bag`; cần bổ sung box `face` trên CVAT) |
| `S03_ir.png` | `child_seat` (`rear_middle`), `object` (`box` ở `rear_right`, `cup` ở `rear_left`) | `wrong_class`, `box` | Đã siết lại box vật hộp ở `rear_right` cho sát mép hơn; phát hiện lỗi gán `child_seat.occupied = yes` không thuộc giá trị cho phép của Guideline §4.1, §4.8 (chỉ cho phép `no` hoặc `unknown`). | Chưa sửa `occupied` trên CVAT (cần mở CVAT đổi về `no`/`unknown` và kiểm tra lại xem có vật tách riêng không) |
| `D02.png` | `face`, `phone`, `hand` | `box`, `guess` | Ban đầu box `phone` vẽ quá rộng (`122x79 px`, IoU=0.18 so với peer) và `gaze_zone` chọn `center` thay vì `road`. | Đã sửa trong `export_final_v2.zip` (siết box `phone` còn `94x20 px`, đổi `gaze_zone = road`) |
| `D03.png` | `face`, `face13`, `hand` | `flag`, `box`, `guess` | Ban đầu hai box `hand` trên vô lăng hơi lỏng (IoU 0.43–0.45) và đánh `eye_right = unknown` kèm cờ `occluded = 1` cho 4 điểm mắt phải dù vẫn thấy khe mắt mở qua kính cận, `gaze_zone` ghi `mirror_r`. | Đã sửa trong `export_final_v2.zip` (siết 2 box `hand`, đổi `eye_right = open`, `gaze_zone = road`, tắt cờ `occluded` mắt phải) |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Vẽ box hơi lỏng (dư khoảng nền xung quanh đối tượng) trên các ảnh DMS (`face`, `hand`, `phone` ở D01, D02, D03, D05), hay chọn `gaze_zone = center` thay vì `road` (D02, D05, D06), và gán giá trị `child_seat.occupied = yes` ngoài schema cho phép trên các ảnh OMS (S03–S08).

Vì sao mình mắc: Khi làm quen với ảnh NIR xoay 90°, em chưa zoom đủ sát mép vật trước khi kéo box và ước lượng hướng nhìn hơi lệch về bảng giữa xe; còn ở OMS, em tự thêm giá trị `yes` cho `child_seat.occupied` theo cảm tính và từng sửa `cabin_schema.py` thay vì bám chặt danh sách giá trị của Guideline §4.1 & §4.8.

Lần sau tránh bằng cách: Luôn zoom sát mép đối tượng trước khi vẽ/thả box (đặc biệt với vật nhỏ như `phone`, `cup`, `hand`), đối chiếu rõ trục đầu–mắt trên ảnh xoay 90° trước khi chọn `gaze_zone`, và tuyệt đối giữ nguyên `scripts/cabin_schema.py` gốc — chỉ chọn đúng giá trị hợp lệ của `labels_cabin_v1.json` (gặp ca ngoài guideline thì ghi `unknown`/`other` và hỏi coach).

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: Ở `D03.png` (`face.eye_right` và 4 cờ `face13.right_eye_*.occluded`) và `D02.png` (`phone.box`, IoU=0.18), bạn Lê Tuấn Khôi đúng còn mình sai. Mình học được rằng cần zoom kỹ vùng mắt qua tròng kính cận trên ảnh NIR trước khi vội gán `unknown`/`occluded`, phải siết box thật sát đúng phần vật thể thấy được để tránh lệch IoU lớn, và khi chạy `cabin_check.py diff` phải đặt đúng thứ tự `export_final.zip` (mine) trước `peer_export_final.zip` (peer) để không bị đảo ngược cột khi đối chiếu.
