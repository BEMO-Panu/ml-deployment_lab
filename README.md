# ML Deployment Lab — Flask & Docker

## 1. รายละเอียดโปรเจกต์

โปรเจกต์นี้เป็นการนำโมเดล Machine Learning ไปใช้งานผ่านเว็บแอปพลิเคชัน โดยใช้ **Flask** สำหรับสร้าง Web Application และ REST API ร่วมกับ **Docker** เพื่อให้สามารถติดตั้งและรันระบบได้สะดวก

โมเดลใช้ชุดข้อมูล California Housing Dataset เพื่อทำนายราคาบ้านจากข้อมูลคุณลักษณะทั้งหมด 8 ตัวแปร

## 2. เทคโนโลยีที่ใช้

- Python — ภาษาที่ใช้พัฒนาโปรแกรม
- Scikit-learn — สร้างและฝึกสอนโมเดล Machine Learning
- Flask — พัฒนา Web Application และ REST API
- Docker — จัดการสภาพแวดล้อมและ Deploy แอปพลิเคชัน
- Git และ GitHub — จัดเก็บและเผยแพร่ Source Code

## 3. โครงสร้างโปรเจกต์

```text
ml_service/
├── app.py
├── train_model.py
├── requirements.txt
├── Dockerfile
├── README.md
├── .gitignore
├── templates/
└── model/
    └── model.pkl
```

**คำอธิบายไฟล์สำคัญ**

- `app.py` — โปรแกรม Flask สำหรับให้บริการหน้าเว็บและ API
- `train_model.py` — โปรแกรมฝึกสอนและบันทึกโมเดล
- `requirements.txt` — รายการ Python Libraries ที่ต้องติดตั้ง
- `Dockerfile` — คำสั่งสำหรับสร้าง Docker Image
- `templates/` — ไฟล์หน้าเว็บ
- `model/model.pkl` — ไฟล์โมเดลที่สร้างจากการ Train โดยอาจอยู่ภายใน Docker Image และไม่จำเป็นต้อง Push ขึ้น GitHub

## 4. วิธี Build Docker Image

เปิด Terminal และเข้าโฟลเดอร์โปรเจกต์

```bash
cd ml_service
```

สร้าง Docker Image ด้วยคำสั่ง

```bash
docker build -t ds-ml-service .
```

เมื่อติดตั้ง Dependencies และ Build สำเร็จ สามารถตรวจสอบ Image ได้ด้วยคำสั่ง

```bash
docker images ds-ml-service
```

## 5. วิธี Run Docker Container

รัน Container โดยเชื่อมต่อพอร์ต `8080` ของเครื่องกับพอร์ต `5000` ภายใน Container

```bash
docker run --rm -p 8080:5000 ds-ml-service
```

จากนั้นเปิดเว็บเบราว์เซอร์ที่

http://localhost:8080

**หมายเหตุ:** ใช้พอร์ต 8080 เนื่องจากพอร์ต 5000 บนเครื่องอาจมีโปรแกรมอื่นใช้งานอยู่

## 6. การทดสอบ API

### 6.1 ตรวจสอบสถานะ API (Health Check)

เปิด URL

http://localhost:8080/health

หาก API ทำงานปกติ ระบบจะตอบกลับข้อมูล JSON ที่แสดงสถานะการทำงาน

### 6.2 ทดสอบ Prediction API

ใช้คำสั่งต่อไปนี้บน macOS หรือ Linux Terminal

```bash
curl -X POST http://127.0.0.1:8080/predict \
  -H "Content-Type: application/json" \
  -d '{"features":[8.3,25.0,6.0,1.0,1200,3.0,34.2,-118.3]}'
```

**ผลลัพธ์ที่ได้จากการทดสอบ**

```json
{
  "prediction": 4.029472199999998
}
```

ผลลัพธ์แสดงว่า Flask API สามารถรับข้อมูล ส่งให้โมเดล Machine Learning ประมวลผล และส่งค่าทำนายกลับมาในรูปแบบ JSON ได้สำเร็จ

### 6.3 ทดสอบผ่าน PowerShell (Windows)

```powershell
Invoke-WebRequest -Uri "http://127.0.0.1:8080/predict" `
  -Method POST `
  -Body '{"features":[8.3,25.0,6.0,1.0,1200,3.0,34.2,-118.3]}' `
  -ContentType "application/json"
```

## 7. การตรวจสอบ Docker Container

แสดง Container ที่กำลังทำงาน

```bash
docker ps
```

แสดง Docker Images

```bash
docker images
```

หากต้องการหยุด Container ให้กด `Ctrl + C` ใน Terminal ที่รัน Docker

## 8. ผลการทดสอบระบบ

จากการทดลองพบว่า

1. สามารถ Build Docker Image ชื่อ `ds-ml-service` ได้สำเร็จ
2. สามารถ Run Docker Container ผ่านพอร์ต 8080 ได้
3. สามารถเปิดหน้าเว็บ Flask ผ่านเบราว์เซอร์ได้
4. Health Check API ทำงานปกติ
5. Prediction API สามารถส่งผลการทำนายกลับมาในรูปแบบ JSON ได้สำเร็จ

## 9. สรุป

โปรเจกต์นี้แสดงขั้นตอนการ Deploy โมเดล Machine Learning ด้วย Flask และ Docker ตั้งแต่การสร้างโมเดล การสร้าง Docker Image การรัน Container ไปจนถึงการทดสอบ REST API

ผลการทดสอบยืนยันว่าสามารถใช้งานโมเดลผ่านเว็บแอปพลิเคชันและ API ภายใน Docker Container ได้สำเร็จ
