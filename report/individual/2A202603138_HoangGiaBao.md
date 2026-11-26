# Báo cáo Day 13 — Hoàng Gia Bảo, 2A202603138

## 1. Kết luận trước

Mã số `2A202603138`, task `D13-2A202603138`. Tôi thực hiện annotation trên 10 frame được phân công: `s01f14`, `s01f43`, `s02f11`, `s02f45`, `s03f08`, `s03f31`, `s04f07`, `s04f45`, `s05f11` và `s05f45`. Môi trường thực hiện: Windows, trình duyệt Chrome, CVAT **[bổ sung phiên bản trong About]**. MSSV bạn ngồi cạnh để kiểm chéo: **[bổ sung MSSV hoặc ghi chưa thực hiện]**.

Tổng cộng tôi tạo **62 cuboid** trên 10 frame. Số lượng theo frame lần lượt là: `s01f14` 5, `s01f43` 8, `s02f11` 10, `s02f45` 3, `s03f08` 9, `s03f31` 2, `s04f07` 9, `s04f45` 4, `s05f11` 8 và `s05f45` 4. Frame có ít cuboid nhất là `s03f31` với 2 cuboid. Các frame có số lượng thấp chủ yếu cần được đối chiếu lại với point cloud để xác định liệu có cụm vật thể đủ cơ sở gán nhãn hay không; không nên tạo thêm cuboid chỉ để đạt số lượng nếu không nhận diện được vật thể chắc chắn. Kết quả `selfcheck` ghi nhận **39/62 cuboid có cảnh báo**. Đây là các trường hợp cần xem lại bằng point cloud và preview, không đồng nghĩa toàn bộ đều là lỗi chắc chắn.

Cách sử dụng CVAT: tôi chuyển đổi giữa góc nhìn 3D và Top để quan sát hình dạng, vị trí và mặt đáy của cụm điểm; xoay hoặc dịch góc nhìn để kiểm tra vật thể từ nhiều phía. Khi tạo cuboid, tôi chọn công cụ Shape/Cuboid, đặt hộp bao quanh cụm điểm rồi chỉnh kích thước và vị trí bằng các góc nhìn Top, Side và Front. Tôi bật `Cuboid orientation` để kiểm tra hướng yaw qua mũi tên và dùng ảnh `preview` để đối chiếu cuboid với ảnh camera.

## 2. Đọc frame (Bước 2)

Hai frame đọc kỹ là `s01f14` và `s01f43`. Ảnh cần đặt trong `report/individual/images/`: `t1-s01f14-3d`, `t1-s01f14-top`, `t1-s01f43-3d`, `t1-s01f43-top`.

| Frame | Phía trước xe ở cạnh nào của ảnh Top; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR khoảng bao nhiêu m) | Vật xa nhất đọc được (khoảng m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| `s01f14` | Phía trước xe nằm ở đáy ảnh Top; +x chỉ sang trái thân xe, +y chỉ về phía sau | Gần phẳng; mặt đường thấp hơn LiDAR khoảng 1,7–1,9 m (đáy các hộp chạm đường quanh −1,73 đến −1,92 m); độ lệch đáy giữa các hộp khoảng 0,2–0,5 m, có thể do dốc nhẹ | Bus, khoảng 29 m (phía trước, bị che một phần) | 4 Car, 1 Bus | Phía sau: 2 Car ở khoảng 20 m và 25 m (133 điểm). Bên trái: 1 Car sát xe (3,6 m) | [Điền cụm chưa đủ cơ sở, hoặc “không có”] |
| `s01f43` | Tương tự `s01f14` | Tương tự: thấp hơn LiDAR khoảng 1,7–1,9 m, độ lệch đáy 0,2–0,5 m | Car, khoảng 18 m (phía trước, 407 điểm) | 5 Car, 3 Cyclist | Phía sau: 1 Car ở khoảng 17 m. Bên phải: 3 Cyclist ở khoảng 9–14 m. Bên trái: 1 Car ở 6,7 m | [Điền cụm chưa đủ cơ sở, hoặc “không có”] |

Nhận xét chung: LiDAR quét hơi thưa ở xa, nhưng các vật trong phạm vi khoảng 10 m vẫn đọc rõ (Car ở 3,6 m có 3.066 điểm, ở 25,5 m chỉ còn 133 điểm).

Ba điều camera có thể cung cấp rõ hơn trong hai frame trên là: (1) đặc điểm ngoại hình để phân biệt các loại phương tiện có hình dạng point cloud gần giống nhau; (2) dấu hiệu nhận biết đầu/đuôi xe và trạng thái của vật thể; (3) vạch đường, làn đường và bối cảnh giao thông để hỗ trợ kiểm tra hướng di chuyển. Ba điều LiDAR cung cấp trực tiếp hơn là: (1) tọa độ và kích thước không gian theo mét; (2) cấu trúc, độ phân bố và mật độ điểm của từng vật thể; (3) vị trí tương đối giữa vật thể, mặt đường và các cụm điểm bị che khuất. Các nhận xét cụ thể cho hai frame cần được đối chiếu lại với ảnh camera và point cloud đã chụp.

Tám frame còn lại được lướt qua:

| Frame | Phía trước ở cạnh nào của ảnh Top | Điều lạ (nếu có) |
|---|---|---|
| `s02f11` | đáy ảnh | Bus 57,4 m, cao 5,85 m, rộng 5,29 m, vượt khoảng Bus của bảng 4.3 nên cần kiểm lại |
| `s02f45` | đáy ảnh | chỉ 3 xe, cảnh thưa |
| `s03f08` | đáy ảnh | 9 Car, cả 9 hộp cùng yaw 272,3° |
| `s03f31` | đáy ảnh | chỉ 2 cuboid; Bus 12 m ở 21,7 m có 1.751 điểm, chiếm phần lớn cảnh |
| `s04f07` | đáy ảnh | có Truck và Bus bên trái; nhiều Car phía sau có đáy cao hơn mặt đường |
| `s04f45` | đáy ảnh | toàn Car, 4 hộp đều bị cảnh báo đáy cao hơn mặt đường |
| `s05f11` | đáy ảnh | toàn Car; `#5` có 221 điểm sát ngay ngoài mép |
| `s05f45` | đáy ảnh | toàn Car; `#2` và `#4` có đáy cao hơn mặt đường trên 2 m |

## 3. Ba cuboid có hướng dẫn (Bước 3)

Ảnh minh họa đặt trong `report/individual/images/`: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front và ảnh camera? | Kích thước theo bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) Xe con gần | `s01f14`, #1 | Car | [Có/Không — yaw trong bảng A = 268,8°; xác nhận bằng mũi tên trong CVAT] | Không hoàn toàn: đáy z = −2,094 m, chìm khoảng 0,33 m (theo selfcheck) | [Có/Chưa] | Trong khoảng: 4,886 × 2,105 × 1,994 m |
| (b) Xe buýt hoặc xe tải | `s03f31`, #2 | Bus | [Có/Không — yaw trong bảng A = 268,8°; xác nhận bằng mũi tên trong CVAT] | Có: lệch 0,00 m (z đáy = −1,696 m) | [Có/Chưa] | Rộng 3,309 m vượt khoảng 2,0–3,2 m (+3,4%); dài 11,999 m và cao 3,128 m nằm trong khoảng |
| (c) Người đi bộ hoặc xe hai bánh | `s01f43`, #7 | Cyclist | [Có/Không — yaw trong bảng A = 274,9°; xác nhận bằng mũi tên trong CVAT] | Gần như có: đáy cao hơn mặt đường khoảng 0,26 m (z đáy = −1,228 m) | [Có/Chưa] | Không bị selfcheck cảnh báo: 1,956 × 0,562 × 1,757 m |

Vòng hiệu chuẩn: [Bổ sung điểm cuboid của tôi khác với người kiểm chéo ở đâu và cách hiểu guideline mà cả lớp thống nhất. Nếu chưa có vòng hiệu chuẩn, ghi rõ chưa thực hiện thay vì suy đoán.]

Các vật bỏ qua vì không đủ cơ sở: [Liệt kê frame, vị trí tương đối, mô tả cụm điểm và loại nghi vấn như `UNCERTAIN_CLASS` hoặc `LOW_VISIBILITY`; nếu không có, ghi “không có”.]

## 4. Bốn hiện tượng chất lượng (Bước 4)

Các nguyên nhân dưới đây cần được trình bày dưới dạng giả thuyết, không khẳng định khi chưa có bằng chứng kiểm chứng. Với frame mượn, cần ghi rõ nguồn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Biểu hiện trên point cloud | Hệ quả khi gán nhãn theo guideline |
|---|---|---|---|---|
| 1. Bóng che | [Frame, ảnh `t4-1-bong-che`, nguồn] | [Ví dụ: tia LiDAR bị vật thể phía trước che khuất] | [Mô tả phần điểm còn thấy và phần bị thiếu] | [Nêu cách xử lý occlusion theo guideline] |
| 2. Thưa ở xa | [Frame, ảnh `t4-2-thua-o-xa`, nguồn] | [Ví dụ: mật độ điểm giảm khi khoảng cách tăng] | [Mô tả cụm điểm thưa/vỡ mảng] | [Nêu giới hạn nhận diện và cách dùng kích thước tham chiếu nếu guideline cho phép] |
| 3. Phản xạ thấp / ít điểm bất thường | [Frame, ảnh `t4-3-phan-xa-thap`, nguồn] | [Giả thuyết về đặc tính bề mặt hoặc góc phản xạ] | [Mô tả mặt/cạnh của vật thể bị thiếu điểm] | [Nêu phần nào có thể gán chắc chắn, phần nào không nên suy đoán] |
| 4. Điểm lạ | [Frame, ảnh `t4-4-diem-la`, nguồn] | [Giả thuyết: phản xạ đa đường, nhiễu hoặc điểm thuộc vật thể khác] | [Mô tả vị trí và hình dạng các điểm bất thường] | [Nêu cách không để điểm lạ làm sai đáy/kích thước cuboid] |

## 5. Số từ cuboid của tôi (Bước 5)

Nguồn số liệu: `submission/D13-2A202603138-bangA.csv`; số điểm lấy từ cột `points_in` trong `submission/D13-2A202603138-tukiem.csv`. Mặt đường tham chiếu khoảng −1,7 m; đáy hộp được tính bằng `z_bottom = z − h/2`. Nếu đáy lệch mặt đường trên 0,3 m thì cần kiểm tra khả năng cuboid bị lơ lửng hoặc chìm, đồng thời xem xét độ dốc và địa hình tại vị trí đó.

| Cuboid (frame, idx, class) | z (m) | h (m) | z đáy (m) | dist_m (m) | r (m) | yaw_deg (°) | Nhận xét |
|---|---:|---:|---:|---:|---:|---:|---|
| `s03f08`, #1, Car | −0,994 | 1,739 | −1,864 | 4,13 | 4,25 | 272,3 | Chạm đường: đáy lệch mặt đường −1,7 m khoảng 0,16 m (selfcheck: −0,10 m), trong ngưỡng 0,3 m. |
| `s01f14`, #1, Car | −1,097 | 1,994 | −2,094 | 3,61 | 3,77 | 268,8 | Chìm: đáy thấp hơn khoảng 0,33 m so với mặt đường theo selfcheck. Cần xem lại độ dốc/địa hình hoặc hạ chiều cao hộp. |
| `s04f07`, #3, Car | 0,634 | 1,739 | −0,236 | 16,12 | 16,13 | 272,3 | Lơ lửng: đáy cao hơn mặt đường khoảng 1,48 m, chỉ 95 điểm. Cần xem lại vị trí z. |

(a) Cuboid gần nhất: **`s01f14`, #1, Car**, `dist_m` = **3,61 m**, `r` = **3,77 m**. Cuboid xa nhất: **`s02f11`, #9, Bus**, `dist_m` = **57,38 m**, `r` = **57,38 m**. `dist_m` là khoảng cách ngang theo `√(x²+y²)`, còn `r` là khoảng cách 3D đến tâm hộp theo `√(x²+y²+z²)`.

(b) Kích thước xe hai bánh: **`s01f43`, #7, Cyclist; 1,956 × 0,562 × 1,757 m**. Selfcheck không cảnh báo cuboid này, nên ba chiều đều nằm trong khoảng của bảng 4.3 (cần đối chiếu lại số liệu bảng 4.3 để ghi khoảng cụ thể). Kích thước xe con: **`s03f08`, #1; 4,852 × 2,219 × 1,739 m**. Cả ba chiều nằm trong khoảng Car của bảng 4.3 (dài 3,0–6,0 m; rộng 1,4–2,3 m; cao 1,2–2,3 m); chiều rộng 2,219 m sát giới hạn trên 2,3 m. Ví dụ ngoài khoảng: `s01f43` #1 Car dài 6,227 m (vượt 6,0 m khoảng 3,8%) và rộng 3,247 m (vượt 2,3 m khoảng 41%). Hộp này có 2.772 điểm bên trong nên khả năng cao là hộp ôm cả vật bên cạnh; cần kiểm tra lại, chưa kết luận giữ hay sửa.

(c) Xe cùng chiều phía trước: **`s01f14`, #2**, `yaw_deg` = **269,7°**. Xe ngược chiều: **`s04f07`, #8**, `yaw_deg` = **84,5°**. Đối chiếu quy ước yaw của guideline: giá trị gần 270° và gần 90° khớp với hai trường hợp này. Lưu ý trong `s02f11` các xe `#1`, `#5`, `#6`, `#7` có yaw 91,2° trong khi `#3` có 262,3°; cần xác nhận hướng thực tế từ ảnh camera.

Mật độ điểm theo khoảng cách (bốn cuboid Car):

| Cuboid (frame, idx, class) | dist_m (m) | Số điểm (`points_in`) |
|---|---:|---:|
| `s01f14`, #1, Car | 3,61 | 3.066 |
| `s03f08`, #4, Car | 12,17 | 1.354 |
| `s01f14`, #3, Car | 20,15 | 407 |
| `s03f08`, #8, Car | 25,95 | 325 |

Quy luật quan sát: khoảng cách tăng khoảng 7,2 lần (từ 3,61 m lên 25,95 m) thì số điểm giảm còn khoảng 1/9,4 (3.066 → 325). So với đường cong tham chiếu trong bài giảng cho xe con (1.968 điểm ở 0–10 m; 718 ở 10–20 m; 214 ở 20–30 m; 72 ở 30–40 m; 19 ở 60–80 m), dữ liệu của tôi **cao hơn** ở các khoảng tương ứng (3.066 so với 1.968 ở 0–10 m; 1.354 so với 718 ở 10–20 m; 407 và 325 so với 214 ở 20–30 m). Chênh lệch có thể đến từ kích thước hộp, loại xe, góc quan sát, mức độ che khuất và cách phân bố điểm; cần dùng các cuboid cùng class để so sánh công bằng. Hộp lớn hơn thực tế hoặc ôm thêm điểm của vật bên cạnh cũng làm số điểm tăng.

Phần tự kiểm: hai cuboid đã sửa sau `preview` / `selfcheck`: (1) **[frame, idx]** — [mô tả thay đổi và lý do]; (2) **[frame, idx]** — [mô tả thay đổi và lý do]. Những cảnh báo giữ nguyên: **[frame, idx, cảnh báo]** — [lý do dựa trên quan sát point cloud; không giữ chỉ vì muốn tránh sửa].

Đổi kiểm: MSSV người kiểm chéo với tôi là **[MSSV / chưa thực hiện]**. Hai số liệu đối chiếu với bảng A: **[số liệu thứ nhất]** và **[số liệu thứ hai]**. Kết quả: **[khớp/lệch; nếu lệch, nêu dữ liệu đã sửa và nguồn xác nhận]**.

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

(a) Nhìn từ phía mô hình học máy, nhãn 3D cần được đo bằng **mét** vì mô hình phải học vị trí, kích thước và khoảng cách thực của vật thể trong không gian, phục vụ các tác vụ như nhận biết chướng ngại vật và lập kế hoạch chuyển động; pixel chỉ biểu diễn vị trí trên mặt phẳng ảnh và không trực tiếp cho biết khoảng cách thật. Vật thể ở xa hoặc bị che thường có ít điểm LiDAR hơn, vì vậy cần quy tắc riêng về độ tin cậy, che khuất và kích thước tham chiếu để tránh biến phần suy đoán thành ground truth.

(b) Hỗ trợ AI trong quá trình làm báo cáo: **ChatGPT** được sử dụng để sắp xếp cấu trúc báo cáo, diễn đạt các phần giải thích và rà soát tính nhất quán của nội dung. Các số liệu annotation phải được lấy từ bảng A và phiếu tự kiểm; các nhận xét về hình dạng, hướng và chất lượng cuboid phải được đối chiếu với CVAT, point cloud và ảnh preview. Không dùng nội dung do AI đề xuất thay cho việc xác minh nhãn trên dữ liệu gốc.

(c) Điều còn chưa chắc: **[Nêu cụ thể các cuboid hoặc hiện tượng còn nghi vấn, ví dụ đáy hộp ở khu vực mặt đường dốc, hướng yaw của vật ít điểm hoặc phân biệt class khi bị che khuất.]** Những điểm phát hiện sau thời hạn nộp cần được ghi nhận bằng lời và phân biệt với các lỗi đã được xác nhận trong phiếu tự kiểm.
