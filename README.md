# Training-pipeline

# Pipeline Dự Đoán Doanh Số Bán Hàng Amazon

## Giới thiệu chung

Dự án này triển khai một pipeline học máy end-to-end để dự đoán số lượng hàng bán được từ dữ liệu bán hàng Amazon. Pipeline được thiết kế với tích hợp MLflow để theo dõi thí nghiệm, quản lý phiên bản mô hình và đảm bảo khả năng tái tạo kết quả.

### Các bước trong pipeline

1. **Tiền xử lý dữ liệu**
   - Kỹ thuật tạo đặc trưng từ các cột ngày tháng
   - Xử lý tự động các đặc trưng phân loại và số học
   - Phiên bản hóa tập dữ liệu bằng mã băm MD5
   - Phân chia tập huấn luyện/validation/kiểm thử với tỷ lệ có thể tùy chỉnh
   - Phát hiện nguy cơ rò rỉ dữ liệu (data leakage)

2. **Huấn luyện mô hình**
   - Tiền xử lý với scikit-learn pipelines cho việc chuẩn hóa và mã hóa đặc trưng
   - Hỗ trợ nhiều mô hình hồi quy (Linear, Ridge, Lasso, RandomForest)
   - Kiểm chứng chéo để đánh giá độ ổn định của mô hình
   - Tự động tạo biểu đồ hiệu suất
   - Trực quan hóa tầm quan trọng của đặc trưng với mô hình dựa trên cây quyết định
   - Lưu trữ mô hình checkpoint với dấu thời gian

3. **Đánh giá mô hình**
   - Tính toán các chỉ số đánh giá toàn diện (MSE, RMSE, MAE, R², MAPE)
   - Trực quan hóa giá trị thực tế so với giá trị dự đoán
   - So sánh mô hình giữa các thuật toán khác nhau

4. **Tích hợp MLflow**
   - Theo dõi thí nghiệm tự động
   - Ghi log tham số (metadata tập dữ liệu, siêu tham số)
   - Ghi log các chỉ số đánh giá (metrics huấn luyện và kiểm thử)
   - Lưu trữ các artifacts (mô hình checkpoint, biểu đồ)
   - Đăng ký mô hình để quản lý phiên bản
   - Giao diện người dùng tương tác để so sánh thí nghiệm

### Điểm mới, sáng tạo

- **Phát hiện rò rỉ dữ liệu tự động**: Pipeline cảnh báo về nguy cơ rò rỉ dữ liệu khi phát hiện các đặc trưng có thể phụ thuộc trực tiếp vào biến mục tiêu
- **Phiên bản hóa tập dữ liệu**: Tạo mã nhận dạng dữ liệu bằng mã băm MD5 để theo dõi thay đổi trong dữ liệu nguồn
- **Trực quan hóa toàn diện**: Tự động tạo biểu đồ hiệu suất và biểu đồ tầm quan trọng của đặc trưng
- **Quản lý mô hình tích hợp**: Tất cả mô hình được tự động đăng ký với MLflow để dễ dàng triển khai

## Công nghệ sử dụng

- **Python 3.11+**: Ngôn ngữ lập trình cốt lõi
- **MLflow 2.8.0**: Theo dõi thí nghiệm và đăng ký mô hình
- **scikit-learn 1.3.0**: Cho thuật toán ML và tiền xử lý
- **pandas 2.0.3**: Xử lý dữ liệu
- **NumPy 1.24.3**: Tính toán số học
- **Matplotlib 3.7.1**: Trực quan hóa
- **joblib 1.3.2**: Lưu trữ mô hình
- **Google Colab**: Môi trường thực thi trên đám mây
- **ngrok**: Cho phép truy cập MLflow UI từ Colab

### Đặc điểm nổi bật

- **Tự động hóa cao**: Toàn bộ quy trình từ tiền xử lý đến đánh giá được thực hiện tự động
- **Tích hợp MLflow sâu**: Mọi thí nghiệm đều được theo dõi và so sánh một cách trực quan
- **Thân thiện với người dùng**: Thiết kế để dễ dàng sử dụng mà không cần hiểu biết sâu về MLflow
- **Khả năng mở rộng**: Dễ dàng thêm mô hình mới hoặc các bước tiền xử lý mới

## Hướng dẫn cài đặt môi trường và chạy code

### Yêu cầu tiên quyết

- Python 3.11 trở lên
- Tài khoản Google (để sử dụng Colab)

### Thiết lập môi trường

1. Clone repository này:
```bash
git clone https://github.com/yourusername/amazon-sales-prediction.git
cd amazon-sales-prediction
```

2. Tạo và kích hoạt môi trường ảo:
```bash
python -m venv venv
source venv/bin/activate  # Trên Windows: venv\Scripts\activate
```

3. Cài đặt các gói thư viện cần thiết:
```bash
pip install mlflow==2.8.0 pandas==2.0.3 numpy==1.24.3 scikit-learn==1.3.0 matplotlib==3.7.1 joblib==1.3.2 pyngrok==6.0.0
```

### Chạy Pipeline

#### Cách 1: Chạy trên máy cục bộ

1. Đặt file CSV dữ liệu vào thư mục thích hợp
2. Cập nhật biến `data_path` trong script
3. Chạy pipeline huấn luyện:
```bash
python amazon_sales_pipeline.py
```
4. Khởi động MLflow UI:
```bash
mlflow ui
```
5. Mở trình duyệt và truy cập `http://localhost:5000`

#### Cách 2: Chạy trên Google Colab

1. Tải notebook lên Google Colab
2. Mount Google Drive:
```python
from google.colab import drive
drive.mount('/content/drive')
```
3. Tải dữ liệu lên Google Drive
4. Thiết lập biến `data_path` trỏ đến vị trí file dữ liệu của bạn
5. Thực thi các cell trong notebook
6. Để truy cập MLflow UI, sử dụng URL từ ngrok tunnel được cung cấp bởi code

## Yêu cầu dữ liệu

Pipeline này cần một file CSV với các cột sau:
- `Order ID`: Định danh duy nhất cho mỗi đơn hàng
- `Customer Name`: Tên khách hàng
- `Date`: Ngày mua hàng theo định dạng DD-MM-YY
- `Quantity`: Biến mục tiêu - số lượng hàng đã mua
- Các đặc trưng bổ sung như danh mục sản phẩm, giá cả, v.v.

## Video demo
