# Mô hình LSTM
Mô hình Long-Short Term Memory (LSTM) được giới thiệu vào năm 1997 là mô hình cải tiến của mô hình RNN truyền thống, sự thành công của mô hình LSTM là khác phục được vấn đề vanishing gradient trong mô hình RNN mắc phải. Về kiến trúc chung thì mô hình LSTM vẫn dự vào kiến trúc của của mô hình RNN, nhưng điểm khác biệt giữa hai mô hình là trạng thái hidden của mô hình LSTM sẽ thêm 1 trạng thái là gọi là state c.
![LSTM cell](https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/LSTM_Cell.svg/450px-LSTM_Cell.svg.png)

# Bidirectional LSTM
Thay vì chỉ vận hành một LSTM chạy từ kí tự đầu đến cuối, ta sẽ khởi tạo một LSTM nữa chạy từ kí tự cuối lên đầu. Bidirectional Long Short-Term Memory sẽ thêm một tầng ẩn cho phép xử lý dữ liệu theo chiều ngược lại một cách linh hoạt hơn so với LSTM truyền thống. Mô tả cấu trúc của mạng nơ-ron hồi tiếp hai chiều với một tầng ẩn.
![Kiến trúc BiLSTM](https://d2l.aivivn.com/_images/birnn.svg)

