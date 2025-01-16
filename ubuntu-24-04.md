# วิธีติดตั้ง docker บน Ubuntu 24.04

## เริ่มต้นการติดตั้ง
ถอนการติดตั้ง Docker ของเก่าที่อาจมีอยู่แล้ว
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
ขั้นตอนแรกต้องทำการอัพเดท ubuntu
```bash
sudo apt update
```
จากนั้นทำการติดตั้ง Repository เพื่อใช้ในการติดตั้ง Docker 
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

ติดตั้ง Docker
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
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
