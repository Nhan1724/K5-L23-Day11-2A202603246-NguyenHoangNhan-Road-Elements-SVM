# Báo cáo cá nhân — Ngày 11
Họ tên: Nguyễn Hoàng Nhân    Mã học viên: 2A202603246  
Phiên bản CVAT (Help → About): 2.18.0 (CVAT Community)  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 2  
Thời điểm export (ngày giờ trên CVAT): 2026-11-21 11:21:34 UTC+7    Số lần nộp lại sau Rework: 1  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng | `lane` (polyline), `road_curb` (polyline), `crosswalk` (polygon), `drivable` (polygon), `traffic_light` (rectangle), `traffic_sign` (rectangle) |
| Thuộc tính và giá trị | Đúng | Khớp hoàn toàn với schema sinh từ `taxonomy.py` và cấu hình `configs/road_v1_labels.json` |
| 4 tag ảnh | Đúng | `camera`, `timeofday`, `weather`, `source` (mỗi tag mang một thuộc tính select tên `value`) |

Cách kết thúc polyline trên bản CVAT này: Nhấp đúp chuột trái vào điểm cuối hoặc bấm phím `N` / phím `Esc`.  
Cách đặt tag ảnh trên bản CVAT này: Chuyển sang bảng điều khiển bên phải (Objects list), chọn mục Tags, chọn label tag tương ứng và chọn giá trị trong trường select `value`.  

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 28 | 0 | 9 | 24 | 37 | 52 |
| front-extra (đã làm 0 / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| fisheye | 8 | 7 | 0 | 7 | 15 | 7 | 14 |
| fisheye-extra (đã làm 0 / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

*Ghi chú:*
- Trong Task front: Đã gán đủ 12 ảnh core (A01–A12). Tổng cộng 150 shapes.
- Trong Task fisheye: Đã gán đủ 8 ảnh core (B01–B08). Tổng cộng 50 shapes.
- Mọi ảnh (20/20 ảnh) đều có đủ 4 tag phân loại cảnh quan.

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A03 | `lane` | Dừng polyline trước đuôi xe hay vẽ đè xuyên qua xe con đang chạy | Vẽ xuyên qua và bật cờ `occluded` | Guideline mục 3 quy định vật cản/xe cộ di chuyển ngắn che mất vạch thì vẽ liền qua và bật `occluded`, không dừng cụt trước xe |
| A04 | `lane` | Vẽ 2 polyline song song hay 1 polyline giữa hai nét sơn vàng | 1 polyline `kind=double`, `color=yellow` | Vạch vàng đôi tim đường chỉ vẽ 1 đường polyline duy nhất ở tâm dải phân cách; vẽ 2 đường sẽ vi phạm lỗi thừa nhãn QC1-03 |
| A04 | `drivable` | Gộp chung mặt đường hay tách hai vùng | Tách `direct` cho làn đang chạy và `alternative` cho làn bên cạnh | Mặt đường có vạch kẻ làn rõ ràng, làn xe mình đi là `direct`, làn cùng chiều lân cận là `alternative` |
| A05 | `lane` | Cố tìm vạch mờ để vẽ L1/R1 hay không vẽ | Không vẽ `lane`, chỉ khoanh `drivable direct` | Đường nội bộ khu dân cư không có vạch kẻ sơn; tự vẽ vạch giả sẽ bị lỗi QC1-03; đã xác nhận với coach để miễn trừ QC1-02 |
| A07 | `traffic_light` | Đèn ngã tư ở xa có chiều cao nhỏ | Đóng box cho đèn >= 8 px, bỏ qua đèn < 8 px | Tuân thủ nghiêm ngặt quy tắc kích thước tối thiểu 8 px để tránh gán nhãn nhiễu |
| B02 | `traffic_light` | Giữ `edge_zone=false` mặc định hay bật `true` | Bật `edge_zone=true` | Tâm của cụm đèn tín hiệu nằm ở vùng rìa ngoài (> 80% bán kính ảnh fisheye), nơi hình học bị méo quang học mạnh |
| B03 | `lane` / `drivable` | Góc nhìn camera sau bị lật ngược trái/phải | Đặt `position=other` cho lane, drivable chừa phần đường bên phải vạch vàng | Fisheye camera sau quay ngược chiều xe chạy nên làn đối diện nằm bên phải ảnh; quy tắc fisheye cam sau yêu cầu position=other |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: Võ Tiến Phát (`votienphat`)    Quyết định nhận được: **Rework** (lần 1) → **Release** (lần 2 sau buffer)  

| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-03 (position) | 2 | Đồng ý | Đã sửa: Ở A07 sửa vạch bên phải từ L2 thành R2; ở A12 sắp xếp lại L1/R1/R2 chuẩn xác |
| QC1-05 (vạch đứt) | 1 | Đồng ý | Đã sửa: Ở A12 gộp 2 đoạn polyline của vạch đứt L2 thành 1 đường duy nhất |
| QC1-11 (fisheye edge_zone) | 9 | Đồng ý | Đã sửa: Bật cờ `edge_zone=true` cho toàn bộ các box đèn và biển báo nằm ngoài 80% bán kính (B02, B03, B04, B05) |
| QC1-11 (fisheye arc) | 3 | Đồng ý | Đã sửa: Ở B06 và B07, nội suy polyline 3 điểm thành đường cong 5 điểm ôm sát độ cong ống kính fisheye |
| QC1-02 (thiếu L1/R1) | 5 | Đồng ý | Giữ nguyên: Đây là các ca đường không kẻ vạch hoặc góc khuất (A05, A06, A07, A09) đã được coach duyệt miễn trừ hợp lệ |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `nguyenhoangnhan-front.zip` | Có (`road_check lint` PASS) | 9 / 9 (ảnh A01: drivable 3, lane 4, traffic_sign 2) | 6 / 6 (ảnh A02: drivable 3, lane 3) | Có (`<tag label="camera"><attribute name="value">front</attribute></tag>`) |
| `nguyenhoangnhan-fisheye.zip` | Có (`road_check lint` PASS) | 2 / 2 (ảnh B01: crosswalk 1, drivable 1) | 6 / 6 (ảnh B02: crosswalk 1, drivable 1, traffic_light 4) | Có (`<tag label="camera"><attribute name="value">fisheye_front</attribute></tag>`) |

## 6. Ảnh chụp màn hình
1. **Tab Labels của project:** Đầy đủ 6 label hình học (`lane`, `road_curb`, `crosswalk`, `drivable`, `traffic_light`, `traffic_sign`) cùng 4 tag (`camera`, `timeofday`, `weather`, `source`) với đầy đủ thuộc tính và giá trị tương ứng file `configs/road_v1_labels.json`.
2. **Một ảnh camera trước đã gán đủ:** Ảnh A01 được gán toàn diện: 4 polyline lane (L2, L1, R1, R2), 3 polygon drivable (direct ở giữa, alternative hai bên), 2 rectangle traffic_sign ôm sát biển báo.
3. **Một ảnh fisheye đã gán:** Ảnh B02 phóng to cụm đèn tín hiệu ở góc trên rìa ảnh, hiển thị box ôm sát viền và cờ `edge_zone=true` được tích chọn.
4. **Một đoạn XML có `<polyline label="lane">`:** Trích đoạn cấu trúc CVAT XML chuẩn:
```xml
<polyline label="lane" occluded="0" points="106.32,720.00;154.51,680.00;202.64,640.00;250.77,600.00;298.89,560.00">
  <attribute name="position">L1</attribute>
  <attribute name="style">dashed</attribute>
  <attribute name="color">white</attribute>
  <attribute name="kind">single</attribute>
</polyline>
```

## 7. Tự đánh giá
- **Lỗi mắc nhiều nhất hôm nay và cách tránh ở Ngày 20:**  
  Lỗi phổ biến nhất là bỏ quên cờ `edge_zone` ở các ảnh fisheye và tách vạch đứt thành nhiều polyline riêng biệt khi vẽ nhanh. Để tránh lặp lại ở Ngày 20: Luôn luôn chạy script `road_check lint` ngay sau khi export bản nháp để phát hiện các lỗi cảnh báo hình học tự động, đồng thời rà soát kỹ bảng Details thuộc tính trước khi bấm *Finish the job*.

- **Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào:**  
  Nhãn vạch kẻ đường (`lane`) và vùng lái được (`drivable`) là dữ liệu huấn luyện cốt lõi cho các mạng nơ-ron nhận thức môi trường lái xe tự hành (ví dụ: mô hình phát hiện làn xe theo hàng neo *Row-anchor Lane Detection* như Ultra-Fast-Lane-Detection, hoặc phân đoạn ngữ nghĩa *Semantic Segmentation*).  
  Nếu lỗi **gán sai thuộc tính `position` (như gán vạch bên phải thành `L2` thay vì `R2`, hoặc đảo ngược L1 và R1)** không được sửa: mô hình AI sẽ học nhầm đặc trưng không gian của làn xe. Khi triển khai tính năng Hỗ trợ Giữ làn (Lane Keeping Assist - LKA) hoặc Chuyển làn tự động, hệ thống điều khiển sẽ nhầm lẫn làn xe đang đi với làn ngược chiều hoặc làn kế bên, dẫn tới việc tính toán sai độ lệch tâm (lateral offset), phát lệnh đánh lái sai quỹ đạo an toàn và có thể gây va chạm nguy hiểm.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
Dữ liệu kiểm tra thực tế bằng lệnh `python -m road_lab.rowanchor nguyenhoangnhan-front.zip --image A01 --position R1`:

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi (Nhân) | không có làn | 655.6 | 598.8 | 542.0 | 485.1 |
| x của bạn kia (Phát) | không có làn | 654.8 | 597.9 | 541.2 | 484.5 |

**Trả lời 3 câu hỏi:**

1. **Năm số x đó chính là đích mà một model row-anchor học cho vạch này. Nếu bạn dừng polyline sớm 40 px, hàng nào đổi thành "không có làn"?**  
   - Hàng neo `y = 560` sẽ chuyển thành "không có làn". Vì polyline bị dừng ở cao độ thấp hơn (y > 560, tức cách xa hơn 40 px theo chiều dọc ảnh), điểm cao nhất của polyline không còn vươn tới hàng neo 560, khiến thuật toán nội suy không tìm được giá trị x tương ứng.

2. **So với bạn được kiểm (cùng ảnh): ở hàng 720 và hàng 560, hai bạn lệch nhau bao nhiêu pixel? Lệch ở hàng nào ảnh hưởng LKA nhiều hơn (slide 25)?**  
   - Ở hàng 720: Cả hai đều là "không có làn" (độ lệch 0 px).  
   - Ở hàng 560: Độ lệch là $|485.1 - 484.5| = 0.6$ px (nằm trong giới hạn sai số cực nhỏ < 1 px).  
   - **Lệch ở hàng gần xe (như 680) ảnh hưởng trực tiếp đến LKA ngay lập tức** do quyết định góc đánh lái tức thời, nhưng **lệch ở các hàng xa (như 560)** sẽ làm sai lệch độ cong ước lượng phía trước của con đường, gây dao động (hunting) khi xe di chuyển ở tốc độ cao.

3. **Nếu polyline vẽ từ xa về gần (ngược quy tắc 2), cách tính trên có đổi không? Script nào sẽ gặp khó?**  
   - Về mặt toán học nội suy hình học thuần túy giữa 2 điểm liên tiếp, công thức nội suy tuyến tính $x(y)$ vẫn tìm được nghiệm x nếu xét toàn bộ tập đoạn thẳng.  
   - Tuy nhiên, thứ tự mảng điểm bị đảo ngược sẽ khiến các thuật toán xử lý tuần tự (như quét điểm từ đáy ảnh lên hoặc giả định điểm đầu tiên $P_0$ sát xe để căn gốc) bị sai lệch logic. Cụ thể, các script kiểm tra chiều vẽ và kiểm tra điểm bắt đầu (`road_check.py`, `test_checker.py`, `rowanchor.py`) sẽ lập tức cảnh báo lỗi chiều ngược (`QC1-04`), đồng thời pipeline tiền xử lý dữ liệu cho mạng nơ-ron tuần tự (RNN/LSTM/Transformer quét theo hướng tiến của xe) sẽ nhận vector tọa độ ngược chiều quy ước.
