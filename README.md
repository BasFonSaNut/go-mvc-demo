# go-mvc-demo

Phage#1 ปูพื้นเล่นกับแพคเกจต่างๆ ที่เพียงพอต่อการทำ โปรเจค

    เราจะทำ web ขนาดเล็ก ที่สามารถ
    1. หลังร้าน : ทำรายการเพิ่มแก้ไขลบสินค้าในระบบได้,ค้นหารายการ โดยสามารถแบ่งหน้าได้
    2. หน้าร้าน : ลูกค้าทำรายการเพิ่มแก้ไขลบการสั่งซื้อส้นค้าได้, ค้นหารายการ โดยสามารถแบ่งหน้าได้
    3. หน้าร้าน และ หลังร้าน ดูรายการการสั่งซื้อได้ โดยสามารถแบ่งหน้าได้
    4. หลังร้านออก Invoice เป็น PDF, ได้ PDF สามารถ
            4.1 ใส่ QRPayment เพื่อลูกค้า สามารถ Scan ผ่านมื่อถือเพื่อชำระเงินได้
            4.2 ใส่ QRCode หรือ Barcode เพื่อ Tracking สถานะการส่งสินค้าได้
            4.3 ใส่ QRCode ของ link เพื่อไปยังหน้า Invoice ของ PDF นี้ได้
            4.4 ใส่ QRCode เพื่อ scan ด้วยมือถือ แล้ววิ่งไปยัง google map ดูที่ตั้งบริษัทได้
    พื้นฐานต้องมี
    1. เล่นกับ pdf pacakge เป็น
    2. เล่นกับ fakedata ค่าไหนก็ได้เป็น ใช้ใน Paginate(แบ่งหน้า) 
       เพื่อเราไม่ต้องมานั่งกรอกข้อมูล sample set ด้วยตัวเอง เพราะ 
       1.ไม่สมจริง
       2. ในการนำไปประยุกต์ใช้ในการทำงานจริง Demo App ให้ลูกค้า
          การนำเสนอ sample set ลักษณะ test1,test2...
          เป็นการแสดงออกถึงความชุ่ย นำเสนอภาพลักษณ์ไม่ดีต่อองค์กร
       3. เพื่อเด็กๆ สามารถมีฐานความรู้เบื้องต้นในการเรียกใช้ free data provider 
          เช่น น้องๆ จะทำโปรเจคเกี่ยวกับ data science หรือ machine learning 
          งานหนึ่ง งานนั้น ต้องมีข้อมูลเกี่ยวกับงานนั้นเพียงพอ และ มากพอ
          มีหลายองค์กร ที่เก็บข้อมูลส่วนนั้นไว้ให้เช่น google, หน่วยงานย่อย หรือแม้แต่
          โปรแกรมเมอร์บุคคลธรรมดา ที่ทำงานนั้นมาก่อน แล้วได้นำมาแชร์ใน github เพื่อเข้าถึงข้อมูล
          โดยเราแค่เรียนรู้การเรียกใช้เท่านั้น ก็ได้ข้อมูลที่นำมาต่อยอดงานได้ 
          ดังนั้น package นี้ จะปูพื้นเพื่อสิ่งนี้ไว้ให้

Phase#2 ว่าด้วย database + struct + gin + fiber

    2.1 NO ORM concept mysqlnative +struct general go lang datatype
        Middleware http-net

    2.2 ORM in Golang(GORM)+struct general go lang datatype
        Middleware gin

    2.3 ORM in Golang(GORM)+struct general go lang datatype
        Middleware fiber

    2.4 ORM in Golang(GORM)+struct json datatype
        Middleware fiber, ลง cors สักเล็กน้อย(ย้ายไปอยู่ใน Phase#2.3) 
        Json ย้ายไปอยู่ที่ Phase#3 ประกอบร่างด้วย json datatype ไปเลย
    จบเฟส 2
    - คาดหวัง Query ด้วย GORM เป็น   
    - ใช้ http-net,Gin,Fiber Framework เป็น
    - เล่นกับ Gin Context,Fiber Context,http-request,response เป็น 
    - เล่นกับผู้ให้บริการข้อมูล เป็น 
    - จบเฟสนี้ จะเลือกแบบไหนตามถนัดไม่ว่ากัน เพราะเป็นแค่ส่วนที่ทำยังไงให้ได้ข้อมูลมา 
      ได้มาแล้วก็ลง data structure กลาง ใครจะไปใช้ต่อยอดอะไร ด้วยเฟรมเวิร์คไหนก็ไม่ใช่ประเด็น
      เพราะ layer หลังจากนี้ ไม่ว่าจะใช้ http-net,gin หรือ fiber สุดท้ายก็เหมือนกัน เล่นกับ struct เป็นหลัก
    - Upload file เป็น (Phase 2.3 Fiber)
    - ติตดั้ง cors,และ set cors เป็น (Phase 2.3 Fiber, เตรียมไว้เพื่อ Phase#3 จะเล่นกับ json datatype เป็นหลัก)
    


Phase#3 ว่าด้วย การนำ Phase#1 และ Phase#2.1,Phase#2.2,Phase#2.3 มาประกอบร่าง 
        จนได้มาซึ่งเว็บแอฟตามเป้าหมาย ตัวอื่น ไม่ว่า http-net หรือ gin สอนให้ เพื่อเป็นโบนัสสำหรับคน อยากรู้เท่านั้น
        Phase#3 เป็นต้นไป เราจะใช้ fiber เป็นหลัก เพราะเป็น framework ที่เร็ว(อันดับ2)และดีที่สุด
        ของฝั่ง Golang (embedded และ สร้างโดย Go) ผ่านการทำ BenchMark มาแล้ว
        โดยจะเอา 3 ความรู้จาก Phase 1,2 มาประกอบร่าง ทำส่วนการสั่งซื้อ 
        การได้มาซึ่ง เอกสาร PDF ปลายทาง คือ จุดรวมความรู้ทั้งหมดที่ได้จาก คลาสนี้

        *เดิมจะแยก data struct ไปไว้ที่ Phase#2.4 
        ขอยุบรวมมาไว้ที่ Phase#3 เสียทีเดียวเพราะจะไม่มีอะไรทำ แค่เปลี่ยน struct เท่านั้น 
        
        *จบเฟส 3 คาดหวังว่าจะเล่นกับ web scrabble เป็น จำเสนอไว้ในการสร้าง data sample
        - วิ่งไปเอาภาพสินค้า,product description จากเว็บขายสินค้าสักเจ้า แล้วนำภาพที่ได้
        มา save ไว้ที่เว็บของเรา,รวมถึงลงในดาต้าเบส
        - ทำเอกสาร PDF เล่นกับ control,component(Row,Column) ได้อย่างเชี่ยวชาญ

Phase#4 ว่าด้วย การ apply Phase#3(MVC) สู่ Microservice

Phase#5 ว่า Elastic search ใน Golang 

Phase#6 ว่า go WASM(web assembly)

        - ทำไมต้อง WASM 
        - เปิดประเด็น งานที่เหมาะกับ WASM 
        - ปูพื้น TinyGo โลกของ iOT 
            - ทำไม ถึงหนี WASM ไม่พ้น ในโลก iOT ด้วย Golang
            - ทำไมต้องเหนื่อยขนาดนั้น เพียงเพื่อจะเข้าถึง javascript ในโลกของ iOT
        - อุปกรณ์ที่ใช้
            - Board ESP32 หรือ ESP8266 จะเสียบบน Breadboard หรือ ผ่าน Arduino ก็ได้ข้อมูลที่นำมาต่อยอดงานได้


การพัฒนา ด้วยโมเดล MVC เพื่อหนีปัญหาการลด performance อันไม่เป็นธรรม ของ Programmer ให้น้อยที่สุด

นั่นคือเขียนเพื่อสนับสนุนการ Deployment Phase 

รับโจทย์มา 5 ข้อ พลาด ข้อเดียว rollback ทั้งหมด โปรเจค (webpack style)
กับ

รับโจทย์มา 5 ข้อ พลาด ข้อเดียว อีก 4 เส้นยังรันได้สบาย 

performance ฝั่งไหนดีกว่ากัน ?

MVC Project Structure พร้อมสำหรับการเปลี่ยน Path สู Microservice Architech ได้ทันที
