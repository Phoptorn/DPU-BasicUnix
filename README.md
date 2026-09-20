# DPU-BasicUnix
Master of Engineering - Artificial Intelligence and Data Engineering

Unix + Docker Practice Repo for Class

# 🐧 Unix เบื้องต้นสำหรับ AI Engineer

คู่มือฉบับภาษาไทย สอนคำสั่ง Unix/Linux ที่จำเป็นตั้งแต่ศูนย์ จนใช้งานจริงในงาน AI/Data Engineering ได้

**กลุ่มเป้าหมาย:** ผู้เริ่มต้น นักศึกษา และวิศวกรที่ต้องทำงานบน Server / Docker / Cloud

## สารบัญ

1. [Unix คืออะไร](#1-unix-คืออะไร)
2. [ทำไม Unix จึงสำคัญกับงาน AI Engineering](#2-ทำไม-unix-จึงสำคัญกับงาน-ai-engineering)
3. [คำสั่ง ls และตระกูลของมัน](#3-คำสั่ง-ls-และตระกูลของมัน)
4. [การสร้างไดเรกทอรี](#4-การสร้างไดเรกทอรี)
5. [การเปลี่ยนไดเรกทอรี](#5-การเปลี่ยนไดเรกทอรี)
6. [ไดเรกทอรีปัจจุบัน (.) และแม่ (..)](#6-ไดเรกทอรีปัจจุบัน--และแม่-)
7. [Home Directory](#7-home-directory)
8. [การคัดลอกและย้ายไฟล์](#8-การคัดลอกและย้ายไฟล์)
9. [การลบไฟล์และไดเรกทอรี](#9-การลบไฟล์และไดเรกทอรี)
10. [การแสดงเนื้อหาไฟล์](#10-การแสดงเนื้อหาไฟล์)
11. [การค้นหาข้อความในไฟล์](#11-การค้นหาข้อความในไฟล์)
12. [Redirect Output และ Input](#12-redirect-output-และ-input)
13. [Pipes](#13-pipes)
14. [Wildcards](#14-wildcards)
15. [ความปลอดภัยของระบบไฟล์](#15-ความปลอดภัยของระบบไฟล์-สิทธิ์การเข้าถึง)
16. [Process เบื้องหลังและที่หยุดชั่วคราว](#16-การดู-process-ที่หยุดชั่วคราวและทำงานเบื้องหลัง)
17. [การ Kill Process](#17-การ-kill-process)
18. [Unix Cheat Sheet](#18-unix-cheat-sheet)

---

## 1. Unix คืออะไร

**Unix** คือระบบปฏิบัติการที่พัฒนาขึ้นราวปี 1969 ที่ AT&T Bell Labs โดย Ken Thompson และ Dennis Ritchie ปัจจุบันคำว่า "Unix" มักหมายถึงตระกูลระบบปฏิบัติการที่สืบทอดแนวคิดเดียวกัน เช่น **Linux**, **macOS**, **FreeBSD**

### ปรัชญาของ Unix
- **ทำสิ่งเดียวให้ดีที่สุด** — แต่ละโปรแกรมทำงานเล็ก ๆ อย่างเดียว แต่ทำได้ดีเยี่ยม
- **ทุกอย่างคือไฟล์** — ฮาร์ดดิสก์ เครือข่าย อุปกรณ์ ล้วนถูกมองเป็นไฟล์
- **ต่อกันได้** — ใช้ Pipe ส่งผลลัพธ์จากโปรแกรมหนึ่งไปอีกโปรแกรมหนึ่ง
- **ข้อความคือมาตรฐานกลาง** — ใช้ plain text เป็นตัวกลางสื่อสาร

### โครงสร้างหลัก

| ส่วนประกอบ | หน้าที่ |
|---|---|
| **Kernel** | แกนกลาง จัดการ CPU, RAM, Disk, Process |
| **Shell** | ตัวแปลคำสั่งที่เราพิมพ์ (bash, zsh, sh) |
| **File System** | โครงสร้างไฟล์แบบต้นไม้ เริ่มจาก `/` (root) |
| **Utilities** | โปรแกรมเล็ก ๆ เช่น `ls`, `grep`, `awk` |

```text
/
├── bin/     โปรแกรมพื้นฐาน
├── etc/     ไฟล์ตั้งค่าระบบ
├── home/    บ้านของผู้ใช้แต่ละคน
├── tmp/     ไฟล์ชั่วคราว
├── usr/     โปรแกรมและไลบรารีของผู้ใช้
└── var/     ข้อมูลที่เปลี่ยนแปลงบ่อย เช่น log
```

---

## 2. ทำไม Unix จึงสำคัญกับงาน AI Engineering

> ความจริงข้อหนึ่ง: **โมเดล AI แทบทั้งหมดในโลกถูกเทรนบน Linux** ไม่ใช่ Windows

### 2.1 เซิร์ฟเวอร์ GPU ทั้งหมดคือ Linux
เครื่อง GPU บน AWS, GCP, Azure, RunPod, Lambda Labs ล้วนรัน Linux และเข้าถึงผ่าน SSH ที่มีแต่ Terminal ไม่มีหน้าจอกราฟิก

```bash
ssh -i key.pem ubuntu@203.0.113.10
nvidia-smi          # ดูสถานะ GPU
```

### 2.2 จัดการ Dataset ขนาดใหญ่
ไฟล์ CSV ขนาด 50 GB เปิดด้วย Excel ไม่ได้ แต่ Unix จัดการได้ในวินาที

```bash
wc -l dataset.csv                    # นับจำนวนแถว
head -n 5 dataset.csv                # ดู 5 แถวแรก
grep "error" logs.txt | wc -l        # นับจำนวน error
cut -d',' -f2 data.csv | sort -u     # ดูค่าที่ไม่ซ้ำในคอลัมน์ 2
```

### 2.3 Training Job ที่รันข้ามวันข้ามคืน

```bash
nohup python train.py > train.log 2>&1 &
tail -f train.log        # ดู log แบบ real-time
```

### 2.4 Docker และ Kubernetes สร้างบน Linux

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
CMD ["python", "serve.py"]
```

### 2.5 Reproducibility และ Automation

```bash
#!/bin/bash
for lr in 0.001 0.0005 0.0001; do
  python train.py --lr $lr --out "model_lr${lr}.pt"
done
```

### สรุปเป็นตาราง

| งาน AI Engineering | คำสั่ง Unix ที่ใช้ |
|---|---|
| ต่อเข้าเซิร์ฟเวอร์ GPU | `ssh`, `scp`, `rsync` |
| สำรวจ Dataset | `head`, `tail`, `wc`, `cut`, `sort` |
| ค้นหาใน Log | `grep`, `awk`, `less` |
| รัน Training ยาว ๆ | `nohup`, `&`, `screen`, `tmux` |
| ตรวจสอบทรัพยากร | `top`, `htop`, `df`, `du`, `nvidia-smi` |
| จัดการ Process ค้าง | `ps`, `kill` |
| จัดการสิทธิ์ไฟล์ | `chmod`, `chown` |

---

## 3. คำสั่ง ls และตระกูลของมัน

```bash
ls              # แสดงไฟล์ในไดเรกทอรีปัจจุบัน (ไม่รวมไฟล์ซ่อน)
ls -a           # แสดงทั้งหมด รวมไฟล์ซ่อนที่ขึ้นต้นด้วยจุด (.env, .git)
ls -l           # แสดงแบบละเอียด (long format)
ls -al          # รวม -a และ -l เข้าด้วยกัน
ls -lh          # ขนาดไฟล์อ่านง่าย (K, M, G)
ls -lt          # เรียงตามเวลาแก้ไขล่าสุดก่อน
ls -ltr         # เรียงตามเวลา แต่กลับด้าน (ใหม่สุดอยู่ท้าย)
ls -lS          # เรียงตามขนาดไฟล์ ใหญ่ไปเล็ก
ls -R           # แสดงแบบ recursive ลงไปในโฟลเดอร์ย่อยทั้งหมด
ls -d */        # แสดงเฉพาะไดเรกทอรี
ls /path/to/dir # แสดงไฟล์ในพาธที่ระบุ
```

### อ่านผลลัพธ์ของ `ls -l`

```text
-rw-r--r--  1  somchai  staff   2048  Jan 15 10:30  train.py
drwxr-xr-x  5  somchai  staff    160  Jan 14 09:12  datasets
│└─┬┘└┬┘└┬┘  │     │       │      │         │          │
│  │  │  │   │     │       │      │         │          └─ ชื่อไฟล์
│  │  │  │   │     │       │      │         └─ วันเวลาที่แก้ไขล่าสุด
│  │  │  │   │     │       │      └─ ขนาด (bytes)
│  │  │  │   │     │       └─ กลุ่มเจ้าของ
│  │  │  │   │     └─ ผู้เป็นเจ้าของ
│  │  │  │   └─ จำนวน hard link
│  │  │  └─ สิทธิ์ของผู้ใช้อื่น (others)
│  │  └─ สิทธิ์ของกลุ่ม (group)
│  └─ สิทธิ์ของเจ้าของ (user)
└─ ชนิด: - = ไฟล์ธรรมดา, d = directory, l = symbolic link
```

### ตัวอย่างใช้งานจริง

```bash
ls -lh models/                    # ดูขนาดไฟล์โมเดล
ls -ltr checkpoints/ | tail -5    # ดู checkpoint 5 อันล่าสุด
```

---

## 4. การสร้างไดเรกทอรี

```bash
mkdir data                         # สร้างโฟลเดอร์ชื่อ data
mkdir data models logs             # สร้างหลายโฟลเดอร์พร้อมกัน
mkdir -p project/data/raw          # -p สร้างโฟลเดอร์ซ้อนกันทีเดียว
mkdir -p project/{data,models,src} # ใช้ brace expansion
mkdir -m 755 public                # สร้างพร้อมกำหนดสิทธิ์
```

> 💡 **เคล็ดลับ:** `-p` ยังป้องกัน error กรณีโฟลเดอร์มีอยู่แล้ว เหมาะมากสำหรับเขียนใน script

### โครงสร้างโปรเจกต์ AI มาตรฐาน

```bash
mkdir -p my-ai-project/{data/{raw,processed},notebooks,src,models,logs,configs}
```

```text
my-ai-project/
├── configs/
├── data/
│   ├── processed/
│   └── raw/
├── logs/
├── models/
├── notebooks/
└── src/
```

---

## 5. การเปลี่ยนไดเรกทอรี

```bash
cd datasets            # เข้าโฟลเดอร์ datasets (relative path)
cd /home/user/data     # เข้าด้วยพาธเต็ม (absolute path)
cd ..                  # ถอยขึ้นไป 1 ระดับ
cd ../..               # ถอยขึ้นไป 2 ระดับ
cd ~                   # กลับไป home directory
cd                     # เหมือน cd ~
cd -                   # กลับไปโฟลเดอร์ก่อนหน้าที่เพิ่งอยู่
cd /                   # ไปที่ root ของระบบ
pwd                    # แสดงว่าตอนนี้อยู่ที่ไหน
```

### Absolute vs Relative Path

| แบบ | ลักษณะ | ตัวอย่าง |
|---|---|---|
| **Absolute** | เริ่มด้วย `/` เสมอ ชี้ตำแหน่งเดิมทุกครั้ง | `/home/somchai/data/train.csv` |
| **Relative** | อ้างอิงจากตำแหน่งปัจจุบัน | `data/train.csv` หรือ `../models/` |

> 💡 กด **Tab** เพื่อเติมชื่อโฟลเดอร์อัตโนมัติ ช่วยลดการพิมพ์ผิดได้มาก

---

## 6. ไดเรกทอรีปัจจุบัน (.) และแม่ (..)

| สัญลักษณ์ | ความหมาย |
|---|---|
| `.` | ไดเรกทอรีปัจจุบัน (current directory) |
| `..` | ไดเรกทอรีแม่ที่อยู่เหนือขึ้นไป (parent directory) |

```bash
ls -a                  # จะเห็น . และ .. เสมอ
./script.sh            # รันสคริปต์ในโฟลเดอร์ปัจจุบัน
cp ../config.yaml .    # คัดลอกไฟล์จากโฟลเดอร์แม่มาที่นี่
cd ../sibling-folder   # ข้ามไปโฟลเดอร์พี่น้อง
python ../src/train.py # รันไฟล์จากโฟลเดอร์แม่
```

### ทำไมต้องใช้ `./` ตอนรันสคริปต์?

Shell จะค้นหาโปรแกรมจากพาธใน `$PATH` เท่านั้น และโดยปกติโฟลเดอร์ปัจจุบัน **ไม่ได้อยู่ใน** `$PATH` (เพื่อความปลอดภัย) จึงต้องระบุ `./` เพื่อบอกชัดเจนว่า "เอาไฟล์จากที่นี่"

```bash
script.sh      # ❌ command not found
./script.sh    # ✅ ทำงานได้
```

---

## 7. Home Directory

**Home directory** คือพื้นที่ส่วนตัวของผู้ใช้แต่ละคน แทนด้วยเครื่องหมาย `~` (tilde)

```bash
echo $HOME       # แสดงพาธ home เช่น /home/somchai
cd ~             # กลับบ้าน
cd ~/projects    # เข้าโฟลเดอร์ projects ในบ้าน
ls ~             # ดูไฟล์ในบ้าน
cd ~otheruser    # เข้า home ของผู้ใช้คนอื่น (ถ้ามีสิทธิ์)
whoami           # ดูว่าตอนนี้เป็นผู้ใช้อะไร
```

### ตำแหน่ง Home ในแต่ละระบบ

| ระบบ | ตำแหน่ง |
|---|---|
| Linux | `/home/username` |
| macOS | `/Users/username` |
| root | `/root` |

### ไฟล์ซ่อนสำคัญในบ้าน

```bash
~/.bashrc           # ตั้งค่า shell (bash)
~/.zshrc            # ตั้งค่า shell (zsh)
~/.ssh/             # กุญแจ SSH
~/.gitconfig        # ตั้งค่า Git
~/.cache/           # แคช เช่น โมเดล HuggingFace
~/.aws/credentials  # คีย์ AWS
```

> ⚠️ **ระวัง:** พื้นที่ใน home บนเซิร์ฟเวอร์มักถูกจำกัด (quota) ควรเก็บ dataset ใหญ่ ๆ ไว้ที่ `/data` หรือ volume แยก

---

## 8. การคัดลอกและย้ายไฟล์

### คัดลอก — `cp`

```bash
cp train.py train_backup.py        # คัดลอกไฟล์
cp train.py ~/backup/              # คัดลอกไปโฟลเดอร์อื่น
cp *.py scripts/                   # คัดลอกไฟล์ .py ทั้งหมด
cp -r datasets/ datasets_backup/   # -r คัดลอกทั้งโฟลเดอร์
cp -i a.txt b.txt                  # -i ถามก่อนเขียนทับ
cp -v a.txt b.txt                  # -v แสดงผลระหว่างทำงาน
cp -p a.txt b.txt                  # -p รักษา timestamp และสิทธิ์เดิม
cp -u src/* dest/                  # -u คัดลอกเฉพาะไฟล์ที่ใหม่กว่า
```

### ย้ายและเปลี่ยนชื่อ — `mv`

```bash
mv old_name.py new_name.py         # เปลี่ยนชื่อไฟล์
mv model.pt models/                # ย้ายไฟล์ไปโฟลเดอร์อื่น
mv *.log logs/                     # ย้ายไฟล์ log ทั้งหมด
mv -i a.txt b.txt                  # ถามก่อนเขียนทับ
mv old_folder/ new_folder/         # เปลี่ยนชื่อโฟลเดอร์ (ไม่ต้องใช้ -r)
```

### คัดลอกข้ามเครื่อง — `scp` และ `rsync`

```bash
scp model.pt user@server:/home/user/models/                  # อัปโหลด
scp user@server:/home/user/logs/train.log ./                 # ดาวน์โหลด
rsync -avzP datasets/ user@server:/data/datasets/            # dataset ใหญ่
```

| Flag ของ rsync | ความหมาย |
|---|---|
| `-a` | archive รักษาสิทธิ์และ timestamp |
| `-v` | verbose แสดงรายละเอียด |
| `-z` | บีบอัดระหว่างส่ง |
| `-P` | แสดง progress และ resume ได้ |

---

## 9. การลบไฟล์และไดเรกทอรี

> ⚠️ **คำเตือนสำคัญ:** Unix **ไม่มีถังขยะ** ลบแล้วคือหายถาวร ตรวจสอบให้ดีทุกครั้ง

```bash
rm file.txt                # ลบไฟล์
rm file1.txt file2.txt     # ลบหลายไฟล์
rm -i file.txt             # ถามยืนยันก่อนลบ (แนะนำสำหรับมือใหม่)
rm -f file.txt             # บังคับลบ ไม่ถาม
rm -r old_folder/          # ลบโฟลเดอร์พร้อมเนื้อหาข้างใน
rm -rf temp/               # ลบแบบบังคับทั้งโฟลเดอร์
rmdir empty_folder/        # ลบเฉพาะโฟลเดอร์ว่าง (ปลอดภัยกว่า)
```

### คำสั่งอันตรายที่ห้ามพิมพ์เด็ดขาด

```bash
rm -rf /        # ❌ ลบทั้งระบบปฏิบัติการ
rm -rf ~        # ❌ ลบไฟล์ส่วนตัวทั้งหมด
rm -rf *        # ❌ ลบทุกอย่างในโฟลเดอร์ปัจจุบัน
rm -rf . /tmp   # ❌ เว้นวรรคผิดที่ = หายนะ
```

### วิธีป้องกันตัวเอง

```bash
# 1. ใช้ ls ตรวจสอบก่อนเสมอ
ls *.tmp
rm *.tmp

# 2. ตั้ง alias ให้ถามทุกครั้ง (ใส่ใน ~/.bashrc)
alias rm='rm -i'

# 3. ใช้ trash-cli แทน (ลบลงถังขยะจริง)
sudo apt install trash-cli
trash-put file.txt
```

---

## 10. การแสดงเนื้อหาไฟล์

```bash
cat file.txt              # แสดงทั้งไฟล์ เหมาะกับไฟล์สั้น
cat -n file.txt           # แสดงพร้อมเลขบรรทัด
cat a.txt b.txt           # ต่อไฟล์หลายไฟล์เข้าด้วยกัน

less large_file.log       # เปิดอ่านแบบเลื่อนได้ (แนะนำที่สุด)
more large_file.log       # คล้าย less แต่เก่ากว่า

head file.csv             # แสดง 10 บรรทัดแรก
head -n 20 file.csv       # แสดง 20 บรรทัดแรก

tail file.log             # แสดง 10 บรรทัดสุดท้าย
tail -n 50 file.log       # แสดง 50 บรรทัดสุดท้าย
tail -f train.log         # 🔥 ติดตาม log แบบ real-time

wc -l data.csv            # นับจำนวนบรรทัด
wc -w report.txt          # นับจำนวนคำ
wc -c file.bin            # นับจำนวน byte
```

### ปุ่มลัดใน `less`

| ปุ่ม | การทำงาน |
|---|---|
| `Space` | เลื่อนลงหนึ่งหน้า |
| `b` | เลื่อนขึ้นหนึ่งหน้า |
| `g` | ไปบรรทัดแรก |
| `G` | ไปบรรทัดสุดท้าย |
| `/คำค้น` | ค้นหาไปข้างหน้า |
| `n` / `N` | ผลลัพธ์ถัดไป / ก่อนหน้า |
| `q` | ออก |

### ใช้งานจริงกับงาน AI

```bash
head -n 1 dataset.csv          # ดูชื่อคอลัมน์
tail -f nohup.out              # เฝ้าดู loss ระหว่างเทรน
wc -l train.csv                # นับจำนวนตัวอย่างข้อมูล
```

---

## 11. การค้นหาข้อความในไฟล์

`grep` = **G**lobal **R**egular **E**xpression **P**rint

```bash
grep "error" app.log              # หาบรรทัดที่มีคำว่า error
grep -i "ERROR" app.log           # -i ไม่สนตัวพิมพ์เล็กใหญ่
grep -n "error" app.log           # -n แสดงเลขบรรทัดด้วย
grep -c "error" app.log           # -c นับจำนวนบรรทัดที่เจอ
grep -v "debug" app.log           # -v แสดงบรรทัดที่ "ไม่มี" คำนี้
grep -r "TODO" src/               # -r ค้นทุกไฟล์ในโฟลเดอร์
grep -w "cat" file.txt            # -w ตรงทั้งคำเท่านั้น
grep -l "import torch" *.py       # -l แสดงเฉพาะชื่อไฟล์ที่เจอ
grep -A 3 "Exception" app.log     # แสดง 3 บรรทัดหลังจากที่เจอ
grep -B 3 "Exception" app.log     # แสดง 3 บรรทัดก่อนหน้า
grep -C 3 "Exception" app.log     # แสดงทั้งก่อนและหลัง 3 บรรทัด
grep -E "error|warning" app.log   # -E ใช้ regex แบบขยาย
```

### ค้นหาไฟล์ด้วย `find`

```bash
find . -name "*.py"                    # หาไฟล์ .py ทั้งหมด
find . -type d -name "checkpoint*"     # หาเฉพาะโฟลเดอร์
find . -size +100M                     # หาไฟล์ใหญ่กว่า 100 MB
find . -mtime -7                       # หาไฟล์ที่แก้ไขใน 7 วันล่าสุด
find . -name "*.tmp" -delete           # หาแล้วลบเลย
find /data -name "*.csv" -exec wc -l {} \;
```

### ตัวอย่างงานจริง

```bash
grep -n -A 5 "Traceback" train.log     # หาว่า training พังตรงไหน
grep -c "Epoch" train.log              # นับ epoch ที่เทรนไปแล้ว
grep "loss:" train.log | tail -20      # ดึงค่า loss ล่าสุด
find ./models -name "*.pt" -size +1G   # หาไฟล์โมเดลใหญ่เกิน 1 GB
```

---

## 12. Redirect Output และ Input

| ชื่อ | หมายเลข | หน้าที่ |
|---|---|---|
| stdin | `0` | ช่องรับข้อมูลเข้า |
| stdout | `1` | ช่องส่งผลลัพธ์ปกติ |
| stderr | `2` | ช่องส่งข้อความ error |

```bash
python train.py > output.log        # เขียนทับไฟล์เดิม
python train.py >> output.log       # ต่อท้ายไฟล์เดิม
python train.py 2> error.log        # แยกเก็บเฉพาะ error
python train.py > out.log 2>&1      # 🔥 เก็บทั้ง output และ error ไว้ไฟล์เดียว
python train.py &> all.log          # แบบย่อ (bash เท่านั้น)
python train.py > /dev/null 2>&1    # ทิ้งทุกอย่าง

sort < unsorted.txt                 # ป้อนไฟล์เข้าทาง stdin
sort < in.txt > out.txt             # รับเข้าและส่งออกพร้อมกัน
```

### Here Document

```bash
cat << EOF > config.yaml
model: bert-base
batch_size: 32
learning_rate: 0.0001
EOF
```

### ใช้งานจริง

```bash
python train.py --epochs 100 > logs/run_$(date +%Y%m%d_%H%M).log 2>&1
nvidia-smi -l 5 >> gpu_usage.log
```

> 💡 `2>&1` แปลว่า "ส่ง stderr (2) ไปที่เดียวกับ stdout (1)" ต้องเขียน **หลัง** `>` เสมอ

---

## 13. Pipes

Pipe `|` ส่งผลลัพธ์จากคำสั่งหนึ่งเข้าเป็นอินพุตของอีกคำสั่ง — หัวใจของปรัชญา Unix

```bash
cat file.txt | grep "error"           # อ่านไฟล์แล้วกรอง
ls -l | grep ".py"                    # แสดงเฉพาะไฟล์ Python
ps aux | grep python                  # หา process ของ python
cat data.csv | wc -l                  # นับบรรทัด
history | grep docker                 # หาคำสั่ง docker ที่เคยใช้
du -sh * | sort -hr | head -10        # หา 10 โฟลเดอร์ที่กินพื้นที่มากสุด
```

### ต่อหลายชั้นได้

```bash
cat app.log | grep "ERROR" | awk '{print $4}' | sort | uniq -c | sort -rn | head -5
```

อธิบายทีละขั้น:
1. `cat app.log` — อ่านไฟล์
2. `grep "ERROR"` — เอาเฉพาะบรรทัด error
3. `awk '{print $4}'` — ดึงคอลัมน์ที่ 4
4. `sort` — เรียงลำดับ
5. `uniq -c` — นับจำนวนที่ซ้ำกัน
6. `sort -rn` — เรียงจากมากไปน้อย
7. `head -5` — เอา 5 อันดับแรก

### เพื่อนคู่ใจของ Pipe

```bash
sort        # เรียงลำดับ
uniq        # ตัดรายการซ้ำ (ต้อง sort ก่อน)
cut         # ตัดเอาเฉพาะคอลัมน์
awk         # ประมวลผลข้อความแบบคอลัมน์
sed         # แก้ไขข้อความแบบ stream
tee         # แสดงบนจอพร้อมบันทึกไฟล์
xargs       # แปลง input เป็น argument ของคำสั่งถัดไป
```

```bash
python train.py | tee train.log       # ดูผลบนจอ + บันทึกไฟล์
find . -name "*.tmp" | xargs rm       # หาแล้วลบ
```

---

## 14. Wildcards

| สัญลักษณ์ | ความหมาย | ตัวอย่าง |
|---|---|---|
| `*` | ตัวอักษรกี่ตัวก็ได้ (รวมศูนย์ตัว) | `*.csv` |
| `?` | ตัวอักษรหนึ่งตัวพอดี | `data?.txt` |
| `[abc]` | ตัวใดตัวหนึ่งในวงเล็บ | `file[123].txt` |
| `[a-z]` | ช่วงตัวอักษร | `[a-m]*.py` |
| `[!abc]` | ตัวที่**ไม่ใช่**ในวงเล็บ | `[!0-9]*.log` |
| `{a,b}` | ตัวเลือกหลายแบบ | `*.{jpg,png,gif}` |

```bash
ls *.py                         # ไฟล์ Python ทั้งหมด
ls model_*.pt                   # ไฟล์โมเดลทุกอัน
ls data_?.csv                   # data_1.csv, data_a.csv
ls *.{jpg,png}                  # ไฟล์ภาพ jpg และ png
cp *.csv backup/                # คัดลอก CSV ทั้งหมด
rm checkpoint_[0-9].pt          # ลบ checkpoint 0-9
mv 2024-*.log archive/2024/     # ย้าย log ปี 2024
```

> ⚠️ `*` ไม่จับไฟล์ซ่อนที่ขึ้นต้นด้วยจุด ต้องใช้ `.*` แยกต่างหาก
> 💡 ใช้ `ls` ทดสอบ pattern ก่อนใช้กับ `rm` เสมอ

---

## 15. ความปลอดภัยของระบบไฟล์ (สิทธิ์การเข้าถึง)

```text
-rwxr-xr--
│ │  │  │
│ │  │  └── others (คนอื่น)  : r-- = อ่านได้อย่างเดียว
│ │  └───── group (กลุ่ม)    : r-x = อ่านและรันได้
│ └──────── user (เจ้าของ)   : rwx = อ่าน เขียน รัน ได้หมด
└────────── ชนิดไฟล์         : - = ไฟล์, d = โฟลเดอร์, l = link
```

### ความหมายของสิทธิ์

| สิทธิ์ | ค่าตัวเลข | กับไฟล์ | กับไดเรกทอรี |
|---|---|---|---|
| `r` (read) | 4 | อ่านเนื้อหาได้ | ดูรายชื่อไฟล์ได้ (`ls`) |
| `w` (write) | 2 | แก้ไขได้ | สร้าง/ลบไฟล์ข้างในได้ |
| `x` (execute) | 1 | รันเป็นโปรแกรมได้ | เข้าไปข้างในได้ (`cd`) |

### chmod — เปลี่ยนสิทธิ์

```bash
# แบบตัวเลข (Octal)
chmod 755 script.sh     # rwxr-xr-x
chmod 644 config.txt    # rw-r--r--
chmod 600 ~/.ssh/id_rsa # rw------- (สำหรับ SSH key)
chmod 777 folder/       # ⚠️ อันตราย ไม่แนะนำ
chmod -R 755 scripts/   # -R ใช้กับทุกไฟล์ในโฟลเดอร์

# แบบสัญลักษณ์
chmod +x script.sh      # เพิ่มสิทธิ์รันให้ทุกคน
chmod u+x script.sh     # เพิ่มสิทธิ์รันเฉพาะเจ้าของ
chmod g-w file.txt      # ตัดสิทธิ์เขียนของกลุ่ม
chmod o-rwx secret.txt  # ตัดสิทธิ์คนอื่นทั้งหมด
chmod a+r public.txt    # ให้ทุกคนอ่านได้
```

### ตารางค่าที่ใช้บ่อย

| ค่า | สิทธิ์ | เหมาะกับ |
|---|---|---|
| `600` | `rw-------` | SSH key, ไฟล์ credential |
| `644` | `rw-r--r--` | ไฟล์ข้อมูล, ไฟล์ config ทั่วไป |
| `700` | `rwx------` | โฟลเดอร์ส่วนตัว, `~/.ssh` |
| `755` | `rwxr-xr-x` | สคริปต์, โฟลเดอร์สาธารณะ |

### chown — เปลี่ยนเจ้าของ

```bash
sudo chown somchai file.txt
sudo chown somchai:staff file.txt
sudo chown -R user:group /data/
```

### แนวปฏิบัติด้านความปลอดภัย

```bash
chmod 600 ~/.ssh/id_rsa       # SSH key ต้อง 600 ไม่งั้น ssh จะปฏิเสธ
chmod 600 .env                # ไฟล์ API key
chmod 600 ~/.aws/credentials  # คีย์ AWS
```

> ⚠️ **อย่าใช้ `chmod 777`** เป็นอันขาดในระบบจริง

---

## 16. การดู Process ที่หยุดชั่วคราวและทำงานเบื้องหลัง

### รันงานเบื้องหลัง

```bash
python train.py &              # รันเบื้องหลังทันที
Ctrl + Z                       # หยุดงานปัจจุบันชั่วคราว (suspend)
bg                             # ส่งงานที่หยุดอยู่ไปทำงานเบื้องหลัง
fg                             # ดึงงานเบื้องหลังกลับมาหน้าจอ
fg %1                          # ดึงงานหมายเลข 1 กลับมา
```

### ดูรายการงาน

```bash
jobs                  # ดูงานทั้งหมดใน shell นี้
jobs -l               # ดูพร้อม PID
```

```text
[1]-  Stopped                 python preprocess.py
[2]+  Running                 python train.py &
```

| สถานะ | ความหมาย |
|---|---|
| `Running` | กำลังทำงานเบื้องหลัง |
| `Stopped` | หยุดชั่วคราว (จาก Ctrl+Z) |
| `Done` | ทำงานเสร็จแล้ว |

### ดู Process ทั้งระบบ

```bash
ps                      # process ของ shell ปัจจุบัน
ps aux                  # ทุก process ในระบบ
ps aux | grep python    # กรองเฉพาะ python
ps -ef                  # อีกรูปแบบหนึ่ง

top                     # ดูแบบ real-time
htop                    # สวยกว่า ใช้งานง่ายกว่า
nvidia-smi              # ดู process ที่ใช้ GPU
```

### รันงานให้อยู่รอดแม้ปิด Terminal

```bash
# วิธีที่ 1: nohup
nohup python train.py > train.log 2>&1 &

# วิธีที่ 2: screen
screen -S training      # สร้าง session (ออกด้วย Ctrl+A ตามด้วย D)
screen -r training      # กลับเข้ามาใหม่

# วิธีที่ 3: tmux (แนะนำที่สุด)
tmux new -s training    # สร้าง session (ออกด้วย Ctrl+B ตามด้วย D)
tmux attach -t training # กลับเข้ามา
tmux ls                 # ดู session ทั้งหมด
```

> 💡 งานเทรนโมเดลที่ใช้เวลาหลายชั่วโมง **ใช้ tmux หรือ screen เสมอ** ไม่งั้นเน็ตหลุดแล้วงานตายทันที

---

## 17. การ Kill Process

### ขั้นตอนที่ 1: หา PID ก่อน

```bash
ps aux | grep train.py          # หา PID จากชื่อโปรแกรม
pgrep -f train.py               # แสดงเฉพาะ PID
top                             # ดูว่าตัวไหนกิน CPU มาก
nvidia-smi                      # ดู PID ที่ใช้ GPU อยู่
```

### ขั้นตอนที่ 2: สั่ง kill

```bash
kill 12345               # ส่ง SIGTERM ปิดอย่างสุภาพ (ค่าเริ่มต้น)
kill -15 12345           # เหมือนด้านบน
kill -9 12345            # ⚠️ SIGKILL บังคับปิดทันที
kill -2 12345            # SIGINT เหมือนกด Ctrl+C

kill %1                  # kill งานหมายเลข 1 จาก jobs
killall python           # kill ทุก process ชื่อ python
pkill -f "train.py"      # kill โดยจับจากชื่อคำสั่ง
```

### สัญญาณที่ควรรู้

| สัญญาณ | เลข | ความหมาย | ปิด process ได้ไหม |
|---|---|---|---|
| SIGINT | 2 | เหมือนกด Ctrl+C | ได้ (ปิดเองอย่างเรียบร้อย) |
| SIGTERM | 15 | ขอให้ปิดอย่างสุภาพ | ได้ (ค่าเริ่มต้น) |
| SIGKILL | 9 | บังคับปิดทันที | ได้เสมอ ปฏิเสธไม่ได้ |
| SIGSTOP | 19 | หยุดชั่วคราว | ไม่ (แค่หยุด) |
| SIGCONT | 18 | ทำงานต่อ | — |

> 💡 **ลำดับที่ถูกต้อง:** ลอง `kill` (15) ก่อนเสมอ เพราะโปรแกรมจะมีโอกาสเซฟ checkpoint และปิดไฟล์ให้เรียบร้อย ถ้ายังไม่ยอมตายค่อยใช้ `kill -9`

### ปัญหาที่เจอบ่อยในงาน AI

```bash
# GPU memory ไม่ยอมคืน หลัง training ล่ม
nvidia-smi
kill -9 <PID>
nvidia-smi

# ปิด process python ทั้งหมดที่ใช้ GPU
nvidia-smi --query-compute-apps=pid --format=csv,noheader | xargs -r kill -9

# หา process ที่ยึดพอร์ตอยู่
lsof -i :8000
kill -9 <PID>
```

---

## 18. Unix Cheat Sheet

### นำทางและสำรวจ

| คำสั่ง | ความหมาย |
|---|---|
| `pwd` | อยู่ที่ไหนตอนนี้ |
| `ls -al` | ดูไฟล์ทั้งหมดแบบละเอียด |
| `cd dir` | เข้าโฟลเดอร์ |
| `cd ..` | ถอยขึ้นหนึ่งระดับ |
| `cd ~` | กลับบ้าน |
| `cd -` | กลับที่เดิมก่อนหน้า |
| `tree` | ดูโครงสร้างแบบต้นไม้ |

### จัดการไฟล์

| คำสั่ง | ความหมาย |
|---|---|
| `mkdir -p a/b/c` | สร้างโฟลเดอร์ซ้อน |
| `touch file` | สร้างไฟล์ว่าง |
| `cp -r src dst` | คัดลอกทั้งโฟลเดอร์ |
| `mv old new` | ย้ายหรือเปลี่ยนชื่อ |
| `rm -rf dir` | ลบทั้งโฟลเดอร์ (ระวัง!) |
| `ln -s target link` | สร้าง symbolic link |

### อ่านและค้นหา

| คำสั่ง | ความหมาย |
|---|---|
| `cat file` | แสดงทั้งไฟล์ |
| `less file` | อ่านแบบเลื่อนได้ |
| `head -n 20 f` | 20 บรรทัดแรก |
| `tail -f log` | ติดตาม log สด |
| `wc -l file` | นับบรรทัด |
| `grep -rn "x" .` | ค้นทุกไฟล์พร้อมเลขบรรทัด |
| `find . -name "*.py"` | หาไฟล์ตามชื่อ |
| `diff a b` | เทียบความต่างสองไฟล์ |

### Redirect และ Pipe

| สัญลักษณ์ | ความหมาย |
|---|---|
| `>` | เขียนทับไฟล์ |
| `>>` | ต่อท้ายไฟล์ |
| `2>` | เก็บเฉพาะ error |
| `> f 2>&1` | เก็บทั้ง output และ error |
| `\|` | ส่งต่อให้คำสั่งถัดไป |
| `\| tee f` | แสดงบนจอ + บันทึกไฟล์ |

### สิทธิ์และผู้ใช้

| คำสั่ง | ความหมาย |
|---|---|
| `chmod 755 f` | ตั้งสิทธิ์ rwxr-xr-x |
| `chmod +x f` | ทำให้รันได้ |
| `chown u:g f` | เปลี่ยนเจ้าของ |
| `whoami` | ฉันคือใคร |
| `sudo cmd` | รันด้วยสิทธิ์ผู้ดูแลระบบ |

### Process

| คำสั่ง | ความหมาย |
|---|---|
| `cmd &` | รันเบื้องหลัง |
| `Ctrl+Z` | หยุดชั่วคราว |
| `bg` / `fg` | ส่งไปหลัง / ดึงมาหน้า |
| `jobs` | ดูงานใน shell นี้ |
| `ps aux` | ดู process ทั้งระบบ |
| `top` / `htop` | monitor แบบสด |
| `kill -9 PID` | บังคับปิด |
| `nohup cmd &` | รันต่อแม้ปิด terminal |

### ระบบและทรัพยากร

| คำสั่ง | ความหมาย |
|---|---|
| `df -h` | พื้นที่ดิสก์คงเหลือ |
| `du -sh *` | ขนาดแต่ละโฟลเดอร์ |
| `free -h` | RAM ที่ใช้อยู่ |
| `nvidia-smi` | สถานะ GPU |
| `uname -a` | ข้อมูลระบบ |
| `history` | คำสั่งที่เคยพิมพ์ |

### ปุ่มลัดที่ต้องจำ

| ปุ่ม | การทำงาน |
|---|---|
| `Tab` | เติมชื่อไฟล์อัตโนมัติ |
| `Ctrl + C` | ยกเลิกคำสั่งที่รันอยู่ |
| `Ctrl + D` | ออกจาก shell |
| `Ctrl + L` | ล้างหน้าจอ |
| `Ctrl + R` | ค้นหาคำสั่งเก่า |
| `Ctrl + A` / `Ctrl + E` | ไปต้นบรรทัด / ท้ายบรรทัด |
| `↑` / `↓` | เลื่อนดูคำสั่งเก่า |

---

## 📚 แหล่งเรียนรู้เพิ่มเติม

```bash
man ls              # คู่มือฉบับเต็มของคำสั่ง
ls --help           # ความช่วยเหลือแบบย่อ
tldr ls             # ตัวอย่างใช้งานจริง (ต้องติดตั้ง tldr)
which python        # ดูว่าโปรแกรมอยู่ที่ไหน
type cd             # ดูว่าคำสั่งนี้เป็นชนิดอะไร
```

---

## ✅ แบบฝึกหัดท้ายบท

1. สร้างโครงสร้างโปรเจกต์ `ai-lab/` ที่มี `data/raw`, `data/clean`, `src`, `models` ด้วยคำสั่งเดียว
2. สร้างไฟล์ `notes.txt` แล้วเขียนคำว่า `hello unix` ลงไปโดยไม่ใช้ editor
3. หาไฟล์ `.log` ทั้งหมดในเครื่องที่ใหญ่กว่า 10 MB
4. นับจำนวนบรรทัดที่มีคำว่า `ERROR` ในไฟล์ log
5. ตั้งสิทธิ์ให้ `run.sh` รันได้เฉพาะเจ้าของ และคนอื่นอ่านได้อย่างเดียว
6. รัน `sleep 300` เบื้องหลัง แล้วหา PID และ kill มัน

---

**License:** MIT · **สร้างขึ้นเพื่อการเรียนการสอน**
