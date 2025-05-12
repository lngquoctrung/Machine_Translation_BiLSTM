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

## Kết quả

```shell
Context: and i want to talk to you about that because even more than giving is the capacity for us to do
something smarter together for the greater good that lifts us both up and that can scale
True Target: <START> và tôi muốn nói với bạn về điều đó bởi_vì thậm_chí nhiều hơn là cho đi là khả_năng chúng_ta
cùng nhau làm điều gì đó thông_minh hơn vì lợi_ích lớn hơn nâng cả hai chúng_ta lên và điều đó
có_thể mở_rộng  <END>
Predict: và tôi muốn nói với bạn về điều đó bởi_vì thậm_chí còn tạo ra là khả_năng để chúng_ta làm điều gì đó
thông_minh hơn cho đến điều tốt_đẹp hơn nâng cao hơn nâng hết con_người này lên và có_thể mở_rộng
quy_mô  <end>
========================================================================================================================
Context: that's why i'm sitting here
True Target: <START> đó là lý_do tại_sao tôi ngồi đây  <END>
Predict: thật là lý_do tại_sao tôi đang ngồi đây  <end>
========================================================================================================================
Context: but i also want to point something else out each one of you is better than anybody else at something
True Target: <START> nhưng tôi cũng muốn chỉ ra một điều khác mỗi người trong số các bạn đều giỏi hơn bất_kỳ ai
khác ở một lĩnh_vực nào đó  <END>
Predict: nhưng tôi cũng muốn điểm một cái khác mỗi người trong số các bạn giỏi hơn bất_kỳ ai khác vào một
điều gì đó  <end>
========================================================================================================================
Context: that disproves that popular notion that if you're the smartest person in the room you're in the
wrong room
True Target: <START> điều đó bác_bỏ quan_niệm phổ_biến rằng nếu bạn là người thông_minh nhất trong phòng thì bạn
đang ở nhầm phòng  <END>
Predict: điều đó có quan_điểm phổ_biến rằng nếu bạn là người thông_minh nhất trong phòng bạn đang ở trong
phòng sai thì không  <end>
========================================================================================================================
Context: so let me tell you about a hollywood party i went to a couple years back and i met this up and
coming actress and we were soon talking about something that we both felt passionately about public
art
True Target: <START> vì_vậy hãy để tôi kể cho bạn nghe về một bữa tiệc ở hollywood mà tôi đã tham_dự vài năm
trước và tôi đã gặp nữ diễn_viên đang lên này và chúng_tôi đã sớm nói về điều mà cả hai đều cảm_thấy
say_mê nghệ_thuật công_cộng  <END>
Predict: vì_vậy hãy để tôi kể cho bạn nghe về một bữa tiệc hollywood tôi đã đi sâu vào cách này và những
người này lên và những người này đang sớm nói về điều gì đó mà cả chúng_ta đều cảm_thấy say_mê với
nghệ_thuật công_cộng  <end>
========================================================================================================================
Context: and she had the fervent belief that every new building in los angeles should have public art in it
True Target: <START> và cô có niềm tin nhiệt_thành rằng mọi tòa nhà mới ở los_angeles nên có nghệ_thuật công_cộng
trong đó  <END>
Predict: và cô ấy đã tin rằng mọi công_trình niềm tin mới ở los_angeles đặc_biệt có những nghệ_thuật
công_cộng vào đó  <end>
========================================================================================================================
Context: she wanted a regulation for it and she fervently started — who is here from chicago
True Target: <START> cô ấy muốn có một quy_định cho nó và cô ấy nhiệt_thành bắt_đầu ai đến từ chicago  <END>
Predict: cô ấy muốn có quy_tắc cho nó và cô ấy đến mức bắt_đầu người ở đây từ chicago  <end>
========================================================================================================================
Context: — she fervently started talking about these bean shaped reflective sculptures in millennium park and
people would walk up to it and they'd smile in the reflection of it and they'd take selfies together
and they'd laugh
True Target: <START> cô ấy bắt_đầu nhiệt_tình nói về những tác_phẩm điêu_khắc phản_chiếu hình hạt đậu này ở
công_viên thiên_niên_kỷ và mọi người sẽ đi đến đó và họ sẽ mỉm cười khi phản_chiếu nó và họ sẽ chụp
ảnh tự sướng cùng nhau và họ sẽ cười  <END>
Predict: cô ấy thích đi nói về những tác_phẩm điêu_khắc bằng các cái điêu_khắc các tác_phẩm điêu_khắc các
tác_phẩm điêu_khắc trong thiên_niên_kỷ thiên_niên_kỷ và mọi người sẽ bước lên tới nó và họ sẽ mỉm
cười trước đó và họ sẽ thích nhau và cười nhau và họ cười  <end>
========================================================================================================================
Context: and as she was talking a thought came to my mind
True Target: <START> và khi cô ấy đang nói một ý_nghĩ nảy ra trong đầu tôi  <END>
Predict: và khi cô ấy đang nói_chuyện một suy_nghĩ đã đến được trong khi nào  <end>
========================================================================================================================
Context: i said i know someone you ought to meet
True Target: <START> tôi nói tôi biết một người mà bạn nên gặp  <END>
Predict: tôi nói tôi biết ai đó bạn phải gặp nhau  <end>
========================================================================================================================
```

## Tham khảo

- [Sequence to Sequence Learning with Neural Networks](https://arxiv.org/abs/1409.3215)
- [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/abs/1409.0473)
- [Effective Approaches to Attention-based Neural Machine Translation](https://arxiv.org/abs/1508.04025)