# BiLSTM-Attention Neural Machine Translation

## Giới thiệu

Dự án này triển khai một mô hình dịch máy (Neural Machine Translation) sử dụng kiến trúc BiLSTM (Bidirectional Long Short-Term Memory) kết hợp với cơ chế Attention để dịch văn bản từ tiếng Anh sang tiếng Việt. Mô hình này được thiết kế để hiểu và bảo toàn ngữ cảnh của cả câu trong quá trình dịch, giúp nâng cao chất lượng bản dịch.

## Cấu trúc mô hình

Mô hình dịch máy này được xây dựng với kiến trúc Encoder-Decoder:

### Encoder
- **Input**: Chuỗi từ tiếng Anh được mã hóa
- **Embedding Layer**: Chuyển đổi các từ thành các vector nhúng (embedding vectors)
- **BiLSTM Layer**: Xử lý chuỗi theo cả hai chiều (tiến và lùi) để nắm bắt ngữ cảnh đầy đủ
- **Output**: Trạng thái ẩn (hidden states) và trạng thái tế bào (cell states) cho Decoder

### Decoder
- **Input**: Chuỗi từ tiếng Việt được mã hóa (bắt đầu với token `<START>`)
- **Embedding Layer**: Tương tự như Encoder
- **LSTM Layer**: Tạo ra chuỗi đầu ra từng bước một
- **Attention Layer**: Cơ chế MultiHead Attention giúp tập trung vào các phần quan trọng của văn bản nguồn
- **Output Layer**: Dự đoán từ tiếp theo trong chuỗi đích

## Đặc điểm kỹ thuật

- **Embedding Dimension**: 64
- **LSTM Units**: 256
- **Dropout Rate**: 0.2 (để giảm hiện tượng overfitting)
- **Optimizer**: RMSprop
- **Loss Function**: Sparse Categorical Crossentropy
- **Batch Size**: 128
- **Maximum Epochs**: 150 (với Early Stopping)

## Dữ liệu

Mô hình được huấn luyện trên tập dữ liệu song ngữ Anh-Việt:
- **Phân chia dữ liệu**:
  - Training: 90% của tập dữ liệu (với 10% dùng làm validation)
  - Testing: 10% còn lại

Các chuỗi đầu vào và đầu ra được tiền xử lý bằng:
- Tokenization (phân tách từ)
- Chuyển đổi thành dãy số (sequences)
- Padding để đảm bảo độ dài đồng nhất

## Quá trình Inference

Quá trình dịch (inference) bao gồm các bước:
1. Mã hóa câu tiếng Anh bằng Encoder
2. Khởi tạo đầu vào của Decoder với token `<START>`
3. Sử dụng Decoder để dự đoán từng từ tiếp theo
4. Kết thúc quá trình khi gặp token `<END>` hoặc đạt độ dài tối đa

## Cài đặt và sử dụng

### Yêu cầu

```
tensorflow>=2.0.0
numpy
pandas
matplotlib
```

### Sử dụng mô hình

```python
# Tải mô hình đã được huấn luyện
encoder_model = load_model("models/BiLSTM/encoder_model.h5")
decoder_model = load_model("models/BiLSTM/decoder_model.h5")

# Dịch một câu từ tiếng Anh sang tiếng Việt
translated_text = translate("How are you today?")
print(translated_text)
```

## Hiệu suất và kết quả

Mô hình đạt được kết quả khả quan trong việc dịch các câu từ tiếng Anh sang tiếng Việt, đặc biệt đối với các câu ngắn và có cấu trúc phổ biến. Hiệu suất được đánh giá thông qua:
- Accuracy trên tập validation
- Đánh giá định tính trên tập test

## Ưu và nhược điểm

### Ưu điểm
- BiLSTM giúp nắm bắt ngữ cảnh theo cả hai chiều
- Cơ chế Attention giúp mô hình tập trung vào các phần quan trọng của câu nguồn
- Hiệu quả với các câu có độ dài vừa phải

### Nhược điểm
- Có thể gặp khó khăn với các câu rất dài
- Bộ từ vựng giới hạn có thể ảnh hưởng đến khả năng dịch các từ hiếm
- Cần nhiều tài nguyên tính toán cho quá trình huấn luyện

## Hướng phát triển

Một số cải tiến có thể áp dụng cho mô hình trong tương lai:
- Sử dụng kiến trúc Transformer thay thế cho RNN/LSTM
- Tăng kích thước bộ dữ liệu huấn luyện
- Áp dụng kỹ thuật transfer learning từ các mô hình ngôn ngữ lớn
- Thêm cơ chế xử lý từ chưa biết (unknown words)

## Tham khảo

- [Sequence to Sequence Learning with Neural Networks](https://arxiv.org/abs/1409.3215)
- [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/abs/1409.0473)
- [Effective Approaches to Attention-based Neural Machine Translation](https://arxiv.org/abs/1508.04025)