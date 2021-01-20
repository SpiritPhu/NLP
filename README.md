# NLP
For saving all NLP Code

1.
########################################################
!!!Transformer!!!#######################################
########################################################

Overview:
- Transformer dùng để xử lý ngôn ngữ tự nhiên (NLP).
- Thông thường xử lý dữ liệu tuần tự (Seq2Seq): RNN, LSTM, GRU,...
 	+ Encoder: đưa vào input (E)
	+ Decoder: giải mã hóa Encoder (D)
- Transformer vẫn bao gồm 2 khối: Encoder và Decoder (Input & Output)
	+ Encoder:
	  - Self Attention: tạo ra mối liên hệ giữa các từ trong câu.
			*Positional Encoding: Do input tất cả các câu và từ song song (Khác với mạng RNN, LSTM đưa dữ liệu theo tuần tự), cần cơ chế để sắp xếp.
			*Cơ chế lưu lại vị trí từng từ trong câu mặc dù có thể chúng giống nhau
			*Dùng hàm sin pos sin tính toán ra vector p bao gồm 512 items
			*Finally, tạo được ra các vector vừa chứa Embedding vừa chứa các vị trí của câu.
			*Seft-Attention để tạo ra mối quan hệ giữa các từ trong câu bằng các nhân ma trận của từng vector với Wq,Wt,...
			*Tối ưu đang xác định được 8 W để xác định mối quan hệ
	  - Feed Forward: tổng hợp các mối quan hệ giữa các từ trong câu thành các vector z.
	+ Decoder:
	  - Đưa các vector output của Ecoder vào Decoder để sinh ra các câu Decoder
	  - Stage 1: đưa ra các masking từng từ để model dự đoán từ tiếp theo đến khi hoàn thành câu,
		     theo ma trận đường chéo để xác định từng từ 
	  - Stage 2: Encoder-Decoder Attention 

2.
########################################################
!!!BERT PHoBERT!!!#######################################
########################################################

- Người ta sử dụng các kiến trúc mạng RNN để có thể tạo ra mối quan hệ thứ tự giữa các từ trong câu,
  từ đó tạo ra vector nhúng từ có ngữ cảnh. Tuy nhiên việc này chỉ thực hiện được theo một chiều 
  left-to-right hoặc right-to-left mà thôi. Một số mạng phức tạp hơn đã sử dụng BiLSTM để chạy dọc 
  theo câu theo 2 hướng ngược nhau nhưng 2 hướng này lại độc lập, chả liên quan gì đến nhau 
  nên có thể xem là một chiều mà thôi
- Ưu điểm vượt trội của BERT là nhúng thêm ngữ cảnh (Context) vào trong các vector embedding
- BERT chia transformer làm đôi, chỉ lấy phần Encoder bên trái và bỏ đi phần Decoder bên phải.
- Phần Decoder được BERT train đồng thời 2 task gọi là Masked LM (để dự đoán từ thiếu trong câu) 
  và Next Sentence Prediction (NSP – dự đoán câu tiếp theo câu hiện tại). 
  Hai món này được train đồng thời và loss tổng sẽ là kết hợp loss của 2 task và 
  model sẽ cố gắng minimize loss tổng này.
	* Masked LM: 
		 Train bằng cách che đi tầm 15% số từ trong câu và đưa vào model, và sẽ train để model predict ra các từ bị che đó dựa vào các từ còn lại.
	* Next Sentence Prediction (NSP): model sẽ được feed cho một cặp câu và nhiệm vụ của nó là output ra giá trị 1 nếu câu thứ hai đúng là câu đi sau câu thứ nhất và 0 nếu không phải. 
					  Trong quá trinh train, ta chọn 50% mẫu là Positive (output là 1) và 50% còn lại là Negative được ghép linh tinh (output là 0).

Elbow method

Tư tưởng chính của phương pháp phân cụm phân hoạch (như k-means) là định nghĩa 
1 cụm sao cho tổng biến thiên bình phương khoảng cách trong cụm là nhỏ nhất, 
tham số này là WSS (Within-cluster Sum of Square)
