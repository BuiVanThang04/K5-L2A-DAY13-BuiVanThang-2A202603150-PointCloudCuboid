# Báo cáo Day 13 — Bùi Văn Thắng, 2A202603150

## 1. Kết luận trước

Task **D13-2A202603150**, job 18, trên Windows 11, Chrome và CVAT **2.80.1**. Mười frame là s01f12, s01f41, s02f01, s02f45, s03f14, s03f51, s04f40, s04f57, s05f25 và s05f40.

Bảng A có **58 cuboid Shape**, gồm **57** cuboid trong phạm vi ngang 50 m. Sáu frame có ít nhất trong bài với 5 hộp mỗi frame là s02f01, s02f45, s03f51, s04f40, s04f57 và s05f40. Các frame này dừng ở 5 hộp sau khi chọn các vật có đủ căn cứ ở LiDAR và camera. Phiếu tự kiểm còn **5/58** hộp có cờ. Hộp xe gần và rõ điểm đáng tin hơn. Các ca cần xem lại là đáy ở sát dải cây, xe xa và hướng của người đứng. Thông tin bạn kiểm chéo và trạng thái đối chiếu ở §5.

- Xoay và dời cảnh ở Perspective, dùng góc từ trên xuống để xác định phía trước và quét hai bên.
- Chọn class và Shape, nháy đúp để đặt hộp, chỉnh tâm và yaw ở Top rồi kiểm đáy và chiều cao ở Side/Front.
- Bật Cuboid orientation để đọc vạch đỏ ngắn. Dùng preview để đối chiếu hộp với ảnh camera.

## 2. Đọc frame

| Frame | Hướng trong ảnh top | Mặt đường | Vật xa nhận ra rõ | Loại vật thấy | Phía sau hoặc hai bên | Cụm chưa xác định |
|---|---|---|---|---|---|---|
| s01f12 | Trước −Y về trái hơi chếch lên, +Y về phải, +X xuống hơi chếch trái | Phần đường gần xe khá phẳng, thấp hơn LiDAR khoảng 1,7 m | Xe buýt phía trước, khoảng 33 m | Xe con, xe buýt, người đi bộ | Có xe con phía sau và người ở lề phải | Một cụm sát dải cây bên trái phía sau bị lẫn với nền |
| s01f41 | Trước −Y về trái hơi chếch lên, +Y về phải, +X xuống hơi chếch trái | Phần đường gần xe khá phẳng, thấp hơn LiDAR khoảng 1,8 m | Xe con phía sau, khoảng 26 m | Xe con và các xe nhỏ hơn về phía xa | Xe con xanh đậm ở bên trái phía sau | Các điểm rời ngoài làn phía xa chưa đủ để tách thành vật |

Ảnh đọc frame: t1-s01f12-3d.jpg, t1-s01f12-top.jpg, t1-s01f41-3d.jpg, t1-s01f41-top.jpg.

**Camera.** (1) cam01 cho thấy xe gần ở s01f12 màu xanh đậm, xe gần ở s01f41 màu trắng. (2) Hình đèn hậu ở góc camera trước giúp nhận ra phần đuôi của xe cùng chiều. (3) cam07 làm rõ dáng người đứng cạnh mái che trong s01f12.

**LiDAR.** (1) Đo được xe con mẫu ở s01f12 cách LiDAR 6,37 m theo phương ngang. (2) Đo được cao độ mặt đường gần xe ở hai frame. (3) Góc nhìn từ trên cho biết tâm xe ở s01f41 phía sau có y dương và cho phép xem cùng lúc cả hai bên xe thu dữ liệu.

Tám frame lướt dùng góc BEV định hướng chuẩn, đầu xe ở mép trên ảnh.

| Frame | Phía trước | Điều đáng chú ý |
|---|---|---|
| s02f01 | Mép trên, −Y | Xe buýt bên trái tạo khoảng thiếu điểm sau vật chắn |
| s02f45 | Mép trên, −Y | Có xe phía sau ở khoảng 40 m chỉ còn vài chục điểm trong hộp |
| s03f14 | Mép trên, −Y | Có xe tải nhẹ và nhiều xe con theo hai phía |
| s03f51 | Mép trên, −Y | Phần dưới của xe buýt làn bên bị dải cây che |
| s04f40 | Mép trên, −Y | Có xe con ở cả phía trước lẫn phía sau |
| s04f57 | Mép trên, −Y | Cụm xe gần bên phải dày hơn cụm xe xa phía trước |
| s05f25 | Mép trên, −Y | Có một cụm điểm thấp bất thường ở phía trước bên phải |
| s05f40 | Mép trên, −Y | Xe buýt gần phía trước trái, khác làn với xe con phía trước phải |

## 3. Ba cuboid có hướng dẫn

| Vật | Frame, idx | Class | Hướng và đáy sau đối chiếu | Kích thước L × W × H |
|---|---|---|---|---|
| Xe gần | s01f12 #1 | Car | Kiểm Top, Side, Front và camera. Yaw 268,9° dọc thân xe. Đáy gần mặt đường | 4,826 × 1,970 × 1,506 m. Rộng hơn khoảng sedan/hatchback 0,070 m |
| Xe buýt | s01f12 #3 | Bus | Kiểm ba góc và camera. Yaw 281,2° cùng chiều tổng quát. Mép rộng cần rà lại | 13,727 × 3,275 × 3,184 m. Dài và rộng hơn khoảng xe buýt thường |
| Người đi bộ | s01f12 #4 | Pedestrian | Thấy người cạnh mái che ở cam07. Ba góc cho thấy có điểm nền gần mép. Hướng mặt và cao độ đáy cần rà lại | 0,745 × 0,730 × 1,785 m. Nằm trong khoảng người lớn ở bảng 4.3 |

Ảnh mẫu: t2-a-car.jpg, t2-b-bus-truck.jpg, t2-c-ped-cyclist.jpg. Các ảnh dùng **idx trong Bảng A** khi dẫn số. Số hiển thị trên thẻ của CVAT là mã giao diện.

Ở lượt rà kỹ thuật trên task, xe con mẫu có yaw dọc thân và đáy sát đường theo phiếu tự kiểm. Xe buýt mẫu còn rộng so với khoảng tham chiếu, còn hướng mặt của người đi bộ chưa chắc.

## 4. Bốn hiện tượng chất lượng

| Hiện tượng | Frame, ảnh, nguồn | Giả thuyết | Dấu hiệu thấy được | Hệ quả khi gán |
|---|---|---|---|---|
| Bóng che | s02f01, t4-1-bong-che.jpg, task cá nhân | Thân xe buýt chặn một phần tia | Sau hông xe theo hướng tia đi xa khỏi LiDAR có dải thưa điểm. Hai bên dải vẫn có điểm | Xem vùng đó là chưa quan sát được. Với vật bị che một phần, dùng Occluded khi đủ căn cứ suy ra hộp |
| Thưa ở xa | s02f45 #5, t4-2-thua-o-xa.jpg, task cá nhân | Cùng góc quét, tia cách nhau xa hơn trên vật ở xa | Hộp xe phía sau, cách 40,12 m, chứa 39 điểm. Hình xe nhỏ trên cam09 | Dùng camera cùng cụm điểm để kiểm class và hướng. Bỏ qua nếu dưới mức quan sát khoảng 15–20% và thiếu căn cứ |
| Phản xạ thấp hoặc ít điểm bất thường | s01f41 #5, t4-3-phan-xa-thap.jpg, task cá nhân | Bề mặt tối, kính hoặc góc tới có thể làm tín hiệu yếu | Xe tối màu có 185 điểm trong hộp ở 26,21 m. Một xe rõ hơn ở 22,47 m có 345 điểm | Cần xét cả khoảng cách, góc và che khuất. Không suy kích thước chỉ từ intensity |
| Điểm lạ | s05f25, t4-4-diem-la.jpg, task cá nhân | Có thể do nhiễu đo hoặc phản xạ nhiều lần | Lát cắt trước bên phải cho thấy cụm 6 điểm khoảng 30,6 m, z khoảng −14,2 m, tách xa mặt đường | Không đặt hộp lên cụm điểm. Hình bên dưới screenshot là lát cắt từ LiDAR để nhìn rõ chênh cao |

## 5. Số từ cuboid

| Frame, idx, class | z | h | z đáy | dist_m | r | yaw | Nhận xét |
|---|---:|---:|---:|---:|---:|---:|---|
| s01f12 #1 Car | −0,988 m | 1,506 m | −1,742 m | 6,37 m | 6,45 m | 268,9° | Gần mặt đường tại chỗ, gap 0,01 m |
| s01f12 #3 Bus | −0,019 m | 3,184 m | −1,611 m | 33,34 m | 33,34 m | 281,2° | Đáy gần mặt đường, gap 0,09 m |
| s01f12 #4 Pedestrian | −0,557 m | 1,785 m | −1,449 m | 7,57 m | 7,59 m | 328,2° | Gần cao độ lề quan sát được, gap 0,27 m theo self-check |

- **Khoảng cách.** Gần nhất là s04f40 #1 với 4,80 m ngang và 4,87 m theo 3D. Xa nhất trong Bảng A là s01f12 #8 với 56,39 m ngang và 56,40 m theo 3D, nằm ngoài phạm vi bài. Xa nhất trong phạm vi là s02f45 #5 với 40,12 m ngang và 40,12 m theo 3D.
- **Kích thước.** Pedestrian #4 có 0,745 × 0,730 × 1,785 m, trong khoảng người lớn. Car #1 có 4,826 × 1,970 × 1,506 m. Chiều rộng vượt khoảng sedan 0,070 m. Cần rà cụm ở hai mép vì hộp có thể đang bao cả phần nhô ra.
- **Yaw.** Xe cùng chiều s01f12 #1 có 268,9°. Xe ngược chiều s02f45 #1 có 90,0°.

| Car | dist_m | Số điểm |
|---|---:|---:|
| s01f12 #1 | 6,37 m | 2.356 |
| s04f40 #3 | 16,78 m | 680 |
| s01f41 #5 | 26,21 m | 185 |
| s02f45 #4 | 33,62 m | 91 |

Từ 6,37 m đến 33,62 m, khoảng cách tăng khoảng 5,3 lần và số điểm giảm còn khoảng 1/26. Số đo từng xe có thể lệch theo hướng nhìn và che khuất, nhưng xu hướng giảm cùng chiều với các trung vị khoảng 1.968, 718, 214 và 72 điểm của slide ở bốn khoảng tương ứng.

**Hai hộp đã sửa trong CVAT.** s01f12 #10 nâng đáy từ khoảng −2,15 m lên −1,72 m theo nền lân cận. s02f45 #5 nâng đáy từ khoảng −1,75 m lên −1,49 m. Cả hai giữ mặt trên và footprint XY. Hai CSV đã được xuất lại từ job sau khi lưu.

**Cờ còn lại.** Bus s01f12 #3 còn rộng và cần so lại mép thân. Pedestrian #4 ở sát cụm điểm của mái che và nền. Car #8 thấp và ngoài 50 m, cần quyết định bỏ khỏi task khi review. Car #9 và #10 nằm gần dải cây, nên mặt đường ước lượng dễ lẫn bề mặt cao hơn. Giữ cờ hai hộp này để xem lại Side/Front.

**Đổi kiểm với bạn học.** Bạn ngồi cạnh MSSV `2A202603137`.Cùng kiểm pointcloud/s02f12.pcd, sự khác biệt có đánh người ở dải phân cách, mình không thấy nên mâu thuẫn không gán
## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

Nhãn 3D theo mét giúp mô hình học kích thước và vị trí vật trong hệ LiDAR, kể cả khi góc camera đổi. Với vật thưa điểm, quy tắc về phần nhìn thấy giúp giảm các mục tiêu huấn luyện đặt vào vùng thiếu bằng chứng.

- **Công cụ:** Codex.
- **Nội dung:** Hỏi về quy trình CVAT, rồi chuẩn bị rà soát báo cáo để review.
- **Cách sử dụng:** Dùng hướng dẫn để thao tác trên CVAT. Codex hỗ trợ chạy các lệnh của lab.

Các điểm còn chưa chắc là chiều rộng xe buýt, hướng mặt của người đi bộ và đáy ở sát dải cây. Một hộp xe ngoài phạm vi 50 m vẫn xuất hiện trong Bảng A.

