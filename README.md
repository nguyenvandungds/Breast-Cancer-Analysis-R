# Phân tích dữ liệu và phân loại khối u vú bằng R

**Breast Cancer Data Analysis and Classification in R**

Dự án học thuật cá nhân sử dụng R để khám phá dữ liệu sinh thiết vú Wisconsin và so sánh các phương pháp học thống kê trong bài toán phân loại khối u **lành tính (benign)** và **ác tính (malignant)**.

Dự án được phát triển từ tiểu luận học phần **Lý thuyết học thống kê**, Trường Đại học Đà Lạt, năm 2024. Nội dung tập trung vào thống kê mô tả, trực quan hóa, phân tích tương quan và đánh giá mô hình phân loại.

## 1. Mục tiêu

- Khám phá phân phối và đặc điểm của các biến trong dữ liệu sinh thiết.
- Phân tích mối liên hệ giữa các đặc trưng và khả năng phân biệt hai lớp.
- Xây dựng bộ phân loại bằng hồi quy tuyến tính, hồi quy logistic và phân tích biệt thức tuyến tính (LDA).
- So sánh kết quả khi sử dụng từng cặp đặc trưng và toàn bộ 9 đặc trưng.
- Đánh giá mô hình bằng ma trận nhầm lẫn và tỷ lệ phân loại sai trên tập kiểm tra.

## 2. Dữ liệu

Theo phần giới thiệu trong báo cáo, dữ liệu được thu thập bởi Tiến sĩ William H. Wolberg tại Bệnh viện Đại học Wisconsin, Madison. Bộ dữ liệu ban đầu có **699 quan sát**, gồm mã mẫu, 9 đặc trưng và nhãn phân loại. Các đặc trưng được chấm điểm từ **1 đến 10**.

| Biến trong phân tích | Ý nghĩa |
| --- | --- |
| `ID` | Mã mẫu; không dùng làm đặc trưng dự báo |
| `thick` | Độ dày cụm tế bào |
| `u.size` | Mức độ đồng đều về kích thước tế bào |
| `u.shape` | Mức độ đồng đều về hình dạng tế bào |
| `adhesion` | Độ bám dính ở rìa |
| `s.size` | Kích thước tế bào biểu mô đơn lẻ |
| `nuclei` | Nhân trần |
| `chromatin` | Chất nhiễm sắc thô |
| `nucleoli` | Hạch nhân bình thường |
| `mitoses` | Phân bào |
| `type` | Nhãn: `benign` hoặc `malignant` |

Các bảng kết quả trong báo cáo sử dụng **683 quan sát**:

| Tập dữ liệu | Lành tính | Ác tính | Tổng |
| --- | ---: | ---: | ---: |
| Huấn luyện | 309 | 169 | 478 |
| Kiểm tra | 135 | 70 | 205 |
| Tổng | 444 | 239 | 683 |

Tỷ lệ chia dữ liệu xấp xỉ **70% huấn luyện / 30% kiểm tra**. Số quan sát được tổng hợp từ trang 8 và trang 11 của báo cáo. Chênh lệch 16 quan sát so với dữ liệu ban đầu cần được đối chiếu với mã tiền xử lý để xác nhận đầy đủ nguyên nhân loại bỏ.

## 3. Quy trình phân tích

### Phân tích dữ liệu thăm dò (EDA)

- Thống kê mô tả: giá trị nhỏ nhất, lớn nhất, trung bình, trung vị và các tứ phân vị.
- Khảo sát phân phối bằng biểu đồ tần suất, histogram, ECDF, boxplot, P–P plot và Q–Q plot.
- Phân tích liên hệ giữa các biến bằng ma trận tương quan và ma trận biểu đồ phân tán.
- Trực quan hóa từng cặp đặc trưng theo nhãn lành tính và ác tính.

### Xây dựng mô hình

| Phương pháp | Vai trò trong dự án |
| --- | --- |
| Hồi quy tuyến tính | Mô hình đối chiếu khi mã hóa nhãn nhị phân và chuyển dự báo thành nhãn bằng ngưỡng |
| Hồi quy logistic | Mô hình phân loại nhị phân dựa trên xác suất |
| LDA | Phân loại dựa trên sự phân tách tuyến tính giữa hai lớp |

Hồi quy tuyến tính được sử dụng cho mục đích so sánh học thuật; đầu ra của mô hình không bị giới hạn trong khoảng từ 0 đến 1 như xác suất của hồi quy logistic.

Mỗi phương pháp được khảo sát với 5 cấu hình đầu vào:

1. `u.size` và `u.shape`.
2. `u.size` và `adhesion`.
3. `u.size` và `s.size`.
4. `thick` và `u.shape`.
5. Toàn bộ 9 đặc trưng.

### Đánh giá

Chỉ số chính trong báo cáo là **tỷ lệ phân loại sai**:

```text
Tỷ lệ phân loại sai = Số mẫu dự đoán sai / Tổng số mẫu đánh giá
Accuracy = 1 − Tỷ lệ phân loại sai
```

Ma trận nhầm lẫn được sử dụng để quan sát các trường hợp dự đoán đúng và sai ở từng lớp. Với các mô hình hai biến hồi quy tuyến tính và logistic, báo cáo còn trực quan hóa biên quyết định.

## 4. Kết quả

Tỷ lệ phân loại sai trên tập kiểm tra, tổng hợp từ trang 40 của báo cáo (càng thấp càng tốt):

| Nhóm đặc trưng | Hồi quy tuyến tính | Hồi quy logistic | LDA |
| --- | ---: | ---: | ---: |
| `u.size` + `u.shape` | 10,73% | **7,32%** | 13,17% |
| `u.size` + `adhesion` | 9,27% | **7,32%** | 8,29% |
| `u.size` + `s.size` | 11,71% | **8,78%** | 10,73% |
| `thick` + `u.shape` | 7,32% | **4,39%** | 7,32% |
| Toàn bộ 9 đặc trưng | **4,39%** | **4,39%** | **4,39%** |

### Nhận xét chính

- **Hồi quy logistic có tỷ lệ lỗi thấp nhất trong cả 4 cấu hình hai đặc trưng** trên lần chia dữ liệu được báo cáo.
- Logistic với `thick` và `u.shape` đạt **accuracy khoảng 95,61%**, tương đương 196/205 mẫu kiểm tra được phân loại đúng.
- Khi sử dụng 9 đặc trưng, cả ba phương pháp đều phân loại sai 9/205 mẫu, tương ứng tỷ lệ lỗi **4,39%**. Tuy nhiên, cùng tỷ lệ lỗi không có nghĩa là các mô hình dự đoán sai cùng những mẫu hoặc cùng số lượng ở từng lớp.
- Ma trận tương quan ở trang 9 cho thấy `u.size` và `u.shape` có tương quan dương cao, khoảng **0,91**. Đây là điểm cần cân nhắc khi diễn giải hệ số và đánh giá thông tin trùng lặp giữa các biến.
- Kết quả logistic hai biến bằng kết quả dùng 9 biến trên tập kiểm tra này gợi ý khả năng xây dựng mô hình gọn hơn; chưa đủ để kết luận hai biến luôn tối ưu trên dữ liệu mới.

## 5. Công cụ và kỹ năng

**Ngôn ngữ:** R.

**Kỹ năng thể hiện qua dự án:**

- Thống kê mô tả và phân tích dữ liệu thăm dò.
- Trực quan hóa phân phối, tương quan và biên quyết định.
- Xây dựng và diễn giải mô hình học thống kê.
- Phân chia dữ liệu huấn luyện và kiểm tra.
- Đọc ma trận nhầm lẫn, so sánh sai số phân loại.
- Tổng hợp kết quả và viết báo cáo phân tích.

## 6. Giới hạn và hướng phát triển

Kết quả hiện tại dựa trên **một lần chia train/test**. Báo cáo chưa trình bày đánh giá bằng cross-validation, khoảng tin cậy của chỉ số hoặc kiểm định chênh lệch hiệu suất giữa các mô hình.

Các hướng cải tiến dự kiến:

- Chuẩn hóa mã nguồn, ghi rõ bước xử lý dữ liệu thiếu, mẫu trùng và mã mẫu lặp.
- Cố định seed và sử dụng chia dữ liệu phân tầng để tăng khả năng tái lập.
- Kiểm tra các mẫu cùng bệnh nhân hoặc cùng mã mẫu để tránh rò rỉ dữ liệu giữa train và test.
- Sử dụng cross-validation trên tập huấn luyện để chọn mô hình; giữ tập kiểm tra cho đánh giá cuối cùng.
- Bổ sung precision, recall, F1-score và ROC-AUC; chú ý recall của lớp ác tính và số âm tính giả.
- Khảo sát ngưỡng phân loại và đánh đổi giữa các loại sai số.
- Đóng gói quy trình bằng R Markdown hoặc Quarto để tái tạo báo cáo từ mã nguồn.

Đây là dự án phục vụ học tập và thực hành phân tích dữ liệu, chưa phải hệ thống chẩn đoán được kiểm chứng để sử dụng lâm sàng.

## 7. Tài liệu dự án

- **Báo cáo gốc:** `LTH_ThongKe.pdf` — tiểu luận gồm 40 trang, trình bày EDA, mô hình, ma trận nhầm lẫn và bảng so sánh kết quả.
- **Tài liệu tham khảo:** Slide và bài giảng học phần Lý thuyết học thống kê của giảng viên Đặng Phước Huy.

README này mô tả kết quả trong báo cáo gốc. Hướng dẫn chạy lại sẽ được bổ sung cùng mã nguồn R, dữ liệu và danh sách thư viện phụ thuộc.

## 8. Tác giả

**Nguyễn Văn Dũng**  
Trường Đại học Đà Lạt  
Dự án học thuật cá nhân — Học phần Lý thuyết học thống kê, 2024.
