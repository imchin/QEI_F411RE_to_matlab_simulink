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

1.timer2 ใช้เป็น timer encoder ของ timer ตัวที่ 2 ของบอร์ด F411RE
  โดย - set period 2399 เนื่องจาก encoder ในตัวอย่างนี้ A=300 B=300 
        จะได้ (300*2)*4=2400 
      - set polarity เป็นแบบ falling เนื่องจากในตัวอย่างเป็นแบบ falling
2.timer5 ใช้เป็น timer interrupt callback 
  โดย set prescaler 99
      set period 9999
      ซึ่งความหมายคือจะได้การ interrupt callback 100Hz

## Simulink
