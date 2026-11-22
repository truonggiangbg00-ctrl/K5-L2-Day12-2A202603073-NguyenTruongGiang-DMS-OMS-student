# Tự đánh giá — Nguyễn Trường Giang · G02

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S03_ir.png` | `child_seat` (`rear_middle`), `object` (`box` ở `rear_right`, `cup` ở `rear_left`) | `box` | Ban đầu box `cup` ở `rear_left` và `box` ở `rear_right` vẽ hơi rộng so với mép vật (IoU=0.46 với peer); ghế đã ánh xạ đúng theo xe. | Đã sửa trong `export_final_v2.zip` (siết gọn box `cup` và `box` trên `S03_ir.png`) |
| `S07_ir.png` | `person` (`rear_middle`), `object` (`box`, `rear_right`) | `box`, `guess` | Ban đầu ghi `seatbelt = unknown` và vẽ box `box` hơi rộng (`167..411`); sau khi kiểm lại trên ảnh và đối chiếu peer đã xác nhận `seatbelt = not_worn` và siết box `box` còn `215..380`. | Đã sửa trong `export_final_v2.zip` |
| `D01.png` | `face`, `face13`, `hand` | `box`, `flag` | Ban đầu box `face` và `hand` phải hơi rộng (IoU 0.43–0.50); sau khi siết box và đổi `eye_right = closed` theo peer nhưng 4 điểm `right_eye_*` vẫn giữ `occluded = 1` nên phát sinh `[WARN]` ở `cabin_check`. | Đã siết box trong `export_final_v2.zip` và ghi chú giải thích `[WARN]` trong `check_final_v2.txt` |
| `D04.png` | `face`, `face13`, `hand` | `guess` | Ban đầu đánh giá nhầm hai mắt qua kính cận là `closed` và `gaze_zone = other`; sau review chéo đã soát lại khe mắt, sửa thành `open`, `gaze_zone = road` và giữ `occluded = 1` cho `mouth_right`, `lip_lower` bị che. | Đã sửa trong `export_final_v2.zip` |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Vẽ box hơi lỏng (dư khoảng nền xung quanh đối tượng) trên cả ảnh DMS (`face`, `hand`, `phone` ở D01, D02, D03, D05) và OMS (`cup` ở S03, `box` ở S07), kèm việc chọn `gaze_zone = center` thay vì `road` ở nhiều khung hình (D02, D05, D06).

Vì sao mình mắc: Khi mới thao tác trên ảnh NIR xoay 90° và ảnh IR-sim đơn sắc mất màu, em chưa zoom đủ sát mép đối tượng trước khi kéo/thả box và ước lượng hướng mắt hơi lệch về màn hình giữa xe thay vì nhìn thẳng qua kính lái.

Lần sau tránh bằng cách: Luôn zoom sát mép đối tượng trước khi vẽ/thả box (đặc biệt với vật nhỏ như `phone`, `cup`, `hand`), đối chiếu rõ trục đầu–mắt chuẩn trên ảnh xoay 90° trước khi chọn `gaze_zone`, và kiểm tra tính nhất quán giữa `eye_*` với cờ `occluded` của 4 điểm `face13` trước khi export.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai. Mình học được gì từ dòng đó: Ở `D03.png` (`face.eye_right` và 4 cờ `face13.right_eye_*.occluded`), `D04.png` (`eye_left`/`eye_right`/`gaze_zone`) và `D02.png` (`phone.box`, IoU=0.18), bạn Lê Tuấn Khôi đúng còn mình sai. Mình học được rằng cần zoom kỹ vùng mắt qua tròng kính cận trên ảnh NIR để phân biệt rõ mắt mở hẹp với mắt nhắm/bị che, phải siết box thật sát đúng phần vật thể thấy được để tăng độ nhất quán IoU, và sau khi sửa theo peer luôn phải chạy lại `cabin_check` để rà các cảnh báo nhất quán (như `[WARN]` cờ `occluded` ở `D01.png`).
