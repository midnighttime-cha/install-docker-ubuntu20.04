# วิธีติดตั้ง docker บน Ubuntu 20.04

## เริ่มต้นการติดตั้ง
ถอนการติดตั้ง Docker ของเก่าที่อาจมีอยู่แล้ว
```bash
sudo apt remove docker docker-engine docker.io containerd runc
```
ขั้นตอนแรกต้องทำการอัพเดท ubuntu
```bash
sudo apt update
```
จากนั้นทำการติดตั้ง Repository เพื่อใช้ในการติดตั้ง Docker 
```bash
sudo apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
เพิ่ม GPG Key ของ official Docker repository
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
สร้าง Docker Repository ใน APT sources
```bash
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

ติดตั้ง Docker
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

กรณีต้องการติดตั้งแบบระบุ vsersion
```bash
# เช็คเวิอร์ชั่นทั้งหมด
apt-cache madison docker-ce

Output
docker-ce | 5:20.10.7~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.6~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.5~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.4~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.3~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages

# ติดตั้งตาม version
sudo apt install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```

ตรวจสอบดูให้แน่ใจเกี่ยวกับ Docker repo
```bash
apt-cache policy docker-ce
```
ผลลัพท์
```
docker-ce:
  Installed: (none)
  Candidate: 5:19.03.9~3-0~ubuntu-focal
  Version table:
     5:19.03.9~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```

เมื่อทำการติดตั้งเรียบร้อยสามารถตรวจสอบการทำงานของ Docker ได้โดย
```bash
sudo systemctl status docker
```
ผลลัพท์
```
Output
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-05-19 17:00:41 UTC; 17s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 24321 (dockerd)
      Tasks: 8
     Memory: 46.4M
     CGroup: /system.slice/docker.service
             └─24321 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```


## วิธีแก้ไขให้ Docker ไม่ต้องใช้ `sudo` นำหน้าทุกครั้ง
เริ่มต้นให้ทำการเพิ่ม username ที่ใช้งานเข้าในกลุ่ม docker group กรณีนี้ถ้าเราทำงานอยู่บน user ใดระบบจะทำการเพิ่ม user ปัจจุบ้นของเราเข้า docker group ด้วยคำสั่งต่อไปนี้
```bash
sudo usermod -aG docker ${USER}
```
หรือจะระบุเป็นชื่อ username นั้นตรงๆไปดังนี้
```bash
sudo usermod -aG docker [username]
```
ทำการเช็ค user
```bash
su - ${USER}
```
จากนั้นใส่ Password

เมื่อเสร็จทุกขั้นตอนสามารถทดลองไม่ใช้ `sudo` ตามตัวอย่างต่อไปนี้
```bash
docker run hello-world
```
