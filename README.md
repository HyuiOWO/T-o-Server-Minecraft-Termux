
🎮 Hướng dẫn cài đặt Minecraft Paper Server trên Termux

Hướng dẫn chi tiết cách thiết lập Minecraft Paper Server trên thiết bị Android sử dụng Termux.

https://img.shields.io/badge/Minecraft-1.20.4-green https://img.shields.io/badge/PaperMC-445-blue https://img.shields.io/badge/Termux-supported-brightgreen

📋 Điều kiện tiên quyết

· 📱 Thiết bị Android chạy phiên bản 7.0 trở lên
· 💾 Dung lượng trống ít nhất 2GB
· 📶 Kết nối internet ổn định
· 🔋 Thiết bị được sạc pin (khuyến nghị)

⚡ Cài đặt nhanh

1. Cài đặt Termux

Tải Termux từ:

· F-Droid
· GitHub Releases

2. Chạy lệnh cài đặt tự động

```bash
curl -sL https://raw.githubusercontent.com/your-repo/minecraft-termux/main/install.sh | bash
```

🔧 Cài đặt thủ công

Bước 1: Cập nhật hệ thống

```bash
pkg update && pkg upgrade -y
pkg install openjdk-17 wget nano tar -y
```

Bước 2: Tạo thư mục server

```bash
mkdir ~/minecraft-server
cd ~/minecraft-server
```

Bước 3: Tải PaperMC

Phiên bản mới nhất (1.20.4):

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/445/downloads/paper-1.20.4-445.jar -O paper.jar
```

Các phiên bản khác:

```bash
# Phiên bản 1.19.4
wget https://api.papermc.io/v2/projects/paper/versions/1.19.4/builds/550/downloads/paper-1.19.4-550.jar -O paper.jar

# Phiên bản 1.18.2
wget https://api.papermc.io/v2/projects/paper/versions/1.18.2/builds/388/downloads/paper-1.18.2-388.jar -O paper.jar
```

Bước 4: Khởi chạy server lần đầu

```bash
java -jar paper.jar
```

Server sẽ tự động tắt sau khi tạo file cấu hình.

Bước 5: Chấp nhận EULA

Chỉnh sửa file eula.txt:

```bash
nano eula.txt
```

Thay đổi:

```ini
eula=false
```

thành:

```ini
eula=true
```

Bước 6: Tạo script khởi động

Tạo file start.sh:

```bash
nano start.sh
```

Nội dung script:

```bash
#!/bin/bash
cd ~/minecraft-server

# Thông số server
JAVA_OPTS="-Xmx1G -Xms512M"
SERVER_JAR="paper.jar"

echo "🔄 Đang khởi động Minecraft Server..."
echo "📝 Sử dụng: java $JAVA_OPTS -jar $SERVER_JAR nogui"

# Khởi động server
java $JAVA_OPTS -jar $SERVER_JAR nogui

echo "❌ Server đã dừng!"
```

Cấp quyền thực thi:

```bash
chmod +x start.sh
```

⚙️ Cấu hình Server

Chỉnh sửa server.properties

```bash
nano server.properties
```

Các cài đặt quan trọng cho Termux:

```properties
server-port=25565
online-mode=true
view-distance=6
simulation-distance=4
max-players=5
motd=Termux Minecraft Server
```

Cấu hình tối ưu hóa

Tạo file server.sh với cấu hình nâng cao:

```bash
nano server.sh
```

```bash
#!/bin/bash

# Cấu hình Java
export JAVA_OPTS="-Xmx1G -Xms512M -XX:+UseG1GC -XX:MaxGCPauseMillis=50"

# Cấu hình server
cd ~/minecraft-server

echo "🎮 Minecraft Paper Server Starting..."
echo "💻 Memory: $JAVA_OPTS"
echo "📂 Directory: $(pwd)"

# Khởi động server
java $JAVA_OPTS -jar paper.jar nogui
```

🚀 Quản lý Server

Khởi động server

```bash
cd ~/minecraft-server
./start.sh
```

Dừng server an toàn

Trong console server, gõ:

```mc
stop
```

Backup server

Tạo script backup backup.sh:

```bash
nano backup.sh
```

```bash
#!/bin/bash
BACKUP_DIR="/sdcard/minecraft-backups"
SERVER_DIR="~/minecraft-server"
DATE=$(date +%Y%m%d-%H%M%S)

mkdir -p $BACKUP_DIR
cd $SERVER_DIR

echo "📦 Đang tạo backup..."
tar -czf $BACKUP_DIR/backup-$DATE.tar.gz world world_nether world_the_end

echo "✅ Backup hoàn thành: backup-$DATE.tar.gz"
```

📱 Quản lý nâng cao

Chạy server ở chế độ nền

Sử dụng tmux hoặc screen:

```bash
pkg install tmux -y
tmux new-session -d -s minecraft 'cd ~/minecraft-server && ./start.sh'
```

Xem log server

```bash
tail -f ~/minecraft-server/logs/latest.log
```

Cài đặt plugin

1. Tải plugin .jar vào thư mục plugins/
2. Khởi động lại server

🛠️ Xử lý sự cố

Lỗi thiếu bộ nhớ

Giảm RAM trong start.sh:

```bash
JAVA_OPTS="-Xmx512M -Xms256M"
```

Lỗi port đã được sử dụng

Đổi port trong server.properties:

```properties
server-port=25566
```

Lỗi kết nối

· Kiểm tra tường lửa
· Đảm bảo port được mở
· Kiểm tra IP address

Server chạy chậm

· Giảm view-distance và simulation-distance
· Giới hạn số lượng player
· Sử dụng plugin tối ưu hóa

🔒 Bảo mật

Bật whitelist

```properties
white-list=true
```

Thêm player vào whitelist

Trong game:

```mc
whitelist add <username>
```

Hoặc trong file whitelist.json

Đặt password OP

```bash
nano ops.json
```

📊 Monitoring

Kiểm tra tài nguyên

```bash
# Kiểm tra RAM
free -h

# Kiểm tra CPU
top

# Kiểm tra dung lượng
df -h
```

🤝 Đóng góp

Đóng góp ý kiến hoặc báo lỗi tại:

· Issues
· Discussions

📜 License

MIT License - xem file LICENSE để biết thêm chi tiết.

⚠️ Lưu ý quan trọng

· ⚡ PIN: Server tiêu tốn nhiều pin, hãy cắm sạc khi sử dụng
· 🔥 NHIỆT ĐỘ: Thiết bị có thể nóng lên, tránh để dưới ánh nắng
· 💾 DUNG LƯỢNG: Thường xuyên dọn dẹp file log và backup
· 🌐 MẠNG: Sử dụng WiFi để có kết nối ổn định

---

Chúc bạn có những giờ phút chơi Minecraft vui vẻ! 🎮✨

Nếu thấy hữu ích, hãy ⭐ star repository này!