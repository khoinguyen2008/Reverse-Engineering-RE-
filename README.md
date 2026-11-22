# Reverse-Engineering-RE

# Lời nói đầu
Đây là bài nghiên cứu đầu tiên của mình về RE trong cyber security có gì sai sót mong mọi người góp ý

**I.** Tìm hiểu về ngôn ngữ Python, cách tạo ra file PE từ file C#.
1. Python là gì

Python là ngôn ngữ lập trình bậc cao, được giới thiệu lần đầu vào năm 1991 bởi Guido van Rossum. Python nổi bật nhờ cú pháp đơn giản, dễ đọc và có thể sử dụng trong nhiều lĩnh vực khác nhau. Vì thân thiện và mạnh mẽ, Python nhanh chóng trở thành ngôn ngữ phổ biến nhất thế giới theo nhiều bảng xếp hạng công nghệ.và có nhiều ứng dụng trong công nghệ thông tin như
* xây dựng web
* Phân tích dữ liệu, trực quan hóa dữ liệu
* Tạo mô hình Machine Learning, Deep Learning
* Viết script tự động hóa
* Viết ứng dụng desktop, IoT, công cụ bảo mật,…
Python chạy tốt trên Windows, Linux, macOS và các thiết bị nhúng như Raspberry Pi, giúp lập trình viên linh hoạt trong quá trình phát triển sản phẩm.
Python có thể kết hợp với Java, C/C++, .NET và nhiều nền tảng khác. Điều này làm Python dễ dàng tích hợp vào hệ thống lớn và những dự án yêu cầu hiệu suất cao.
2. file PE là gì
PE (Portable Executable) là định dạng tệp nhị phân được sử dụng chủ yếu trong hệ điều hành Windows cho các tệp thực thi (EXE), thư viện liên kết động (DLL), và các loại file chạy khác. Định dạng này mô tả cấu trúc cần thiết để hệ điều hành có thể tải và thực thi chương trình, bao gồm thông tin về mã máy, dữ liệu, thư viện cần thiết, và thông tin định vị các thành phần bên trong tệp.
Định dạng PE được xây dựng dựa trên định dạng COFF (Common Object File Format) và cũng là nền tảng cho định dạng thực thi trong `.NET`
3. C# là gì
`C# (C-Sharp)`là một ngôn ngữ lập trình hướng đối tượng và chạy trong khuôn khổ nền tảng `.NET`.
4. Net platform là gì
`.Net Flatform` là một nền tảng mã nguồn mở hỗ trợ các ngôn ngữ như: `C#`, `F#` `VB.NET` dùng để phát triển phần mềm chạy trên đa nền tảng.
5. Các bước tạo ra file PE từ C#
<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/84fdfc64-21a1-4b80-9cea-f179ce0a338f" />
6. Demo
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0d799def-f09a-4b39-b524-a209f0a75ccf" />
mình đã chuẩn bị một môi trường demo và khi ấn chạy chương trình `Program.cs` thì terminal đã hiện ra chừ `hello word`
khi mình bấm chạy thì file `C-sharp.dll` đã có cập nhật mới và đó là cách tạo ra file PE từ C#
<img width="1227" height="658" alt="image" src="https://github.com/user-attachments/assets/e98fbccf-c6e7-40c5-9712-c1e3e2ce1561" />

**II.** Cách reverse file C# PE
cái này mình sẽ dùng công cụ `ilspy-vscode` của vscode và có một đoạn code demo do AI viết chừ mình không biết viết C#
code demo:
```
using System;

Console.WriteLine("=== HE THONG KIEM TRA BAN QUYEN ===");
Console.Write("Nhap License Key de kich hoat: ");
string? userInput = Console.ReadLine();

if (!string.IsNullOrEmpty(userInput) && CheckLicenseKey(userInput))
{
    Console.ForegroundColor = ConsoleColor.Green;
    Console.WriteLine("\n[+] Chuc mung! Ban da kich hoat phan mem thanh cong.");
}
else
{
    Console.ForegroundColor = ConsoleColor.Red;
    Console.WriteLine("\n[-] Sai License Key! Vui long thu lai.");
}

Console.ResetColor();

// Hàm kiểm tra mật khẩu bí mật
static bool CheckLicenseKey(string input)
{
    // Mảng byte đã được mã hóa bằng phép toán: (ký_tự ^ 0x13) + 2
    int[] encryptedFlag = { 119, 104, 114, 122, 53, 108, 126, 104 };

    if (input.Length != encryptedFlag.Length)
    {
        return false;
    }

    for (int i = 0; i < input.Length; i++)
    {
        int transformed = (input[i] ^ 0x13) + 2;
        if (transformed != encryptedFlag[i])
        {
            return false;
        }
    }

    return true;
}
```
sau đó mình chạy thử code và nó ra như thế này
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f7b4f98-ae85-4fd2-b3f4-606bbf7935af" />
bây giờ mình sẽ tiến hành dịch ngược bằng tool xem mã xác thực nó là cái gì.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2f989715-26eb-45c1-adf1-9d88449b9acf" />
sau đó mình dùng ISLpy và mình vào thư mục và chọn file `C-sharp.dll`
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a8675dd0-7d3b-41ae-be77-15b75f2936bb" />
sau khi reverse bằng công cụ ILSP thì nó ra như thế này 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1c1c6a3a-632b-4e3f-a912-dbea08b4a3de" />
sau khi vào mục program thì sẽ có công thức của key
<img width="1674" height="954" alt="image" src="https://github.com/user-attachments/assets/a599ec7b-3d1d-4bed-b16a-7d8f00db3223" />
và mình viết ngay một chương trình c++ đơn giản để tìm key





