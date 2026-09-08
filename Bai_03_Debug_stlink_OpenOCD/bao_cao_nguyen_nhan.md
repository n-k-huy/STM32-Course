1. Hiện tượng:
    OpenOCD vẫn nhận target, PC vẫn đọc đúng nhưng line code khác với mong đợi
2. Hai giả thuyết ban đầu:
    - H1: lỗi đường SWD --> target không gửi phản hồi về cho GDB
    - H2: ELF trong Flash khác với ELF của GDB mở
3. Kiểm tra và đánh giá:
    - H1: kiểm tra phản hồi SWD --> tín hiệu phản hồi về cho GDB sẽ không có, trong khi log vẫn báo phản hồi của target về cho GDB đường SWD vẫn hoạt động bình thường --> Loại
    - H2: kiểm tra SHA-256 của từng ELF cho thấy ELF trong flash khác hoàn toàn với ELF mà GDB mở. --> giả thuyết xác thực đúng
4. Nguyên nhân:
    Ghi ELF L03 cũ.
    Sửa ELF L03 mới nhưng chưa ghi
    GDB và target giữ 2 ELF riêng biệt, khi break main, PC sẽ được GDB yêu cầu gửi xuống để target chạy đến PC trong ELF của CPU mà GDB yêu cầu sau đó phản hồi lại cho GDB mapping với symbol của ELF GDB khiến cho GDB đọc sai line code.
5. Sữa lỗi:
    Mở lại ELF L03 cũ ghi và verify lại.
6. Phòng tránh:
    Trước khi thực hiện debug cần verify ELF trên GDB và Flash
    Lưu SHA-256 cho lần đầu ghi và kiểm tra lại SHA-256 sau mỗi lần thay đổi ELF
