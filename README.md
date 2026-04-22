# Semiconductor-Defect-Prediction-API
Predictive Quality Analytics for Semiconductor Manufacturing

โปรเจกต์นี้คือการพัฒนาระบบ Machine Learning แบบ End-to-End เพื่อทำนายและตรวจจับชิปที่เป็นของเสีย ในกระบวนการผลิตเซมิคอนดักเตอร์แบบ Real-time โดยวิเคราะห์จากข้อมูลเซนเซอร์ภายในเครื่องจักร จำนวน 590 ตัว

เป้าหมายหลักคือการดักจับความผิดปกติให้ได้เร็วที่สุดก่อนที่ชิ้นงานจะถูกส่งไปประกอบต่อ เพื่อลดต้นทุนความเสียหายและเพิ่มประสิทธิภาพในการผลิต

ประโยชน์ทางธุรกิจ
- Proactive Defect Detection: ตรวจจับของเสียล่วงหน้าผ่านข้อมูลเซนเซอร์ ช่วยโรงงานประหยัดต้นทุนจากการหลุดรอดของชิ้นงานที่ไม่ได้มาตรฐาน
- Data-Driven Root Cause Analysis: ช่วยวิศวกรโรงงานตีกรอบหาสาเหตุความผิดปกติของเครื่องจักรได้รวดเร็วขึ้น
- Seamless Integration: ออกแบบระบบให้อยู่ในรูปของ RESTful API ที่พร้อมเชื่อมต่อกับระบบควบคุมเครื่องจักร (MES/Equipment System) ในโรงงานได้ทันที

โครงสร้างระบบ (Machine Learning Pipeline)
ข้อมูลที่ใช้คือ SECOM Dataset* ซึ่งมีความท้าทายด้าน Imbalanced Data และ Missing Values สูงมาก กระบวนการพัฒนาโมเดลประกอบด้วย:
- Data Imputation: จัดการข้อมูลเซนเซอร์ที่สูญหาย (Missing Values) ด้วย SimpleImputer (Mean Strategy) เพื่อรักษาสภาพข้อมูลแบบ Time-series
- Data Standardization: ปรับสเกลข้อมูลเซนเซอร์ทั้งหมดให้อยู่ในมาตรฐานเดียวกันด้วย StandardScaler ป้องกันความลำเอียงของโมเดล
- Dimensionality Reduction (PCA): ลดมิติข้อมูลจาก 590 เซนเซอร์ ให้เหลือเฉพาะกลุ่มตัวแปรหลัก เพื่อลด Noise และเพิ่มความเร็วในการประมวลผล
- Handling Imbalanced Data (SMOTE): แก้ปัญหาข้อมูลของเสียที่มีเพียง ~6% ด้วยการสร้างข้อมูลจำลองแบบ SMOTE ในชุดฝึกสอนทำให้โมเดลเรียนรู้ Pattern ได้อย่างสมดุล
- Modeling & Evaluation: เทรนโมเดลด้วย Random Forest โดยโฟกัสการวัดผลที่ค่า Recall ของ Class 1 (Defect) เป็นหลัก เพื่อให้มั่นใจว่าโมเดลสามารถดักจับของเสียได้มากที่สุด

การติดตั้งและใช้งาน API (How to Run) **ข้อมูลติดตั้งอยู่ใน Jupyter notebook 
โปรเจกต์นี้ถูกนำไปใช้งานจริง (Deploy) ผ่าน FastAPI 
1. ติดตั้งไลบรารีที่จำเป็น:
2. สั่งเปิดเซิร์ฟเวอร์ FastAPI:
3. การทดสอบ (Testing via Swagger UI):
เมื่อเซิร์ฟเวอร์ทำงาน สามารถเข้าไปที่ http://127.0.0.1:8000/docs
เพื่อทดสอบยิงข้อมูลเซนเซอร์ (JSON Payload) เข้าไปที่ Endpoint POST /predict ระบบจะตอบกลับสถานะของชิ้นงานทันที:

ตัวอย่าง Response จากเซิร์ฟเวอร์:

JSON
{
  "prediction_status": "Fail",
  "defect_probability": "85.50%",
  "recommended_action": "ALERT: REJECT WAFER"
}

Karinthip S.

## แหล่งข้อมูล (Dataset Reference)
โปรเจกต์นี้ใช้ชุดข้อมูล **SECOM Dataset** ซึ่งเป็นข้อมูลจริงจากเซนเซอร์ในสายการผลิตชิ้นส่วนเซมิคอนดักเตอร์
**Source:** [Kaggle - SECOM Dataset](https://www.kaggle.com/datasets/paresh2047/uci-semcom)
ประกอบด้วยตัวแปรเซนเซอร์ 590 ตัว และป้ายกำกับ (Label) ที่ระบุสถานะของชิ้นงาน (Pass/Fail) 
