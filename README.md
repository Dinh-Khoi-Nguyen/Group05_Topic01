\# Tên đề tài: "Phân tích và Dự đoán Thời gian Phản hồi Dịch vụ Web dựa trên Đặc trưng Mạng"



\## 1. Thành viên nhóm và mã sinh viên



| STT | Họ và tên | Mã sinh viên |

|-----|-----------|--------------|

| 1   | Đinh Khôi Nguyên  | 2519960039     |

| 2   | Nguyễn Việt A  | 2519960002     |

| 3   | Nguyễn Cao Hoàng Minh  | 2519960034     |

| 4   | Nguyễn Sỹ Sơn  | 2519960045     |

| 5   | Dương Tiến An  | 2519960003     |



\## 2. Mô tả ngắn bài toán



Dự án đo đạc và phân tích các thông số mạng (DNS, TCP, TLS, RTT, kích thước phản hồi, loại domain, thời điểm truy cập) khi gửi request tới 15 website công cộng phổ biến tại Việt Nam, nhằm xác định mức độ ảnh hưởng của từng đặc trưng đến thời gian phản hồi (`response\_time\_ms`). Trên cơ sở dữ liệu thu thập được, nhóm xây dựng và so sánh bốn mô hình dự đoán: Baseline (Mean), Linear Regression, Decision Tree Regressor và Random Forest Regressor.



\## 3. Môi trường chạy và Python version



\- Python \*\*3.13.3\*\*

\- Hệ điều hành: không phụ thuộc 

\- Kiểm tra version đã cài: `python3 --version` (hoặc `python --version`)



\## 4. Thư viện cần cài và cách cài từ requirements.txt



Cài đặt toàn bộ thư viện cần thiết bằng:



```bash

pip install -r requirements.txt

```



Danh sách thư viện:



| Thư viện | Version | Dùng cho |

|----------|---------|----------|

| numpy | >=1.26 | `src/main.py` |

| pandas | >=2.0 | `src/main.py` |

| scikit-learn | >=1.3 | `src/main.py` |

| matplotlib | >=3.7 | `src/main.py` |

| pycurl | >=7.45 | `src/crawl.py` |

| pythonping | >=1.1 | `src/crawl.py` |



> Lưu ý: `pycurl` yêu cầu thư viện hệ thống `libcurl` đã được cài sẵn trên máy trước khi `pip install` (trên Windows có thể cần cài qua wheel tương ứng nếu build lỗi).



\## 5. Dataset: nguồn, vị trí file, cách tạo hoặc cách tải



\- \*\*Nguồn:\*\* dữ liệu được thu thập chủ động bằng `src/crawl.py`, gửi GET request (qua PycURL) và đo ping (qua `pythonping`) tới 15 URL công khai (Google, Facebook, X, TikTok, YouTube, Shopee, Tuổi Trẻ, Wikipedia, VietJack, Báo Mới, Apple, Pinterest, Microsoft, Cốc Cốc, Lazada), lặp 10 lần/site mỗi lượt chạy.

\- \*\*Vị trí file:\*\* `data/Dataset\_finale.csv`

\- \*\*Cách tạo:\*\*

&#x20; - Nếu đã có sẵn `Dataset\_finale.csv` (nộp kèm gói bài): đặt trực tiếp vào thư mục `data/`.

&#x20; - Nếu muốn tạo mới / thu thập lại: chạy `python src/crawl.py` — script tự tạo `data/` nếu chưa có và \*\*append\*\* thêm bản ghi vào `Dataset\_finale.csv` nếu file đã tồn tại (chạy nhiều lần ở nhiều thời điểm trong ngày để tái lập đúng quy trình thu thập gốc — xem mục 9).



\## 6. Cấu trúc thư mục



```

Group05\_Topic01/

├── Report_Group05.pdf

├── Report_Group05.docx

├── README.md

├── requirements.txt

├── data/

│   └── Dataset\_finale.csv

├── src/

│   ├── crawl.py

│   └── main.py

└── results/

&#x20;   ├── tables/

&#x20;   └── figures/

```



\## 7. Thứ tự chạy các script



1\. `pip install -r requirements.txt`

2\. \*(Tùy chọn — chỉ nếu cần tạo lại dataset)\* `python src/crawl.py`

3\. `python src/main.py` — chạy toàn bộ pipeline: Load → Clean → Analyze → Feature Engineering → Train → Evaluate → Visualize.



Chạy cả hai script từ thư mục gốc project (`Group05\_Topic01/`), không chạy từ trong `src/`.



\## 8. Output mong đợi



| Loại | Số lượng | Vị trí |

|------|----------|--------|

| Bảng thống kê mô tả (train) | 1 | `results/tables/describe\_train.csv` |

| Biểu đồ scatter đặc trưng–mục tiêu | 6 | `results/figures/feature\_vs\_target/` |

| Bảng + biểu đồ tương quan Pearson | 1 bảng + 1 biểu đồ | `results/tables/pearson\_correlations.csv`, `results/figures/pearson\_correlations.png` |

| Bảng hệ số/độ quan trọng đặc trưng | 1 | `results/tables/model\_coefficients.csv` |

| Bảng kết quả đánh giá tổng hợp (MAE, RMSE, R², Accuracy) | 1 | `results/tables/model\_results.csv` |

| Bảng phân tích sai số lớn nhất (top 10) | 3 | `results/tables/error\_analysis\_<tên\_mô\_hình>\_top10.csv` |

| Biểu đồ chẩn đoán mô hình (actual vs predicted, residual plot, residual hist) | 12 | `results/figures/<tên\_mô\_hình>/` |



\*\*Metric chính:\*\* MAE, RMSE, R², MAPE/Accuracy — xem `model\_results.csv`.






\## 9. Ghi chú về random\_state, seed, cấu hình để tái lập kết quả



\- `random\_state = 42` được cố định xuyên suốt: `train\_test\_split`, `IsolationForest`, `DecisionTreeRegressor`, `RandomForestRegressor` → tái lập chính xác 100% kết quả với cùng dữ liệu đầu vào.

\- Tham số Isolation Forest: `contamination = 0.02`.

\- Cấu hình mô hình: Decision Tree (`max\_depth=6`), Random Forest (`n\_estimators=200, max\_depth=8`), Linear Regression (mặc định scikit-learn).

\- Mọi bước fit (Isolation Forest, one-hot categories, `StandardScaler`) chỉ fit trên tập \*\*train\*\*, áp dụng sang test — không có data leakage.

\- Nếu chạy lại `crawl.py` để thu thập dữ liệu mới, kết quả số liệu ở Chương 5 của báo cáo \*\*sẽ không tái lập chính xác\*\* (dữ liệu mạng thực tế thay đổi theo thời điểm đo) — chỉ dataset gốc `Dataset\_finale.csv` nộp kèm mới cho đúng số liệu trong báo cáo.



\## 10. Các giới hạn hoặc lưu ý an toàn



\- `crawl.py` chỉ gửi GET request tới các trang public, không xác thực, không bypass bất kỳ cơ chế bảo vệ nào.

\- Tần suất request được kiểm soát thấp: 10 request/site mỗi lượt chạy, các lượt chạy cách nhau \~3 giờ (8 lượt/ngày) — tránh gây quá tải hoặc bị hiểu nhầm là tấn công (DoS) lên server mục tiêu.

\- Dataset không chứa thông tin cá nhân hoặc dữ liệu nhạy cảm — chỉ gồm thông số thời gian mạng và metadata request.

\- Domain\_category được gán thủ công (hardcode trong `crawl.py`) — nếu đổi/thêm URL cần cập nhật lại dict `categories` tương ứng, nếu không script sẽ lỗi khi không tìm được category cho URL mới.
