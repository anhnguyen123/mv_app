# Hướng dẫn tạo file version.xml chuẩn

## 📝 Nội dung file

File `version.xml` chỉ chứa **1 dòng** với định dạng:

```
x.x.x.x
```

Ví dụ:

```
2.0.0.5
```

> ⚠️ **Chỉ 1 số duy nhất, không khoảng trắng, không xuống dòng thừa, không ký tự đặc biệt.**

## ✅ Cách tạo file đúng (3 cách)

### Cách 1: Dùng Notepad++ (Khuyên dùng)
1. Mở Notepad++
2. Gõ nội dung: `2.0.0.5`
3. Menu **Encoding → Encode in UTF-8 without BOM**
4. **File → Save As...** → đặt tên `version.xml`

### Cách 2: Dùng PowerShell
```powershell
$utf8NoBom = New-Object System.Text.UTF8Encoding $false
[System.IO.File]::WriteAllText("version.xml", "2.0.0.5", $utf8NoBom)
```

### Cách 3: Dùng Visual Studio Code
1. Mở VS Code, tạo file mới
2. Gõ: `2.0.0.5`
3. Ấn `Ctrl+Shift+P` → gõ `Change File Encoding` → chọn **Save with Encoding → UTF-8**
4. **File → Save**

## ❌ Sai lầm thường gặp

### Sai 1: File có BOM (Byte Order Mark)
```
﻿2.0.0.5    ← ký tự ﻿ ẩn ở đầu
```
> Ký tự BOM (`EF BB BF`) làm `Convert.ToInt32` bị lỗi `FormatException`.

Kiểm tra bằng PowerShell:
```powershell
Format-Hex version.xml
# Nếu thấy "EF BB BF" ở đầu → có BOM → SAI
```

### Sai 2: File có ký tự xuống dòng thừa
```
2.0.0.5\n
```
> `.Trim()` đã xử lý được lỗi này.

### Sai 3: File có nội dung XML thay vì plain text
```xml
<?xml version="1.0"?>
<version>2.0.0.5</version>
```
> Chỉ được ghi số thuần: `2.0.0.5` — không XML tag.

### Sai 4: Upload sai encoding lên server
Dùng FileZilla hoặc FTP → chọn **Binary mode**, không dùng ASCII mode.

## 🔍 Cách kiểm tra file đã chuẩn chưa

```powershell
# Kiểm tra hex
Format-Hex version.xml

# Kết quả ĐÚNG (không BOM):
# 32 2E 30 2E 30 2E 35
# → 2  .  0  .  0  .  5

# Kết quả SAI (có BOM):
# EF BB BF 32 2E 30 2E 30 2E 35
```

## 📌 Lưu ý khi upload lên server

- File trên server phải giống hệt file local (không BOM)
- Dùng **FileZilla** → chuột phải file → **Transfer → Binary**
- Hoặc upload bằng **cPanel File Manager** → đảm bảo không tự động thêm BOM
