# LLM_Detect_and_Prevent
# 🛡️ LLM Security Risk Detector: Fine-tuned Model Evaluation

Dự án này tập trung vào việc đánh giá hiệu năng của một mô hình Ngôn ngữ lớn (LLM) đã được Fine-tune để phân loại các truy vấn hệ thống (queries) nhằm phát hiện các rủi ro an toàn thông tin.

Được xây dựng với tư duy thực tế của System Administration (Sysadmin) và Security, dự án không chỉ dừng lại ở việc tính toán độ chính xác (Accuracy) đơn thuần mà tập trung giải quyết bài toán cốt lõi trong thực tế: **Mất cân bằng dữ liệu (Data Imbalance)**.

## ✨ Điểm nổi bật (Key Features)

* **Stratified Sampling (Lấy mẫu phân tầng):** Tự động điều chỉnh tỷ lệ dữ liệu test (Ví dụ: 60% Benign / 40% Malicious) để đảm bảo mô hình luôn đối mặt với các cuộc tấn công hiếm.
* **Balanced Metrics Focus:** Sử dụng `Balanced Accuracy` và phân tích tỷ lệ `Recall` để tránh tình trạng "báo cáo ảo" khi mô hình chỉ học vẹt lớp đa số.
* **Professional Visualizations:** Tự động sinh các biểu đồ chuẩn báo cáo khoa học (Normalized Confusion Matrix, Per-class Metrics Comparison) để dễ dàng thuyết minh trước đội ngũ kỹ thuật/quản lý.
* **Sysadmin-Friendly:** Script được viết theo dạng CLI, dễ dàng tích hợp vào các pipeline CI/CD hoặc các cronjob giám sát log hệ thống.

## 📂 Cấu trúc thư mục (Project Structure)

\`\`\`text
.
├── evaluate_finetune_model_fixed.py    # Main script: Xử lý dữ liệu, dự đoán và lưu kết quả
├── create_classification_report_visual.py # Script sinh biểu đồ trực quan từ kết quả
├── data_train.jsonl                    # (Input) Dữ liệu huấn luyện/kiểm thử gốc
├── stratified_evaluation_results.json  # (Output) File log chứa kết quả dự đoán chi tiết
├── visual_1_confusion_matrix.png       # (Output) Ảnh Ma trận nhầm lẫn
└── visual_2_metrics_report.png         # (Output) Ảnh so sánh các chỉ số Precision/Recall/F1
\`\`\`

## 🚀 Hướng dẫn sử dụng (Installation & Usage)

### 1. Yêu cầu hệ thống (Prerequisites)
Đảm bảo bạn đã cài đặt Python 3.8+ và các thư viện cần thiết. Chạy lệnh sau để cài đặt:

\`\`\`bash
pip install pandas numpy scikit-learn matplotlib seaborn
\`\`\`
*(Lưu ý: Bạn cần có model LLM (Ollama) đang chạy ở local với tên `detector-risk` hoặc cập nhật lại tên model trong file test_ollama_direct.py)*

### 2. Chạy kịch bản đánh giá mô hình
Script này sẽ thực hiện lấy mẫu (sampling), gửi query tới LLM, và tính toán các chỉ số.

\`\`\`bash
python evaluate_finetune_model_fixed.py
\`\`\`
*Kết quả:* Script sẽ in ra màn hình cảnh báo về mất cân bằng dữ liệu (nếu có) và xuất ra file `stratified_evaluation_results.json`.

### 3. Sinh biểu đồ báo cáo
Sau khi đã có file JSON kết quả, chạy lệnh sau để xuất ra các biểu đồ trực quan:

\`\`\`bash
python create_classification_report_visual.py
\`\`\`
*Kết quả:* Sẽ có 2 file `.png` được tạo ra trong thư mục hiện tại để bạn đưa vào báo cáo.

## 📊 Phân tích kết quả (Evaluation Interpretation)

Dự án này phân loại traffic thành 4 nhóm chính:
1. `benign` (Gói tin sạch - Đa số)
2. `sql_injection` (Tấn công SQLi)
3. `dos_query` (Tấn công từ chối dịch vụ vào DB)
4. `privilege_escalation` (Leo thang đặc quyền)

**Tại sao chúng tôi tập trung vào Balanced Accuracy?**
Trong môi trường thực tế, số lượng gói tin `benign` có thể chiếm tới 99%. Một AI tồi nếu đoán "Tất cả đều sạch" vẫn đạt Accuracy 99% nhưng lại bỏ lọt 100% cuộc tấn công (Recall = 0). 
Dự án này sử dụng Ma trận nhầm lẫn (Confusion Matrix) và F1-Score để đo lường độ nhạy thực sự của AI, đảm bảo tỷ lệ bỏ lọt tội phạm (False Negative) ở mức thấp nhất.

## 🤝 Đóng góp (Contributing)
Mọi đóng góp nhằm tối ưu hóa prompt hoặc cải thiện script trực quan hóa (visualization) đều được hoan nghênh. Vui lòng mở Issue hoặc Pull Request.
