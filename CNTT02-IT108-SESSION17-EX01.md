### **Phần 1: Phân tích lỗi trong lập luận của Nam**

Lỗi về DNS: Nam sai hoàn toàn. Nếu DNS Lookup thất bại (không tìm thấy IP), trình duyệt sẽ báo lỗi dạng DNS\_PROBE\_FINISHED\_NXDOMAIN hoặc ERR\_NAME\_NOT\_RESOLVED, chứ không bao giờ trả về 500 Internal Server Error.

Lỗi về mã trạng thái 500: Mã 500 thuộc nhóm Server Error (lỗi từ phía máy chủ, ví dụ: lỗi code backend, sập cơ sở dữ liệu, thiếu quyền truy cập tệp). Điều này chứng minh rằng DNS đã phân giải thành công và máy tính của Nam đã kết nối thành công đến Server của trường. Vấn đề nằm ở server đang gặp sự cố xử lý, hoàn toàn không phải do mạng nhà hay DNS hỏng.

### **Phần 2: Luồng dữ liệu (Từ DNS đến Server Response)**

DNS Lookup: Máy tính gửi truy vấn đến DNS Server $\rightarrow$ Thành công nhận lại địa chỉ IP của lms.ptit.edu.vn.

TCP Handshake & TLS: Thiết lập kết nối thành công với Server của trường qua IP nhận được.

HTTP Request: Trình duyệt gửi gói tin HTTP Request lên Server.

Server Processing: Server nhận request và tiến hành xử lý (đọc database, chạy logic). Tại đây xảy ra ngoại lệ/lỗi nghiêm trọng (ví dụ: mất kết nối DB).

Server Response: Server không thể trả về trang web bình thường (200 OK) mà sinh ra lỗi hệ thống, trả về mã trạng thái 500 Internal Server Error cho trình duyệt của Nam.

