# Danh sách lớp v0 — Nguyễn Trường Giang · G02

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `eye_state` (`eye_left`, `eye_right`) | `open` / `closed` / `unknown` | Phát hiện buồn ngủ, ngủ gật (tính tỷ lệ nhắm mắt PERCLOS) và cảnh báo tài xế mất tỉnh táo | Có (nhìn rõ cả ban đêm nhờ LED NIR 940 nm, trừ khi kính chói phản xạ hoặc bị che khuất) |
| 2 | `gaze_zone` | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Phát hiện tài xế rời mắt khỏi đường quá lâu (eyes-off-road / distraction) để phát cảnh báo | Có (dựa vào hướng đầu và đồng tử trên NIR, giảm độ tin cậy khi quay hẳn đầu hoặc đeo kính râm tối) |
| 3 | `face13` | 13 điểm mốc mặt kèm cờ `occluded` / `outside` | Ước lượng tư thế đầu (yaw/pitch/roll), độ mở mắt (EAR) và độ mở miệng/ngáp (MAR) để đánh giá mệt mỏi và hướng chú ý | Có (nhận diện tốt trên NIR, cần đánh dấu `occluded`/`outside` ở điểm bị tay, vật cầm hoặc đốm phản xạ che) |
| 4 | `glasses` | `none` / `clear` / `sun` / `unknown` | Nhận biết tài xế đeo kính cận hay kính râm để xử lý phản xạ tròng kính hoặc điều chỉnh ngưỡng tin cậy của nhánh theo dõi mắt | Một phần (thấy gọng kính trên NIR, nhưng kính râm có thể trông trong suốt nên dễ nhầm `clear` với `sun`) |
| 5 | `phone` | `held` / `mounted` / `lap` | Phát hiện hành vi cầm hoặc sử dụng điện thoại gây mất tập trung khi đang lái xe | Có (nhận diện qua cạnh, góc và hình dạng máy; khó hơn RGB vì mất màu, dễ nhầm vật tối khác nếu không rõ cạnh) |
| 6 | `hand` | `whose` (`driver`/`passenger`), `side` (`left`/`right`), `on_wheel` (`yes`/`no`/`unknown`) | Kiểm tra trạng thái đặt tay trên vô lăng (hands-on/hands-off wheel) phục vụ ADAS/chuyển giao quyền lái và nhận diện thao tác phụ | Có (thấy rõ hình dạng bàn tay và vị trí tương đối so với vô lăng trên NIR) |
| 7 | `person.seatbelt` (tài xế) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Cảnh báo tài xế chưa thắt dây an toàn hoặc thắt sai quy cách (lệch vai, luồn dưới nách) | Có (thấy dải dây chéo qua vai/ngực nếu đủ tương phản, nhưng dễ bị che bởi tay hoặc áo tối → `unknown`) |
| 8 | `frame_meta.camera_blocked` | `yes` / `no` | Phát hiện camera DMS bị che khuất, bám bẩn hoặc bị cố tình bịt để báo lỗi hệ thống (system unavailable) thay vì bỏ sót cảnh báo | Có (nhận biết trực tiếp khi khung hình NIR bị tối đen, mờ đục hoặc có vật cản sát ống kính) |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` | theo `seat` (`driver`, `front_passenger`, `rear_left`, `rear_middle`, `rear_right`) | Xác định số lượng và vị trí hành khách ngồi trên từng ghế để điều khiển túi khí, cảnh báo dây an toàn và tối ưu điều hòa | Có (nhận diện theo hình dạng cơ thể; vùng tối cần tăng tương phản hiển thị để tránh bỏ sót người) |
| 2 | `person.seatbelt` (hành khách) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Kiểm tra việc thắt dây an toàn của hành khách hàng ghế sau và phát hiện thắt sai cách | Một phần (mất màu trên IR/IR-sim nên dây tối trên áo tối rất khó thấy → thường phải ghi `unknown`) |
| 3 | `child_seat` | theo `seat`, `occupied` (`no` / `unknown`) | Nhận diện ghế trẻ em lắp trên hàng ghế sau để quản lý an toàn hành khách nhỏ tuổi và phân biệt với ghế trống/người lớn | Có (nhận diện theo hình dạng khung ghế; vùng bên trong ghế tối dễ khó xác định rõ nên dùng `unknown`) |
| 4 | `infant_seat` | theo `seat`, `facing` (`rear` / `front`) | Phát hiện nôi em bé và hướng lắp để nhắc quên trẻ trong xe (CPD) và tắt túi khí khi lắp quay sau ở ghế trước | Có (thấy rõ hình dạng nôi xách và hướng lắp trên IR) |
| 5 | `object` | theo `seat`, `type` (`bag` / `box` / `cup` / `other`) | Phân biệt đồ vật đặt trên ghế với người ngồi để tránh cảnh báo nhầm dây an toàn (false SBR alert) và hỗ trợ nhắc quên đồ | Có (dựa vào hình dạng và vị trí; khó phân loại chi tiết vật nhỏ tối màu do không có thông tin màu sắc) |
| 6 | `face` (OMS) | box mặt hành khách | Xác định vị trí đầu hành khách để phát hiện tư thế ngồi bất thường (out-of-position) phục vụ điều chỉnh bung túi khí | Có (thấy được vùng đầu/mặt nếu hành khách không quay hẳn đi hoặc bị che khuất) |
| 7 | `frame_meta` | `modality` (`rgb`/`ir`), `light` (`day`/`night`/`unknown`), `camera_blocked` (`yes`/`no`) | Phân tách dữ liệu RGB và IR cho pipeline huấn luyện/đánh giá và phát hiện camera cabin bị che khuất | Có (`modality` và `camera_blocked` xác định trực tiếp; `light` là `unknown` trên ảnh tổng hợp SVIRO) |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. Câu hỏi: Guideline Cabin v1.0 (§4.1, §4.8) quy định `child_seat.occupied` chỉ có hai giá trị `no` và `unknown` (không cho phép `yes`) và lưu ý bộ ảnh SVIRO không có trẻ em ngồi, nhưng trong nhiều khung hình thực tế ghế trẻ em trông không hoàn toàn trống (hoặc có vật/búp bê nằm trong lòng ghế). Guideline có nên bổ sung tiêu chí phân biệt rõ khi nào tách thêm box `object` độc lập trong ghế trẻ em và khi nào bắt buộc ghi `occupied = unknown` (hoặc báo coach bỏ qua), thay vì để annotator dễ hiểu nhầm rằng cần giá trị `yes`?
   Khung ví dụ: `G02 S03_rgb / S03_ir` và `G02 S04_rgb / S04_ir` (ghế `rear_middle`).
   Bạn tạm gán thế nào và vì sao: Ban đầu trong lúc gán trên CVAT em và bạn review chéo cùng gặp lúng túng nên thử dùng `occupied = yes` cho `S03–S08`. Sau khi đối chiếu lại Guideline §3, §4.1, §4.8 và schema gốc `labels_cabin_v1.json`, giá trị `yes` là không hợp lệ nên cần chuyển về đúng chuẩn (`no` khi trống + tách box `object` nếu là đồ vật, hoặc `unknown` nếu không nhìn rõ / nghi giống trẻ em và báo coach).

2. Câu hỏi: Trên ảnh NIR thật xoay 90° có đeo kính cận (`glasses = clear`), khi tài xế cúi mắt hoặc nheo mắt rất hẹp, ranh giới giữa `eye_* = closed`, `eye_* = open` (liếc xuống nhưng vẫn còn khe mắt hẹp) và `eye_* = unknown` (không đủ độ tương phản qua tròng kính để kết luận) rất dễ gây bất đồng giữa các annotator. Người viết guideline có thể bổ sung tiêu chí định lượng (ví dụ ngưỡng thấy rõ lòng trắng/khe mí tối thiểu hoặc quy tắc mặc định về `unknown` khi có bóng kính) không?
   Khung ví dụ: `G02 D04.png` (so sánh thêm với `G02 D01.png` ở mắt phải bị che).
   Bạn tạm gán thế nào và vì sao: Trên `G02 D04.png`, em tạm gán `eye_left = closed`, `eye_right = closed`, `gaze_zone = other` (trong khi bạn review chéo gán `open` và `road`) và ghi nhận `ambiguous` trong `disagreement_log.csv` để hỏi coach phân xử. Riêng `G02 D01.png`, em giữ `eye_right = unknown` kèm bật `occluded = 1` cho cả 4 điểm `right_eye_*` đúng quy tắc 4.4-1 và 4.6 vì không thấy rõ mắt phải.
