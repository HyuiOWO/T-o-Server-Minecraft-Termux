# 🎮 Hướng dẫn cài đặt Minecraft Paper Server trên Termux

Hướng dẫn chi tiết cách thiết lập Minecraft Paper Server trên thiết bị Android sử dụng Termux.

![Minecraft](https://img.shields.io/badge/Minecraft-Server-blue)
![PaperMC](https://img.shields.io/badge/PaperMC-Latest-orange)
![Termux](https://img.shields.io/badge/Termux-supported-brightgreen)

## 📋 Điều kiện tiên quyết

· 📱 Thiết bị Android chạy phiên bản 7.0 trở lên
· 💾 Dung lượng trống ít nhất 2GB
· 📶 Kết nối internet ổn định
· 🔋 Thiết bị được sạc pin (khuyến nghị)
· ☕ Java 17 hoặc cao hơn

🔧 CÀI ĐẶT THỦ CÔNG

Bước 1: Cập nhật và cài đặt package

```bash
pkg update && pkg upgrade -y
pkg install openjdk-17 wget nano tar -y
```

Bước 2: Tạo thư mục server

```bash
mkdir ~/minecraft-server
cd ~/minecraft-server
```

Bước 3: Kiểm tra phiên bản PaperMC mới nhất

Truy cập trang web để xem phiên bản mới nhất:

· https://papermc.io/downloads
· Hoặc kiểm tra API: https://api.papermc.io/v2/projects/paper

Ví dụ các phiên bản hiện tại (cập nhật tháng 1/2024):

Minecraft 1.21+ (Mới nhất)

```bash
# Ví dụ cho 1.21.1 - KIỂM TRA LẠI SỐ BUILD TRÊN WEBSITE
wget https://api.papermc.io/v2/projects/paper/versions/1.21.1/builds/107/downloads/paper-1.21.1-107.jar -O paper.jar
```

Minecraft 1.20.6

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.20.6/builds/165/downloads/paper-1.20.6-165.jar -O paper.jar
```

Minecraft 1.20.4

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/445/downloads/paper-1.20.4-445.jar -O paper.jar
```

Minecraft 1.20.1

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.20.1/builds/196/downloads/paper-1.20.1-196.jar -O paper.jar
```

Minecraft 1.19.4

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.19.4/builds/550/downloads/paper-1.19.4-550.jar -O paper.jar
```

Bước 4: Khởi chạy server lần đầu

```bash
java -jar paper.jar
```

Server sẽ tự động tạo các file cấu hình và tắt sau vài giây.

Bước 5: Chấp nhận EULA

Mở file eula.txt:

```bash
nano eula.txt
```

Sửa dòng:

```ini
eula=false
```

thành:

```ini
eula=true
```

Lưu file: Ctrl + O → Enter → Ctrl + X

Bước 6: Cấu hình server cơ bản

Mở file server.properties:

```bash
nano server.properties
```

Các cài đặt quan trọng:

```properties
server-port=25565
online-mode=true
view-distance=6
simulation-distance=4
max-players=5
motd=Termux Minecraft Server
difficulty=normal
level-type=default
```

Bước 7: Tạo script khởi động

Tạo file start.sh:

```bash
nano start.sh
```

Thêm nội dung:

```bash
#!/bin/bash

echo "=========================================="
echo "🎮 Minecraft Paper Server Starter"
echo "=========================================="

cd ~/minecraft-server

# Kiểm tra file jar
if [ ! -f "paper.jar" ]; then
    echo "❌ Lỗi: Không tìm thấy paper.jar"
    echo "📍 Hãy chắc chắn bạn đã tải paper.jar vào thư mục ~/minecraft-server/"
    exit 1
fi

# Thông tin server
echo "📂 Thư mục: $(pwd)"
echo "💾 Bộ nhớ: -Xmx1G -Xms512M"
echo "🚀 Đang khởi động server..."

# Khởi động server
java -Xmx1G -Xms512M -jar paper.jar nogui

echo "=========================================="
echo "❌ Server đã dừng!"
echo "=========================================="
```

Cấp quyền thực thi:

```bash
chmod +x start.sh
```

🚀 KHỞI ĐỘNG SERVER

```bash
cd ~/minecraft-server
./start.sh
```

⚙️ CẤU HÌNH NÂNG CAO

Tối ưu hóa cho Termux

Tạo file server.sh:

```bash
nano server.sh
```

```bash
#!/bin/bash

# Cấu hình Java
JAVA_OPTS="-Xmx1G -Xms512M -XX:+UseG1GC -XX:MaxGCPauseMillis=50"

# Thư mục server
SERVER_DIR="~/minecraft-server"
cd $SERVER_DIR

echo "🎯 Minecraft Paper Server - Termux Optimized"
echo "💻 Java Options: $JAVA_OPTS"
echo "📦 Server JAR: $(ls paper.jar)"
echo "⏰ $(date)"

# Kiểm tra eula
if [ ! -f "eula.txt" ] || grep -q "eula=false" "eula.txt"; then
    echo "⚠️  Chưa chấp nhận EULA! Chỉnh sửa eula.txt thành eula=true"
    exit 1
fi

echo "✅ Khởi động server..."
java $JAVA_OPTS -jar paper.jar nogui
```

```bash
chmod +x server.sh
```

Cấu hình bộ nhớ

Tuỳ theo RAM của thiết bị:

Thiết bị 4GB RAM:

```bash
JAVA_OPTS="-Xmx2G -Xms1G"
```

Thiết bị 3GB RAM:

```bash
JAVA_OPTS="-Xmx1G -Xms512M"
```

Thiết bị 2GB RAM:

```bash
JAVA_OPTS="-Xmx768M -Xms256M"
```

🛠️ QUẢN LÝ SERVER

Tạo script backup

```bash
nano backup.sh
```

```bash
#!/bin/bash
BACKUP_DIR="/sdcard/minecraft-backups"
SERVER_DIR="~/minecraft-server"
DATE=$(date +%Y%m%d-%H%M%S)

echo "📦 Tạo backup server..."

mkdir -p $BACKUP_DIR

cd $SERVER_DIR
tar -czf $BACKUP_DIR/backup-$DATE.tar.gz \
    world/ \
    world_nether/ \
    world_the_end/ \
    server.properties \
    ops.json \
    whitelist.json

echo "✅ Backup hoàn thành: backup-$DATE.tar.gz"
echo "📍 Vị trí: $BACKUP_DIR/"
```

```bash
chmod +x backup.sh
```

Quản lý qua TMUX

```bash
pkg install tmux -y

# Tạo session mới
tmux new-session -d -s minecraft 'cd ~/minecraft-server && ./start.sh'

# Xem session
tmux attach-session -t minecraft

# Thoát session (giữ server chạy): Ctrl + B → D
```

📋 LỆNH QUAN TRỌNG TRONG GAME

Quản trị server

```mc
/stop                    # Dừng server an toàn
/save-all               # Lưu thế giới ngay lập tức
/op <player>            # Cấp quyền operator
/whitelist add <player> # Thêm vào whitelist
```

Thông tin server

```mc
/list                   # Hiện player đang online
/tps                    # Kiểm tra hiệu suất
/gamemode creative <player>
```

❌ XỬ LÝ SỰ CỐ

Lỗi "Out of memory"

Giảm RAM trong start.sh:

```bash
java -Xmx512M -Xms256M -jar paper.jar nogui
```

Lỗi port đã sử dụng

Đổi port trong server.properties:

```properties
server-port=25566
```

Lỗi Java version

Kiểm tra Java version:

```bash
java -version
```

Cài đặt Java 17 nếu cần:

```bash
pkg install openjdk-17 -y
```

Lỗi kết nối

· Kiểm tra WiFi/Internet
· Đảm bảo port không bị chặn
· Kiểm tra IP server: ifconfig hoặc ip addr

🔒 BẢO MẬT

Bật whitelist

```bash
nano server.properties
```

```properties
white-list=true
enforce-whitelist=true
```

Thêm player vào whitelist

Trong game:

```mc
/whitelist add <username>
```

Hoặc chỉnh sửa file:

```bash
nano whitelist.json
```

📊 KIỂM TRA HIỆU SUẤT

Xem log server

```bash
tail -f ~/minecraft-server/logs/latest.log
```

Kiểm tra tài nguyên

```bash
# Kiểm tra RAM
free -h

# Kiểm tra CPU và process
top

# Kiểm tra dung lượng
df -h ~/
```

💾 CÀI ĐẶT PLUGIN

1. Tải plugin .jar vào thư mục plugins/
2. Tạo thư mục nếu chưa có: mkdir plugins
3. Khởi động lại server

📝 GHI CHÚ QUAN TRỌNG

· ⚠️ Luôn kiểm tra phiên bản mới nhất trên https://papermc.io/
· 🔄 Backup thường xuyên trước khi cập nhật
· 📱 Giảm view-distance nếu server lag
· 🔋 Cắm sạc khi chạy server lâu
· 🌡️ Theo dõi nhiệt độ thiết bị

---

🎯 Lưu ý: Các URL phiên bản có thể thay đổi. Luôn truy cập https://papermc.io/downloads để lấy link chính xác nhất!

Chúc bạn tạo server thành công! 🎮✨