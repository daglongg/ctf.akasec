# Th3Hun7 1

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/beaa32fb-6c51-4d0e-a505-7863ef9b5be9)

Ở đây tôi sẽ đọc hint mà tác giả đã cho. Có vẻ trang duy nhất của giải này.

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/81ee84f7-f4e2-4d3e-a7b0-d6afbde563c4)

Mình đã tìm kiếm với từ khóa `akasec` thì kết quả dãn tôi đến trang web có vẻ là trang chủ chính. Nhưng tôi nghĩ đây kphai là hướng đúng. Thử thay đổi từ khóa thành `akasec.club` và tôi có 1 trang có vẻ là web của giải lần này `https://www.akasec.club/`

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/83c29d4e-8f2d-48e1-9330-b000e3733ecf)

Thử mở với mã nguồn của trang web hiện tại và tôi để ý tới dòng thứ 54. 

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/e8da75db-91d6-4207-b93c-bfc947e0f904)

Ở đây ta thấy thuộc tính src chứa đường dẫn tới tệp hình ảnh. Trong trường hợp này, nó trỏ đến một tệp JPEG được lưu trữ trong thư mục tĩnh của ứng dụng Next.js. Thử pass theo đường link đó và tôi có flag đầu tiên

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/4ab4b2c5-18e1-40c4-9a0c-1a7fb6811f0c)

Flag: `AKASEC{Wh4T_a_PfP_:3}`

# Th3Hun7 2

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/013b7eb2-22d4-43c0-9722-35279e8f683d)

Tiếp tục theo luồng trên tôi thấy ở dòng 63 có đường link github là `https://github.com/l3rnds`

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/a34500ca-6504-4bb7-917e-ba3f8e2dc0db)





