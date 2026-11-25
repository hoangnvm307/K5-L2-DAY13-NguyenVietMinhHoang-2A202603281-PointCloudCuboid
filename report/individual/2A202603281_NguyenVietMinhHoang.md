# Báo cáo Day 13 - Nguyễn Việt Minh Hoàng, 2A202603281

## 1. Kết luận trước

Bài cá nhân `D13-2A202603281`. Bảng A hiện có **54 cuboid**, ít nhất 5 hộp/frame; 5 hộp được cắm cờ tự kiểm. Số lượng theo lớp: {'Cyclist': 5, 'Car': 48, 'Truck': 1}.

| Frame | Số hộp |
|---|---:|
| s01f08 | 9 |
| s01f58 | 5 |
| s02f34 | 5 |
| s02f52 | 5 |
| s03f00 | 5 |
| s03f43 | 5 |
| s04f14 | 5 |
| s04f49 | 5 |
| s05f09 | 5 |
| s05f28 | 5 |

## 2. Đọc frame (Bước 2)

Hai frame đọc kỹ là **s01f08** và **s01f58**. Ảnh đối chiếu: `t1-s01f08-3d.jpg`, `t1-s01f08-top.jpg`, `t1-s01f58-3d.jpg`, `t1-s01f58-top.jpg`.

Các hướng dưới đây theo ảnh Top đã đặt **-y hướng lên**: phía trước ở cạnh trên, **+x sang trái**, **+y xuống dưới**. Khoảng cách là khoảng cách ngang theo LiDAR, được làm tròn. Mốc xa ghi trong bảng là vật đã đối chiếu được, không phải giới hạn tầm đo của cảm biến hoặc khẳng định không còn vật nào xa hơn.

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f08 | Cạnh trên là phía trước (-y); +x sang trái, +y xuống dưới | Gần phẳng cục bộ ở làn xe thu; mặt đường thấp hơn LiDAR khoảng 1,7–1,8 m. Phần đường đối diện thấp hơn, không dùng chung một mức z cho mọi hộp | Đã đối chiếu xe phía sau khoảng 35 m; chưa xác định chắc vật xa nhất của toàn cảnh | Sedan, van chở người, người đi xe hai bánh; camera còn thấy xe buýt phía trước, cây và biển báo | Sedan xanh sát bên trái khoảng 3,9 m; người đi xe hai bánh bên phải khoảng 10 m; ô tô phía sau khoảng 20–35 m | Cụm ở phía dưới người/xe hai bánh sau hàng cây; chưa tách chắc điểm của vật với điểm cây, phần bánh bị che |
| s01f58 | Cạnh trên là phía trước (-y); +x sang trái, +y xuống dưới | Đường gần xe tương đối phẳng, thấp hơn LiDAR khoảng 1,8 m; có vỉa hè và bồn cây cao hơn mặt đường | Đối chiếu được phần xe buýt phía trước khoảng 32 m; chưa đo chắc vật xa nhất toàn cảnh | Nhiều sedan, xe buýt ở xa, xe hai bánh gần vỉa hè, cây và biển chỉ dẫn làn | Sedan trắng sát trái khoảng 3 m; sedan trắng phía sau khoảng 7 m, thấy rõ qua cam09 | Các cụm nhỏ cạnh cây và xe hai bánh bên phải; chưa chắc cụm nào là người, xe đỗ hoặc cây khi chỉ nhìn point cloud |

**Ba điều camera cho biết rõ mà point cloud đang xem không thể hiện được:**

1. **Màu sắc xe:** sedan sát trái ở s01f08 màu xanh; các sedan gần xe thu trong s01f58 màu trắng. Màu hiển thị của điểm LiDAR không phải màu sơn xe.
2. **Chi tiết trang phục:** ở s01f08, người đi xe hai bánh bên phải mặc áo xanh và có ba lô đỏ; các chi tiết này giúp tìm đúng người giữa hàng cây trong cam07/cam08.
3. **Nội dung và màu của biển chỉ dẫn:** ở s01f58, camera cho thấy biển nền xanh có mũi tên phân làn; cụm điểm trên biển không cho đọc các ký hiệu in trên mặt biển.

**Ba điều LiDAR cho biết trực tiếp mà ảnh camera đơn lẻ không đo được chính xác:**

1. **Vị trí và khoảng cách theo mét:** sedan xanh ở s01f08 có tâm khoảng x = 3,49 m, y = -1,70 m, cách ngang khoảng 3,88 m; sedan trắng sát trái ở s01f58 cách khoảng 2,95 m.
2. **Độ cao tương đối của mặt đường:** các điểm mặt đường gần xe ở hai frame nằm thấp hơn gốc LiDAR khoảng 1,7–1,8 m; có thể xem mặt cắt để phân biệt mặt đường với vỉa hè và hàng cây.
3. **Mật độ và phân bố phản hồi laser trong không gian:** thân xe gần có nhiều điểm hơn xe phía sau khoảng 35 m ở s01f08; vùng dưới người/xe hai bánh bị cây che thiếu điểm, nên không được coi phần điểm nhìn thấy là toàn bộ vật.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f34 | Cạnh trên (-y), theo ảnh Top cùng quy ước | Xe ở đường đối diện bị dải phân cách che một phần; cần xem camera để tách xe khỏi hàng cây |
| s02f52 | Cạnh trên (-y) | Điểm cây gần xe đối diện dễ lẫn vào thân xe, làm hộp quá cao hoặc quá rộng |
| s03f00 | Cạnh trên (-y) | Nhiều xe gần ở trước, sau và bên trái; phải tách từng xe, tránh gộp hai cụm thành một vật |
| s03f43 | Cạnh trên (-y) | Có người đi xe hai bánh phía sau-phải; xe phía sau khoảng 37 m thưa điểm hơn xe gần |
| s04f14 | Cạnh trên (-y) | Có xe hai bánh phía sau khoảng 28 m; nếu chỉ xem cam01 sẽ bỏ sót vật này |
| s04f49 | Cạnh trên (-y) | Người đi xe hai bánh phía trước-phải nằm gần các ô tô; cần tách hộp người và xe hai bánh khỏi ô tô bên cạnh |
| s05f09 | Cạnh trên (-y) | Van chở người phía sau-phải có thân cao; không phân loại thành Truck chỉ vì xe lớn hơn sedan |
| s05f28 | Cạnh trên (-y) | Một số xe phía đối diện bị cây hoặc vật ven đường che; điểm thưa ở vài bề mặt chưa đủ để kết luận do phản xạ thấp |


## 3. Ba cuboid có hướng dẫn

Ảnh mẫu được chiếu lại từ bảng A hiện tại: Car s01f08 #2 (`t2-a-car.jpg`), hộp đang gán Truck s01f08 #4 (`t2-b-bus-truck.jpg`), Cyclist s04f49 #3 (`t2-c-ped-cyclist.jpg`). 

**Ưu tiên sửa s01f08 #1 và #3:** hai hộp Cyclist gần trùng vị trí, tự kiểm báo chồng lấn. Đáy đang cao hơn mặt đường 0.84 m và 0.94 m. 

s01f08 #4 có cờ đáy cao 0.66 m; #6 có nhiều điểm sát ngoài mép; #7 có chiều cao 2.34 m vượt khoảng tham khảo Car 2.3 m. 

## 4. Bốn hiện tượng chất lượng

| Hiện tượng | Ảnh / vật | Quan sát và giả thuyết |
|---|---|---|
| Bóng che | t4-1-bong-che.jpg / s01f08 #1 | Người và xe hai bánh bị hàng cây che phần dưới; giả thuyết mất phản hồi do che khuất. Hộp hiện tại còn lỗi, ảnh không phải ví dụ nhãn đạt. |
| Thưa ở xa | t4-2-thua-o-xa.jpg / s03f43 #5 | Chỉ một phần bề mặt xe xa có phản hồi; giả thuyết mật độ quét giảm theo khoảng cách. Không coi phần thấy được là toàn bộ chiều dài. |
| Phản hồi không đồng đều | t4-3-phan-xa-thap.jpg / s05f28 #5 | Vùng thân/kính có ít điểm; vật liệu, góc tới và che khuất là các giả thuyết. XYZ không đủ chứng minh phản xạ thấp. |
| Điểm rời quanh vật | t4-4-diem-la.jpg / s03f00 #1 | Có điểm ngoài thân xe; giả thuyết có vật lân cận hoặc nhiễu. Không kéo hộp theo mọi điểm rời, không khẳng định multipath. |


## 5. Số từ cuboid của tôi

| Frame | idx | Nhãn hiện tại | z (m) | h (m) | z đáy (m) | dist (m) | yaw (độ) |
|---|---|---|---:|---:|---:|---:|---:|
| s01f08 | 2 | Car | -1.040 | 1.500 | -1.790 | 3.94 | 268.1 |
| s01f08 | 4 | Truck | -0.563 | 1.710 | -1.418 | 14.98 | 88.2 |
| s04f49 | 3 | Cyclist | -0.940 | 1.670 | -1.775 | 18.40 | 269.9 |

Gần nhất: **s01f58 #1, 2.95 m**. Xa nhất: **s03f43 #5, 37.16 m**. Tổng 54 hộp, frame ít nhất 5, số bị cờ 5. Các con số lấy từ bảng A và phiếu tự kiểm hiện tại.

Các cờ còn lại:
- s01f08 #1: đáy cao hơn mặt đường 0.84 m (lơ lửng?); chồng lên hộp 3 (mỗi vật 1 hộp)
- s01f08 #3: đáy cao hơn mặt đường 0.94 m (lơ lửng?); chồng lên hộp 1 (mỗi vật 1 hộp)
- s01f08 #4: đáy cao hơn mặt đường 0.66 m (lơ lửng?)
- s01f08 #6: 152 điểm sát ngay ngoài mép (quá chật, hay vật bên cạnh?)
- s01f08 #7: height=2.34 ngoài khoảng 1.2–2.3 của Car


## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc

**(a) Góc nhìn của mô hình học từ nhãn — hai câu:**

Mô hình cần học vị trí và kích thước thật của vật trong không gian, nên nhãn 3D dùng mét để biểu diễn nhất quán, còn kích thước theo pixel thay đổi theo khoảng cách, góc nhìn và camera. Vật thưa điểm cần quy tắc riêng vì phần phản hồi nhìn thấy có thể chỉ là một phần nhỏ của vật; nếu ôm sát các điểm đó hoặc đoán tùy ý, nhãn sẽ dạy mô hình sai kích thước và vị trí, nên chỉ hoàn thiện phần khuất khi có đủ bằng chứng đối chiếu.

**(b) Dùng AI — ba dòng:**

Không dùng

**(c) Điều còn chưa chắc và lỗi phát hiện khi rà lại:**

Tôi còn chưa chắc kích thước đầy đủ của các xe và người đi xe hai bánh bị hàng cây hoặc xe khác che, cũng như độ cao mặt đường tại phần đường đối diện. Tôi cũng chưa đủ bằng chứng để kết luận vùng ít điểm là do vật liệu phản xạ thấp hay do góc quét và che khuất. Khi rà lại đã phát hiện có bản nhãn được nhập vào nhầm bộ frame, một vật có nhiều hộp chồng nhau và hộp đặt sai hướng hoặc chưa bao trọn vật; các vấn đề này cần kiểm lại sau khi import và Save. Thời điểm phát hiện từng lỗi so với hạn nộp chưa được đối chiếu, nên tôi chưa xác nhận riêng lỗi nào là lỗi phát hiện sau hạn.
