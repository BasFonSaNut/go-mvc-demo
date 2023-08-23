# go-mvc-demo
Project Sample ที่เราจะทำกัน คือ 
ิจะสร้าง
  หน้าลงรายการสินค้า เที่ยบเท่าฝั่ง Backoffice
    มีข้อมูล 2 table หนึ่ง table(เมื่อเป็น GORM ต่อไปนี้เราจะเรียก cluster) จะพยายามไม่ให้เกิน 3-4 fields เมื่อ GORM 
  หน้าแสดงสินค้าเพื่อขายหน้าร้าน
    มีข้อมูล 2 table หนึ่ง table(เมื่อเป็น GORM ต่อไปนี้เราจะเรียก cluster) จะพยายามไม่ให้เกิน 3-4 fields เมื่อ GORM 

Process/Work Flow
  หน้าบ้าน จำลอง ลูกค้า สั่งซื้อสินค้า => ระบบสร้างรายการการสั่งซื้อ และ generate ออกมาเป็น pdf ที่เรียกว่าใบสั่งซื้อ
  ใบสั่งซื้อประกอบด้วย 
              ข้อมูลบริษัท(จำลอง), มี QRCode เมื่อแสกนจากมือถือ วิ่งไปที่ ที่ตั้งบริษัท google map
              รายการสั่งซือ{รายการ,จำนวน,ราคา}, 
                มี QRCode prompt pay แสกนผ่าน มื่อถือ ด้วยแอฟธนาคารแล้วชำระเงินได้เลยด้วยจำนวนเงินที่ระุบไปให้แล้วในการสร้าง QR Payment
                มี Barcode หรือ Qrcode ของเลขที่พัสดุที่จัดส่ง เมื่อ แสกนผ่าน มื่อถือ ด้วย camera ธรรรมดา วิ่งไปยังหน้า สถานะของพัสดุ

Programming 
แยกเป็น 5 เฟส
#Phase1 วางโครงสร้าง (project structure) เริ่มด้วยทำส่วน backend ให้มั่นใจว่าหลังบ้านมีครบ function ครบทุกฟีเจอร์
#Phase2 วางโครงสร้าง ข้อมูล (data structure) ทำมัน 3 ค่าย เพราะ vote มาครบทุก product mysql,postgres,mongodb
        ด้วยเขียนเป็น (ORM) structure เหมือนกัน จะย้ายไปเป็นค่าไหนก็แค่เปลี่ยนแค่บรรทัดเดียว คือ driver
        ใครจะใช้ db ตัวไหน ก็แค่ส่ง parameter ไป switch case จากไฟล์กลางเอาจะเขียนเป็นไว้เป็น package service กลาง
        ชื่อ DBconnect() เรียกด้วย 
            services.DBconnect(1) # mysql
            services.DBconnect(2) # mongodb
            services.DBconnect(3) # postgress

        Connection String จะเก็บไว้ใน .evn ครบทั้ง 3 ค่าย 
            PGCNN = ......
            MYSQCNN = .....
            MONGOCDD = ....

        สร้าง function พื้นฐานในการประมวล DB SEARCH,ADD,EDIT,DELETE
        ทั้งหมดนี้จะอยู่ในส่วน MODEL ซึ่งจะมีทั้งหมด 3 ไฟล์ ตาม DB vendor ที่เรา ใช้ mysql,postgres,mongodb
        และแน่นอน สร้าง pacakge ไว้ในชื่อ dbvendor 
        เรียกด้วย 
            dbvendor.selectORM(1) # mysql
            services.selectORM(2) # mongodb
            services.selectORM(3) # postgress       
        ดังนั้นแรกประตูทางเข้าของเรา สมมติชื่อ app.go จะทำ AutoMigration ไว้ทั้ง 3 ค่าย ส่วนใครจะไป โมทีหลังให้เหลือ
        ค่ายที่ตัวเองถนัดค่ายเดียว ก็แค่ remark บรรทัดที่ไม่ต้องการก็พอ    
#Phase3 ว่าด้วย component มาสร้าง event(function) กัน 
#Phase4 ว่าด้วย route มาสร้าง เส้นทาง(ประตู) สู่ component กัน 
#Phase5 ว่าด้วย view มาสร้างหน้าตา(UI) ด้วย template playout ,typescript สไตล์ go กัน 
        ถ้าใครมี พื้นฐาน typescript สไตล์ {{ }} ใช้ได้เลย หรือ สาย <% %>  ก็ไม่ได้ต่างกัน เปลี่ยนแค่ tag 
        ทำแบบง่ายยังไม่ถึงขั้นระบบสมาชิก หรือ login เพื่อเข้าจัดการหลังบ้าน 
        UI หลังร้าน เพิ่ม,แก้ไข,ลบรายการสินค้า (ทำฟอร์มอย่างง่ายที่สุด key-in ธรรมดาเป้าหมายเพียงแค่จะนำเสนอ submit แล้ว
        จะวิ่ง ผ่าน route ไหน เพื่อไปที่ component(ทำอะไร),การทำอะไรนั้นถ้าเกี่ยวกับ db จะเรียกไปยัง models ยังไง )
                  ทำรายการยืนยันสั่่งซื้อสินค้า (ออก QrPayment,
                  ออก Qr หรือ Barcode เลขที่พัสดุ(ถ้ามี), 
                  ออก Barcode ที่ชี้มายังใบสั่งซื้อ เมื่อสแกนแสดงหน้า หรือใบสั่งซื้อตาม Qutoation-Id นั้น)  
        UI หน้าร้าน เพิ่ม,แก้ไข,ลบรายการสั่งสินค้า,submit สั่งซื้อสินค้า (ทำฟอร์มอย่างง่ายที่สุด key-in ธรรมดาเป้าหมายเพียงแค่จะนำเสนอ submit แล้ว
        จะวิ่ง ผ่าน route ไหน เพื่อไปที่ component(ทำอะไร),การทำอะไรนั้นถ้าเกี่ยวกับ db จะเรียกไปยัง models ยังไง )

ทุก Phase จะนำขึ้น github ทีละเฟส มีคลิปวีดีทำให้ดูว่า ได้ไฟล์ไปแล้วจะรันทำยังไง
ไม่มีการมานั่งสอนพิมพ์ไปพร้อมกันซึ่งจะเสียเวลา แต่จะมีลำดับของการทำสร้างไฟล์ไหนก่อน แล้วรันยังไงไว้ให้
โดยใช้ชื่อไฟล์ว่า Phase(เลขที่Phase)Note.txt, Clip สาธิตการรันหลังได้ repository ไปแล้ว Phase(เลขที่Phase)Clip.gif

การพัฒนาแนวทาง solid development principles เริ่มต้นเฟสจากหลังมาหน้า (backend=>frontend) 
นั่นคือ จาก backend->fronend เพื่อให้มั่นใจว่า
ทุกฟังก์ชั่นหลังบ้านรันได้ 100% เมื่อนำมาประกอบร่าง แล้วรันจาก frontend=>Backend error ใดๆ หากเกิด
นั่นคือเกิด ที่ middleware/middle tier/third party 

จบโปรเจคนี้ สามารถต่อยอดโปรเจคนี้สู่ microservice ได้เลย เพราะ MVC pattern ,project structure
เพื่อสนับสนุนการเปลี่ยน path สู่ microservice อยู่แล้ว
route 1 เส้น(node) ประมวลผลแค่ 1 งาน ,1 สามารถ duplicate ออกมากี่เส้นก็ได้ หากพบว่า workload ของงานนั้น concurency 
สูงกว่าเส้นเดียวจะรับไหว (หากมี พื้นฐาน deploy ด้วย docker+swarm อยู่แล้ว ปรับเป็น go-micro ได้เลย) ก็ สร้าง node เพิ่ม
ช่วง concurency น้อยก็ลด node ลง (สามารถเขียน bat ให้ทำจุดนี้แบบ auto ได้ด้วยตัวมันเอง)

การพัฒนา ด้วยโมเดล MVC เพื่อหนีปัญหาการลด performance อันไม่เป็นธรรม ของ Programmer ให้น้อยที่สุด
นั่นคือเขียนเพื่อสนับการ Deployment Phase 
รับโจทย์มา 5 ข้อ พลาด ข้อเดียว rollback ทั้งหมด โปรเจค (webpack style)
กับ
รับโจทย์มา 5 ข้อ พลาด ข้อเดียว อีก 4 เส้นยังรันได้สบาย 
performance ฝั่งไหนดีกว่ากัน ?
