# AIE314 Group Project: การวิเคราะห์ปริมาณการเดินทางและผู้โดยสาร (ชุดข้อมูลที่ 4)

**รายวิชา:** AIE314 Data Preparation and Data Mining
**กลุ่ม:** Loading Brain... 1%

| ชื่อ–นามสกุล | รหัสนักศึกษา |
|---|---|
| ธีรนัย ธีรภาพวงศา | 1670903515 |
| ธนภัค อัครธนาวัสส์ | 1670904190 |
| กัญญพัชร รอดคล้าย | 1670902111 |

---

## คำถามที่ตอบ

| ข้อ | คำถาม | Notebook |
|---|---|---|
| Q1 | ปริมาณการใช้บริการขนส่งแตกต่างกันอย่างไรระหว่างวันธรรมดาและวันหยุดสุดสัปดาห์ | `02_Q1_Weekday_Weekend.ipynb` |
| Q2 | ปริมาณผู้ใช้บริการช่วงต้นปี พ.ศ. 2569 เปลี่ยนแปลงจากช่วงเดียวกันของปี พ.ศ. 2568 อย่างไร | `03_Q2_YoY_Comparison.ipynb` |
| Q3 | จำนวนผู้โดยสารระหว่างประเทศขาเข้าและขาออกมีความแตกต่างกันอย่างไร | `04_Q3_International.ipynb` |

---

## โครงสร้างโฟลเดอร์

```
AIE314-Loading-Brain...-1percent/
├── data/                              ข้อมูลต้นฉบับ (ไม่แก้ไข)
│   ├── 4_passengers68.csv             ข้อมูลรายวันปี พ.ศ. 2568
│   └── 4_passengers69.csv             ข้อมูลรายวันต้นปี พ.ศ. 2569 (ถึง 11 มี.ค.)
├── notebooks/
│   ├── 00_Data_Understanding.ipynb    สำรวจโครงสร้างข้อมูลและบริการ
│   ├── 01_Data_Preparation.ipynb      ตรวจคุณภาพ ทำความสะอาด สร้าง feature และ export
│   ├── 02_Q1_Weekday_Weekend.ipynb    Q1
│   ├── 03_Q2_YoY_Comparison.ipynb     Q2
│   └── 04_Q3_International.ipynb      Q3
├── outputs/                           ไฟล์ที่ notebook สร้างขึ้น
│   ├── prepared_daily.csv
│   ├── q1_weekday_weekend.csv
│   ├── q2_yoy_by_method.csv
│   ├── q3_in_out_monthly.csv
│   └── figures/                       รูปกราฟทั้งหมด (notebook บันทึกให้อัตโนมัติ)
│       ├── prep_fig1_ratio_to_median_before.png
│       ├── q1_fig1_weekday_weekend.png
│       ├── q1_fig2_day_of_week_heatmap.png
│       ├── q1_fig3_monthly_pct_diff.png
│       ├── q2_fig1_group_yoy.png
│       ├── q2_fig2_service_pct_change.png
│       ├── q2_fig3_sensitivity.png
│       ├── q3_fig1_in_out_monthly.png
│       ├── q3_fig2_in_out_ratio_heatmap.png
│       └── q3_fig3_volatility.png
├── requirements.txt
└── README.md
```

---

## วิธีรัน

**ต้องมี:** Python 3.13 และ Jupyter (เช่น VS Code + Jupyter extension) แล้วติดตั้งไลบรารีจาก `requirements.txt`

```bash
pip install -r requirements.txt
```

**ลำดับการรัน (ต้องเรียงตามนี้):**

1. วางไฟล์ `4_passengers68.csv` และ `4_passengers69.csv` ไว้ในโฟลเดอร์ `data/`
2. เปิด notebook จากโฟลเดอร์ `notebooks/` เพราะทุกไฟล์อ้างข้อมูลด้วย path `../data/` และ `../outputs/`
3. รันทีละไฟล์ด้วย **Restart → Run All**

| ลำดับ | Notebook | อ่าน | เขียน |
|---|---|---|---|
| 1 | `00_Data_Understanding` | `data/*.csv` | ไม่มี |
| 2 | `01_Data_Preparation` | `data/*.csv` | `outputs/prepared_daily.csv`, `outputs/figures/prep_fig1_*.png` |
| 3 | `02_Q1_Weekday_Weekend` | `outputs/prepared_daily.csv` | `outputs/q1_weekday_weekend.csv`, `outputs/figures/q1_fig*.png` |
| 4 | `03_Q2_YoY_Comparison` | `outputs/prepared_daily.csv` | `outputs/q2_yoy_by_method.csv`, `outputs/figures/q2_fig*.png` |
| 5 | `04_Q3_International` | `outputs/prepared_daily.csv` | `outputs/q3_in_out_monthly.csv`, `outputs/figures/q3_fig*.png` |

ต้องรัน `01` ก่อนเสมอ ส่วน `02`–`04` อ่านข้อมูลจากไฟล์เดียวกันและไม่พึ่งกัน จะรันข้อไหนก่อนก็ได้ โฟลเดอร์ `outputs/figures/` จะถูกสร้างให้อัตโนมัติตอนรัน

**ฟอนต์ภาษาไทย:** ทุก notebook ตั้ง `plt.rcParams["font.family"] = "Tahoma"` (มีใน Windows) ถ้ารันบน macOS หรือ Linux ให้เปลี่ยนเป็นฟอนต์ไทยที่มีในเครื่อง ไม่อย่างนั้นตัวอักษรไทยในกราฟจะเป็นกล่องสี่เหลี่ยม

---

## สรุปการเตรียมข้อมูล (`01_Data_Preparation`)

| รายการ | ก่อน | หลัง |
|---|---:|---:|
| จำนวนแถว (รวมสองไฟล์) | 72,450 | 18,705 |
| แถวซ้ำ | 1 | 0 |
| ชื่อบริการสะกดผิด | 435 แถว | 0 |
| ค่าว่างใน `qty` | 444 | 1,215 |
| ค่า 0 | 793 | 34 |
| ค่าที่ยืนยันว่าผิด (outlier) | 2 | 0 (ตั้งเป็นค่าว่าง) |
| วันที่ยอดย่อยเกินยอดรวม (ทางด่วน) | 5 วัน | 0 (ตั้งเป็นค่าว่าง) |

ค่าว่างเพิ่มขึ้นเพราะค่า 0 ที่เป็น "ไม่มีการบันทึก" และค่าที่ยืนยันว่าผิด ถูกเปลี่ยนเป็นค่าว่างแทนการเดาค่าใหม่ ส่วนค่า 0 ที่เหลือ 34 ค่าติดธง `zero_unconfirmed` ไว้

---

## ไฟล์ผลลัพธ์ (`outputs/`)

| ไฟล์ | ขนาด | ใช้ตอบ | เนื้อหา |
|---|---|---|---|
| `prepared_daily.csv` | 18,705 × 18 | ทุกข้อ | ข้อมูลรายวัน (บริการ × วัน) ที่ทำความสะอาดแล้ว |
| `q1_weekday_weekend.csv` | 36 × 5 | Q1 | ค่าเฉลี่ยต่อวันวันธรรมดา/วันหยุด รายบริการ |
| `q2_yoy_by_method.csv` | 146 × 7 | Q2 | อัตราการเปลี่ยนแปลงรายบริการ × 4 วิธีจัดการค่าว่าง/ศูนย์ (`method` = A คือผลหลัก) |
| `q3_in_out_monthly.csv` | 48 × 8 | Q3 | ขาเข้า/ขาออกรายเดือน แยกตามรูปแบบการเดินทาง |

ทุกไฟล์บันทึกด้วย `encoding="utf-8-sig"` เปิดใน Excel แล้วภาษาไทยไม่เพี้ยน

### Feature สำคัญ

**`prepared_daily.csv`**
| Feature | ความหมาย |
|---|---|
| `date`, `year`, `month`, `day_of_week`, `day_name` | วันที่และส่วนประกอบ (`day_of_week`: 0 = จันทร์ … 6 = อาทิตย์) |
| `is_weekend` | 1 = เสาร์/อาทิตย์, 0 = จันทร์–ศุกร์ |
| `in_q2_window` | 1 = อยู่ในช่วง 1 ม.ค.–11 มี.ค. (ใช้เทียบสองปีใน Q2) |
| `direction` | ขาเข้า / ขาออก / ไม่ระบุ (ใช้ใน Q3) |
| `qty` | ปริมาณ (ตัวเลข) หน่วยตามคอลัมน์ `หน่วย` (คน หรือ คัน) |
| `ratio_to_median` | `qty` ÷ มัธยฐานของบริการนั้น (1 = วันปกติ) |
| `zero_unconfirmed` | True = ค่า 0 ที่ยังยืนยันไม่ได้ว่าจริงหรือไม่ |
| `outlier_status` | ปกติ / ยังยืนยันไม่ได้ / ยืนยันว่าผิด |
| `subset_error` | True = วันที่ยอดรถ 4 ล้อเกินยอดรถทุกประเภท (ทางด่วน) |

**`q1_weekday_weekend.csv`:** `avg_weekday`, `avg_weekend` (คน/วัน), `pct_diff` = (วันหยุด − วันธรรมดา) ÷ วันธรรมดา × 100

**`q2_yoy_by_method.csv`:** `avg_2568`, `avg_2569` (คน/วัน), `pct_change` = (2569 − 2568) ÷ 2568 × 100, `rank` (1 = เพิ่มมากสุด), `method` (A ข้ามค่าว่าง, B ศูนย์ที่ยังยืนยันไม่ได้เป็นค่าว่าง, C เติมค่าว่างด้วย 0, D เติมด้วยมัธยฐาน)

**`q3_in_out_monthly.csv`:** `avg_in`, `avg_out` (คน/วัน), `net_flow` = ขาเข้า − ขาออก, `in_out_ratio` = ขาเข้า ÷ ขาออก, `monthly_index_in`/`_out` = เฉลี่ยของเดือน ÷ เฉลี่ยทั้งปี (std ของค่านี้ = ความผันผวนภายในปี)

---

## ข้อควรรู้

- **หน่วยคนกับคันไม่รวมกัน:** Q1 และ Q2 ใช้เฉพาะบริการหน่วยคน (36 บริการ) ส่วน Q3 ใช้ 14 บริการระหว่างประเทศที่ระบุขาเข้า/ขาออก
- **ตัดบริการที่ข้อมูลไม่ครบ:** รถร่วม (หมวด 1) ไม่มีข้อมูลเลย ส่วนรถหมวด 3 และรถหมวด 4 ไม่มีข้อมูลตั้งแต่ ต.ค. 2568
- **หน่วยเป็นคน-เที่ยว:** ไม่ใช่จำนวนคนไม่ซ้ำ ผู้โดยสาร 1 คนที่เดินทางไปและกลับนับเป็น 2
- **ข้อมูลต้นฉบับในโฟลเดอร์ `data/` ไม่ถูกแก้ไข** การเปลี่ยนแปลงทั้งหมดอยู่ในโค้ดของ `01`