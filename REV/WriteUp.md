# Sperm Rev

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/7fa9dc32-7ccb-4897-b726-58204d5c5039)

Phân tích file 

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/43e71581-c028-4013-ac74-7641a57d8a9d)

Sử dụng IDA để phân tích tĩnh file. Ở đây ta thấy chương trình này sẽ in dòng chữ "strings me please hh por favor" ra màn hình và sau đó kết thúc

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/b6f1d181-5f94-4611-8c63-062e00196e6e)

Sử dung F12 và ta có flag là `AKASEC{strings_b35t_t00l_1n_r3v3r5e_eng1n33r1ng}`

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/2433ab7f-8145-4f25-a105-daa6277ac79c)

# Grip

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/56d5f810-0f35-4db1-9890-b300d8a887ca)

Phân tích file 

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/74b67604-d4ee-4f96-8e26-d861c6dd969f)

Sử dụng IDA để phân tích tĩnh file. 

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/3ebaa6f7-f071-4097-a175-5da72946d305)

Ở đây khi phân tích độn tôi phát hiện có antidbg làm chương trình dừng đột ngột. Ngoài ra logic của đoạn code này chỉ là 
- Đọc giá trị từ fs:[0x28].
- Khai báo các biến số nguyên 64-bit với các giá trị đã mã hóa.
- XOR từng byte của các biến này với 0xF2.
- Ánh xạ một vùng bộ nhớ với kích thước 0x8D bytes cho phép đọc/ghi và vô danh.
- Sao chép các giá trị đã được XOR vào vùng bộ nhớ được ánh xạ.
- Thực thi vùng bộ nhớ được ánh xạ như một hàm.
- In ra chuỗi ":3".
- Kết thúc chương trình với mã trả về 0.

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/92a8a491-fb2b-4e34-989b-8a74fd04e7fe)

Đặt các bp để vượt qua antidbg ta có flag là `akasec{sh1tty_p4ck3d_b1n4ry}`

# Paranoia

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/8e5f0ea8-5ef5-4ffd-a37a-baea40594374)

Sử dụng IDA để phân tích tĩnh file. 

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/d3b76393-26f8-48a3-b198-167d1788d6c1)

Logic rất đơn giản khi 
- Chương trình khởi tạo bộ sinh số ngẫu nhiên sử dụng thời gian hiện tại.
- Nó duyệt qua từng ký tự của chuỗi flag, XOR ký tự đó với một số ngẫu nhiên để làm mờ nó.
- Các giá trị đã làm mờ được in ra dưới dạng các số nguyên
- Phải giữ lại 8 bit thấp nhất của giá trị v5 bắng cách & 0xFF

![image](https://github.com/daglongg/ctf.akasec/assets/138242812/be428fe8-3926-4dde-b210-b996ab5d9fc7)

Tôi đã viết 1 scirp để bruce fore seed ấy

```
import ctypes
from pwn import remote
from Crypto.Util.number import long_to_bytes
import time

def load_libc_functions():
    libc = ctypes.cdll.LoadLibrary("libc.so.6")
    return libc.srand, libc.rand

def connect_to_server(address, port):
    return remote(address, port)

def receive_and_process_data(connection):
    data = connection.recvline().decode().strip().split()
    return list(map(int, data))

def get_current_time():
    return int(time.time())

def generate_result(seed, data, srand, rand):
    srand(seed)
    return [(value ^ (rand() & 0xFF)) for value in data]

def brute_force_seed(current_time, time_range, data, srand, rand):
    for offset in range(-time_range, time_range + 1):
        seed = current_time + offset
        result = generate_result(seed, data, srand, rand)
        flag = b''.join(long_to_bytes(value) for value in result)
        
        try:
            decoded_flag = flag.decode()
            if all(32 <= ord(char) < 127 for char in decoded_flag):
                return seed, decoded_flag
        except UnicodeDecodeError:
            continue
    return None, None

def main():
    srand, rand = load_libc_functions()
    connection = connect_to_server('20.80.240.190', 1234)
    data = receive_and_process_data(connection)
    current_time = get_current_time()
    time_range = 60

    seed, flag = brute_force_seed(current_time, time_range, data, srand, rand)
    if seed is not None:
        print(f"Seed used: {seed}")
        print(f"Flag: {flag}")
    else:
        print("Could not find the correct seed within the specified range.")

if __name__ == "__main__":
    main()
```




