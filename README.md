# C07_BAITHUCHANH

Bộ bài thực hành về ứng dụng trí tuệ nhân tạo trong an toàn thông tin, gồm ba nội dung công việc:

| Công việc | Chủ đề | Số bài |
|---|---|---|
| **CV4.1** | Phát hiện tấn công mạng | 17 |
| **CV4.2** | Phát hiện mã độc | 16 |
| **CV4.4** | Phát hiện tấn công trong hệ thống ICS | 10 |

Mỗi bài gồm một notebook (đã lưu kèm toàn bộ output) và một báo cáo `.docx`.

---

## Cài đặt

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows
pip install -r requirements.txt
```

### Chỉ làm 15 bài học máy cổ điển

Nếu chỉ cần nhóm bài KNN / Naive Bayes / SVM / Phát hiện bất thường / Ensemble thì cài riêng nhóm 1 là đủ — nhẹ hơn rất nhiều vì không kéo theo TensorFlow và PyTorch:

```bash
pip install numpy==1.26.4 pandas==1.5.1 scikit-learn==1.2.0 matplotlib==3.8.2 seaborn==0.13.1 jupyterlab==4.1.5
```

### Các nhóm thư viện

| Nhóm | Thư viện | Dùng ở bài |
|---|---|---|
| 1. Học máy cổ điển | `numpy` `pandas` `scikit-learn` `matplotlib` `seaborn` `jupyterlab` | **Toàn bộ 15 bài mới**, CV4.1 B1–B4, CV4.2 B1–B5 và B11, CV4.4 B1–B2 |
| 2. Gradient boosting | `xgboost` | CV4.1 B3, B4, B6, B8, B10 |
| 3. Học sâu | `tensorflow` | CV4.1 B5–B10, CV4.2 B6–B9, CV4.4 B3–B4 |
| 4. Mô hình ngôn ngữ | `torch` `transformers` `datasets` | CV4.1 B11–B12, CV4.2 B10, CV4.4 B5 |
| 5. Giải thích mô hình | `shap` `lime` | CV4.2 B11 |
| 6. Thống kê | `statsmodels` | CV4.2 B4 |

### Ba lưu ý khi cài

**`torch` sẽ cài bản CPU.** Dòng `torch==2.6.0` lấy bản mặc định trên PyPI — chạy được nhưng không dùng GPU. Muốn bản CUDA:

```bash
pip install torch==2.6.0 --index-url https://download.pytorch.org/whl/cu124
```

**`tensorflow==2.11.0` sẽ hạ cấp `protobuf` xuống 3.19.6.** Trong môi trường ảo mới thì không sao; nếu cài trực tiếp vào môi trường đang dùng thì nên biết trước. Lưu ý thêm: 2.11 là bản cuối còn hỗ trợ GPU trực tiếp trên Windows, từ 2.12 trở đi phải chạy qua WSL2.

**`statsmodels` dùng ràng buộc `>=0.14`** thay vì ghim phiên bản cố định như các gói khác, vì gói này chưa được cài trên máy đã chạy toàn bộ notebook. Nếu `CV4.2/B4` báo `ModuleNotFoundError: No module named 'statsmodels'` thì chỉ cần cài riêng nó.

### Nếu pip báo lỗi `UnicodeDecodeError` khi đọc requirements.txt

pip từ phiên bản 24.x trở về trước giải mã `requirements.txt` bằng **bảng mã hệ thống** (trên Windows tiếng Việt là cp1252), nên file chứa ký tự có dấu sẽ gây lỗi:

```
UnicodeDecodeError: 'charmap' codec can't decode byte 0x90 in position 4
```

File `requirements.txt` trong repo này đã được viết thuần ASCII nên không gặp lỗi đó. Nếu bạn tự thêm chú thích tiếng Việt vào file, hãy nâng pip lên bản mới (`python -m pip install --upgrade pip`) — từ 25.0 trở đi pip luôn đọc UTF-8.

---

## Cấu trúc thư mục

```
CV4.1/
  B1/ ... B17/       mỗi thư mục: 1 notebook + 1 báo cáo .docx
  Dataset/           bộ dữ liệu (không có trong repo)
CV4.2/
  B1/ ... B16/
  Dataset/
CV4.4/
  B1/ ... B10/
    data/BATADAL/    bộ dữ liệu riêng của từng bài (không có trong repo)
```

## Về bộ dữ liệu

**Repo không chứa bộ dữ liệu.** Lý do: dữ liệu đã được cài sẵn trong máy ảo của học viên, và `CV4.1/Dataset/data_capec_multilabel.csv` nặng 417 MB — vượt giới hạn 100 MB mỗi file của GitHub.

Điều này không cản trở việc đọc bài: **mọi notebook đều đã lưu kèm output**, nên xem được toàn bộ bảng số liệu và biểu đồ mà không cần chạy lại.

Muốn chạy lại, đặt dữ liệu vào đúng vị trí trong sơ đồ trên. Phần cấu hình (mục 2) của mỗi notebook có danh sách đường dẫn dự phòng nên tự dò được nhiều vị trí khác nhau. Riêng BATADAL, file `CV4.4/B*/data/README.md` có hướng dẫn tải từ nguồn gốc.

---

## 15 bài học máy cổ điển

Năm thuật toán, mỗi thuật toán áp dụng lên ba miền dữ liệu khác nhau:

| Thuật toán | CV4.1 (lưu lượng mạng) | CV4.2 (PE header) | CV4.4 (cảm biến ICS) |
|---|---|---|---|
| KNN | B13 | B12 | B6 |
| Naive Bayes | B14 | B13 | B7 |
| SVM | B15 | B14 | B8 |
| Phát hiện bất thường không giám sát | B16 | B15 | B9 |
| Voting & Stacking Ensemble | B17 | B16 | B10 |

Toàn bộ 15 notebook chỉ dùng `scikit-learn`, không cần GPU, và đã chạy trọn trên dữ liệu thật với `SEED = 42`.

Cách tổ chức này cho phép so sánh trực tiếp cùng một thuật toán trên ba loại dữ liệu — và kết quả thường trái ngược nhau giữa ba miền, ví dụ:

- Quy tắc "KNN luôn phải chuẩn hóa đặc trưng" **sai ở hai trong ba** bộ dữ liệu: chuẩn hóa làm *giảm* F1 trên PE header, gần như không đổi trên lưu lượng mạng, và chỉ thực sự giúp ích trên cảm biến ICS.
- Ensemble **không phải lúc nào cũng tốt hơn**: ở CV4.1 nó gần như vô ích, ở CV4.2 một Random Forest đơn lẻ còn thắng ensemble của năm thuật toán đã dò siêu tham số.
- Ở CV4.4, mô hình **yếu nhất** trên tập test chia ngẫu nhiên (phát hiện bất thường, B9) lại là mô hình **tốt nhất** trên tập held-out thật — vì nó không học từ nhãn tấn công nên không bị rò rỉ thời gian.

Mỗi notebook in kèm nhận xét giải thích ý nghĩa của từng con số, và kết thúc bằng phần câu hỏi thảo luận.
