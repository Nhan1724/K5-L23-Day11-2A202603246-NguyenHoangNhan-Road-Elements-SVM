# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): Võ Tiến Phát (`votienphat`)        Người kiểm (học viên): Nguyễn Hoàng Nhân (`nguyenhoangnhan`)  
Ngày: 2026-11-21                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `votienphat-front.zip`, `votienphat-fisheye.zip` (manifest `revision 1`)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm, ghi riêng)   Số nhãn đã kiểm (shape + tag): 56  

Các ảnh được chọn kiểm theo mẫu (4 front + 2 fisheye):
- `A01_bc5fadac-fa6dd225.jpg` (front, trọng tâm: vạch lane L1/R1/L2/R2, drivable)
- `A03_b1c66a42-6f7d68ca.jpg` (front, trọng tâm: vạch bị che occluded, đèn/biển giao thông)
- `A04_b7a8e795-0ff55db2.jpg` (front, trọng tâm: vạch vàng đôi kind=double, drivable direct/alternative)
- `A05_b2576c8c-2de20b8a.jpg` (front, trọng tâm: ca khó không có vạch kẻ làn, road_curb)
- `B01_02688_FV.png` (fisheye_front, trọng tâm: vạch cong theo ống kính fisheye, crosswalk)
- `B02_03177_FV.png` (fisheye_front, trọng tâm: traffic_light sát mép và cờ edge_zone)

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 5 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 12 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 12 | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 6 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 12 | 1 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 11 | 0 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 11 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 15 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 8 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 6 | 1 | chính |
| QC1-12 file | lỗi file | 2 | 0 | nhẹ / nặng |

*Ghi chú ngoại lệ QC1-02 tại A05:* Ảnh đường nội bộ không có vạch sơn phân chia làn xe, học viên không tự vẽ vạch giả mà khoanh drivable direct và road_curb; đã đối chiếu xác nhận với coach được miễn trừ QC1-02.

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-04 | A01 | lane (R1) | chính | Điểm dừng xa của polyline R1 kéo hơi sớm khoảng 20 px so với điểm xa nhất còn thấy vạch sơn |
| 2 | QC1-06 | A04 | lane (L1) | chính | Vạch vàng đôi tim đường gắn thuộc tính `kind=single` thay vì `kind=double` |
| 3 | QC1-11 | B02 | traffic_light | chính | Cụm đèn bên phải có tâm nằm ngoài 80% bán kính ảnh fisheye nhưng chưa tick `edge_zone=true` |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A03 | Đèn tín hiệu giao thông ngã tư ở xa (chiều cao ~8-9 px) quay nghiêng, phân vân giữa `facing=side` hay `facing=back` | Lead giải đáp: Nhìn thấy cạnh thân đèn nghiêng thì ưu tiên chọn `facing=side`, đặt `color=unknown` |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 3 / 56 nhãn = 5.36 % (trước buffer) -> Sau khi trao đổi trực tiếp trong giờ Buffer, bạn đã cập nhật tick `edge_zone` ở B02 và sửa `kind=double` ở A04, đưa số lỗi chính về 1 / 56 (1.79 %)   Lỗi nhẹ: 0   Ca chờ lead: 0  
Quyết định (Release / Hold / Rework): **Release**  
Lý do (theo luật của phiếu): Không có lỗi nặng (QC1-01, QC1-02 ngoại lệ đã xác nhận, QC1-03, QC1-08 front đều chuẩn); tỉ lệ lỗi chính sau khi hoàn thiện buffer đạt 1.79% <= 5%, đủ điều kiện Release lô gán nhãn.
