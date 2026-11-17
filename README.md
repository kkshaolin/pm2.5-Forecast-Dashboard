# PM2.5 Forecast Dashboard

โปรเจกต์สำหรับเตรียมข้อมูลคุณภาพอากาศ ทดลองสร้างโมเดล Machine Learning เพื่อพยากรณ์ค่าฝุ่น และนำผลการพยากรณ์มาแสดงผลผ่านเว็บ Dashboard ด้วย Python

Repository ประกอบด้วย Notebook สำหรับวิเคราะห์ข้อมูล PM2.5/PM10 และไฟล์ `app.py` สำหรับสร้าง Dashboard

- วิเคราะห์และเตรียมข้อมูลคุณภาพอากาศจากไฟล์ Excel
- ใช้ข้อมูลเวลา ความชื้น อุณหภูมิ และค่าฝุ่นเป็นตัวแปรสำหรับการทดลอง
- ทดลองแนวทาง Regression ด้วย PyCaret
- แสดงค่าพยากรณ์ PM10 รายวันล่วงหน้า 7 วันผ่าน Dashboard

## เทคโนโลยีที่ใช้

| เทคโนโลยี | หน้าที่ |
|---|---|
| Python | ภาษาหลักของโครงการ |
| Pandas | อ่าน รวม และจัดการข้อมูล |
| PyCaret | ทดลองและเปรียบเทียบโมเดล Machine Learning แบบ Regression |
| Dash | พัฒนาเว็บ Dashboard |
| Dash Bootstrap Components | จัดวางองค์ประกอบหน้าเว็บ |
| Plotly Express | สร้างกราฟแสดงผล |
| Jupyter Notebook | สำรวจข้อมูลและทดลองโมเดล |
| Matplotlib | สร้างกราฟสำรวจข้อมูล |


### รายละเอียดไฟล์หลัก

- `pm_2_5.ipynb` — รวมข้อมูลจากไฟล์ Excel, ตรวจสอบชนิดข้อมูลและค่าว่าง, สำรวจการกระจายของตัวแปร และแบ่งข้อมูลตามเวลาเพื่อเตรียม Train/Test
- `pm10.ipynb` — Notebook สำหรับงาน PM10 (ไฟล์มีขนาดใหญ่ และไม่สามารถตรวจสอบเนื้อหาทั้งหมดได้ในการสำรวจครั้งนี้)
- `app.py` — สร้าง Dashboard โดยโหลดโมเดล `pm10_nofeture` และแสดงผลพยากรณ์ PM10 ในช่วง 7 วันข้างหน้า

## ข้อมูลที่ใช้

จากผลลัพธ์ที่บันทึกอยู่ใน `pm_2_5.ipynb` ชุดข้อมูลที่ผ่านการรวมมี 27,450 แถว ครอบคลุมวันที่ 16 สิงหาคม 2021 ถึง 11 กุมภาพันธ์ 2025 โดยมีคอลัมน์สำคัญดังนี้

| คอลัมน์ | ความหมาย |
|---|---|
| `timestamp` | วันและเวลาที่บันทึกข้อมูล |
| `humidity` | ความชื้น |
| `pm_2_5` | ค่าฝุ่น PM2.5 |
| `temperature` | อุณหภูมิ |

## กระบวนการทำงาน

### 1. Data Preparation

1. ค้นหาไฟล์ Excel ด้วย path `Data/pm-data/*1d.xlsx`
2. อ่านและรวมข้อมูลด้วย Pandas
3. ตรวจสอบชนิดข้อมูลและจำนวนค่าว่าง
4. เลือกคอลัมน์ที่เกี่ยวข้องกับการทดลอง
5. เติมค่าว่างในข้อมูลความชื้นและอุณหภูมิด้วยค่าเฉลี่ย

### 2. Exploratory Data Analysis

Notebook PM2.5 มีการสร้าง Scatter Plot เพื่อสำรวจการกระจายของอุณหภูมิ ความชื้น และค่าฝุ่น รวมถึงทดลองกรองช่วงค่าบางส่วนเพื่อดูการกระจายของข้อมูล

### 3. Model Experiment

Notebook แบ่งข้อมูลตามเวลา โดยกำหนดข้อมูล 7 วันสุดท้ายเป็น Test Set และข้อมูลก่อนหน้านั้นเป็น Train Set จากนั้นเริ่มตั้งค่า PyCaret สำหรับงาน Regression และเรียกขั้นตอนเปรียบเทียบโมเดล

### 4. Dashboard

ไฟล์ `app.py` ทำงานโดยสรุปดังนี้

1. โหลดโมเดลด้วย `load_model('pm10_nofeture')`
2. สร้างข้อมูลอนาคต 7 วันจากวันที่ปัจจุบัน
3. เตรียมตัวแปร `day`, `month`, `year`, `humidity` และ `temperature`
4. ใช้โมเดลทำนายและเก็บผลไว้ในคอลัมน์ `pm10`
5. แสดงข้อมูลบนหน้าเว็บด้วยการ์ด ตาราง และกราฟแท่ง

หน้า Dashboard มีองค์ประกอบหลัก:
- Today's Temperature
- Today's Humidity
- PM10 Prediction
- PM10 Forecast Table
- PM10 Levels Over 7 Days

**ข้อแตกต่างที่สำคัญ:** แม้ชื่อ Repository จะระบุ PM2.5 แต่ `app.py` ที่ตรวจสอบแสดงผล PM10 และโหลดโมเดล `pm10_nofeture` โดยตรง ส่วน Notebook `pm_2_5.ipynb` เป็นขั้นตอนทดลองข้อมูล PM2.5 แยกต่างหาก

## การติดตั้งและการใช้งาน

### 1. Clone Repository

```bash
git clone https://github.com/kkshaolin/pm2.5-Forecast-Dashboard.git
cd pm2.5-Forecast-Dashboard
```

### 2. สร้าง Virtual Environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

macOS / Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. ติดตั้ง Dependencies

ตัวอย่างแพ็กเกจหลักที่อ้างอิงจาก Imports ในโค้ด:

```bash
pip install pandas pycaret dash dash-bootstrap-components plotly matplotlib openpyxl jupyter
```

### 4. เตรียมข้อมูล

วางไฟล์ Excel ที่ Notebook ต้องใช้ไว้ใน:

```text
Data/pm-data/
```

โดยชื่อไฟล์ต้องตรงกับรูปแบบ `*1d.xlsx`

### 5. ทดลอง Notebook

```bash
jupyter notebook
```

เปิด `pm_2_5.ipynb` หรือ `pm10.ipynb` เพื่อสำรวจและทดลองขั้นตอนการเตรียมข้อมูล/โมเดล

### 6. เรียกใช้ Dashboard

```bash
python app.py
```

## Results

![PM Forecast Dashboard](image\result.jpg)

## แนวทางพัฒนาต่อ

- แยก Pipeline สำหรับ PM2.5 และ PM10 ให้ชัดเจน
- ทำให้ขั้นตอน Train, Evaluate และ Save Model สมบูรณ์
- เพิ่มการประเมินโมเดลด้วย Time Series Split หรือการทดสอบแบบ Walk-forward
- เพิ่มกราฟเปรียบเทียบค่าจริงกับค่าพยากรณ์
- เพิ่มการจัดการกรณีไม่พบไฟล์โมเดลหรือข้อมูลไม่ครบ
- เพิ่มการบันทึกประวัติการพยากรณ์และเวลาอัปเดตข้อมูล