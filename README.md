# QEI_F411RE_to_matlab_simulink

แบ่งเป็น 2 ส่วนคือ

1.Code F411RE

2.Simulink

## Code F411RE
### การ set ioc  

### internal clock setting
set internal clock ให้เป็น 100 MHz เพื่อง่ายต่อการคำนวณtimer

### timer 
timer ใช้ตัวอย่างนี้ใช้ timer 2 ตัว คือ

1.timer2 ใช้เป็น timer encoder ของ timer ตัวที่ 2 ของบอร์ด F411RE โดย 
- set period 2399 เนื่องจาก encoder ในตัวอย่างนี้ A=300 B=300 จะได้ (300*2)*4=2400 
        
- set polarity เป็นแบบ falling เนื่องจากในตัวอย่างเป็นแบบ falling

ซึ่งความหมายคือจะได้ counter 0-2399 

2.timer5 ใช้เป็น timer interrupt callback โดย 
- set prescaler 99
- set period 9999

ซึ่งความหมายคือจะได้การ interrupt callback 100Hz

### UART
Set UART2 เป็น 115200 8N1 และเปิดโหมด interrupt uart

### Flow_Code
- startTimer encoder mode ของ timer 2 
- start base Timer 5 แบบ interrupt เพื่อการ interrupt callback 100Hz
- function interrupt callback จะเป็นการส่ง UART counter จาก register ของ timer2
#### UART_Protocol
Format is ["I","m","@","c",high_byte,low_byte,"~"]

แบ่งเป็น 3 ส่วน 
- Header เป็น DEC=[73,109,64,99] หรือ ["I","m","@","c"]
- Data แบ่งเป็น high_byte กับ low_byte เนื่องจาก Uart ส่งครั้งละ 8 bits
- Terminator เป็น DEC=126 หรือ "~"

## Simulink
Block Serial Configuration
- Set port Com
- Set 115200 8N1

Block Serial Receive
- Set port Com
- Set data type Uint16
- Set header [73 109 64 99]
- Set terminator [126]


## วิธีนำไปใช้อย่างง่าย
- 1.Clone
- 2.คำนวณและตั้งค่า period ใน Timer2 ตาม spec encoder ใน ioc
- 3.ต่อสายencoderโดย A เข้า PA0 B เข้า PA1
- 4.upload code to F411RE
- 5.Set port com ใน simulink ทั้ง 2 block
- 6.เลือกความถี่ในการอ่านใน Simulink ที่ต้องการแต่ใน code ตัวอย่าง F411RE ส่งด้วยความถี่ 100 Hz
