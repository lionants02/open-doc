# การหา ITEM_CODE จากฐาน Super App

```mermaid
erDiagram
    NHSO_PAY_RESULT one to one CHARD : "1 ITEM_CODE ต่อ 1 LOCALCODE"

    NHSO_PAY_RESULT{
        varchar ITEM_CODE
    }

    CHARD{
        varchar LOCALCODE
    }
```
ค่าเดียวกันจะถูกใช้ใน 2 ฟิวด์ที่มีค่าเดียวกัน คือ
- ฟิวด์ `ITEM_CODE` จะถูกใช้ในแฟ้ม `NHSO_PAY_RESULT`
- ฟิวด์ `LOCALCODE` จะถูกใช้ในแฟ้ม `CHARD`

## สร้าง ITEM_CODE ในแฟ้ม NHSO_PAY_RESULT

**คอนเซปจะต้องทำการ link 2 ตารางนี้เข้าด้วยกันด้วยค่า `localcode` แต่ตอนส่งจะส่งด้วยค่า `stdcode_origin`**
```mermaid
flowchart TD
    s("start") --> pro1["link แมพค่าระหว่างตาราง\nm_serv_result และ m_serv_std"]
    pro1-->rename["เปลี่ยนชื่อฟิวด์ stdcode_origin เป็น ITEM_CODE"]
    rename-->scs["ส่งข้อมูลเข้าถังแดง"]
    scs-->e("end")
```


- `m_serv_result` ฟิวด์ส่วนใหญ่จะถูกแปลงค่าเป็น `NHSO_PAY_RESULT`
- `m_serv_std` ฟิวด์ส่วนใหญ่จะถูกแปลงค่าเป็น `CHARD`

```mermaid
erDiagram
    m_serv_result one to one m_serv_std : "1 m_serv_result ต่อ 1 m_serv_std"


    m_serv_result{
        varchar localcode "ใช้สำหรับ Lookup"
        varchar tran_no "ใช้สำหรับ Lookup"
        varchar stdcode_origin "จะถูกเปลี่ยนชื่อเป็น ITEM_CODE"
    }

    m_serv_std{
        varchar localcode "ใช้สำหรับ Lookup"
        varchar tran_no "ใช้สำหรับ Lookup"
        number reimbprice "จะถูกเปลี่ยนชื่อเป็น REIMB_UNIT_PRICE"
        number chargeamt "จะถูกเปลี่ยนชื่อเป็น AMOUNT_CHARGE"
    }
```