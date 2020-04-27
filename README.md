 # สรุปเนื้อหาวิชา 
 # CN210 Computer Architecture
 ## สรุปการบ้านครั้งที่ 1
 ### MIPS Instruction format 
 MIPS คอมพิวเตอร์ชนิด RISC ผลิตโดย MIPS Technologies ซึ่งในทุกๆคำสั่งของ MIPS จะมีขนาด 32 bit ประกอบด้วย 3 ชนิดด้วยกัน คือ **R-format**,
 **I-format** และ **J-format**
 โดย **R-format** นั้นจะเป็นชุดคำสั่งที่เกี่ยวกับการคำนวณทางคณิตศาสตร์ เช่น คำสั่ง add เป็นคำสั่งเกี่ยวกับการบวก,คำสั่ง sub เป็นคำสั่งเกี่ยวกับการลบ เป็นต้น
 **รูปแบบคำสั่งของ R-format**
 
|**R-Format**|     |     |     |     |     |
|------------|-----|-----|-----|-----|-----|
|     op     | $rs | $rt | $rd |shamt|func |
|     6bit   | 5bit| 5bit| 5bit|5bit |6bit |

หรือ

|**คำสั่ง**|     |     |     |     |
|------------|-----|-----|-----|-----|
|ALU         | alu | $rd | $rs | $rt |

**I-Format** นั้นจะเป็นชุดคำสั่งที่เกี่ยวกับการโอนย้ายข้อมูล เช่น คำสั่ง lw ,sw เป็นต้น
**รูปแบบคำสั่งของ I-Format**

|**I-Format**|      |       |                 |   
|------  | -----| ----- | -----           | 
|   op   |  rs  |  rt   | value or offset |
|  6bit  | 5bit | 5bit  | 16bit           |

หรือ

|**คำสั่ง** |    |
----- | ----- |
|Data  Transfer    |  lw    $rt,offset($rs) |
|                  |  sw    $rt,offset($rs) |
|Branch            |  beq   $rs,$rt,offset |

**J-format** นั้นจะเป็นชุดคำสั่งที่เกี่ยวกับการ jump จาก Address ปัจจุบัน ไปยังอีกตำแหน่งหนึ่ง

|J-Format|    |   
----- | ----- | 
|op  | absolute address |
|6bit|      26bit       | 

หรือ

|คำสั่ง |    |
----- | ----- |
|Jump      |  j   address|
|Jump&Link  | jal   address|

ตัวอย่างขั้นตอนการทำงานแบบคร่าวๆของคำสั่ง **R-Format**
1.ทำการอ่านค่า op ซึ่งในที่นี้ op ของ r-format จะเป็น 000000
2.ทำการอ่านค่า func ในที่นี้ค่า func จะเป็นตัวบอกว่าจะต้องการดำเนินการทางคณิตศาสตร์ในรูปแบบใด เช่นถ้าเป็น add ก็จะเป็น 100000
3.จากนั้นจะทำการดำเนินการางคณิตศาสตร์ โดยจะนำค่าที่เก็บอยู่ใน $rs มาบวกกับ(add) $rt แล้วนำผลลัพธ์ที่ได้นำไปเก็บไว้ในค่า $rd

### ส่งการบ้านครั้งที่ 1
[คลิปงานครั้งที่ 1 อธิบายรูปแบบคำสั่ง R-FORMAT](https://www.youtube.com/watch?v=6F4AESota9A)

## สรุปการบ้านครั้งที่ 2
### การทำงานของ CPU
ในการทำงานกับคอมพิวเตอร์นั้น มนุษย์เราจะใช้ software ชุดคำสั่งภาษาในการติดต่อกับคอมพิวเตอร์ เช่นภาษา JAVA,C,Python เป็นต้น แต่ว่าเครื่องคอมพิวเตอร์ไม่เข้าใจภาษาของมนุษย์ คอมพิวเตอร์จะต้องทำการแปลงเป็นภาษาเครื่องเพื่อให้คอมพิวเตอร์เข้าใจ

<br>**คำสั่งที่มนุษย์เข้าใจ** 

    public class Test{
        public static void (String[]args){
              int a = 10;
              int b = 20;
              int c = a + b; 
        }
    } 

ภาษาข้างต้นเป็นภาษา JAVA คอมพิวเตอร์จะต้องทำการแปลงคำสั่งภาษาจาวาเหล่านี้ให้เป็นภาษาเครื่องเสียก่อน
<br>**คำสั่งที่คอมพิวเตอร์เข้าใจ**

                    === Machine Language ===

        00000000:           08400000        //j  01000000
        
โดยคอมพิวเตอร์ MIPS จะมีขั้นตอนการดำเนินคำสั่งตามตัวอย่างต่อไปนี้
เมื่อเริ่มทำการเปิดคอมพิวเตอร์ คอมพิวเตอร์จะมาที่คำสั่ง address 00000000 นั่นคือคำสั่ง 08400000 เป็นเลขฐาน 16 โดย MIPS จะทำการเปลี่ยนคำสั่งเลขฐาน 16 เป็นเลขฐาน 2 จะได้ 00001000010000000000000000000000 ต่อมาเมื่อคำสั่งเข้ามาใน Decoder จะทำการอ่านคำสั่ง 6 bit แรกก่อน ในที่นี้ก็คือ 000010 เป็น J-Format ส่วน 26 bit เหลือคือตำแหน่ง Address ที่จะทำการ JUMP เราจะทำการเติม 0 เข้าไปข้างหน้า 4 bit และเติม 0 เข้าไปข้างหลังอีก 2 bit ที่ 26 bit จะได้ Address ที่มีขนาด 32 บิท นั่นก็คือ Address 00000001000000000000000000000000(01000000) 

       
       00000004:           1A000000        //data
       
   *(Address ถัดมาจะลงท้ายด้วย 4 เนื่องจาก 1 คำสั่งมีขนาด 4 byte)*
คอมพิวเตอร์ได้ทำการ JUMP มาที่ Address 01000000 ซึ่งมีคำสั่ง 8C090004 ดังบรรทัดด้านล่าง

        ...

        01000000:           8C090004        //lw $9,$0(4)

คอมพิวเตอร์จะทำการแปลงคำสั่ง 8C090004 เลขฐาน 16 ให้เป็นเลขฐาน 2 ได้ 0001100000010010000000000000100 จากนั้น Decoder ทำการอ่าน 6 bit แรกคือ 000110 พบว่าเป็น I-Format เป็นคำสั่ง load word(lw) ส่วน 5 bit ถัดมาคือ 00000(คือ $rs=0) 5 bit ถัดมาคือ 01001($rt=9) 16 bit ถัดมาคือ 0000000000000100($offset=4) ซึ่งการทำงานของ lw นั้นคือการ นำค่า offset ไปบวกกับ $rs จะได้ค่า Address ที่ต้องไปดึงผลลัพธ์ข้อมูลออกมาอ่านและเก็บ ใน $rt

### ส่งการบ้านครั้งที่ 2
  [คลิปงานครั้งที่ 2 อธิบายหลักการทำงานของ CPU](https://www.youtube.com/watch?v=6wOGuS73R9k)
  
## สรุปการบ้านครั้งที่ 3
 **ความแตกต่างระหว่าง Single Cycle และ Multi Cycle**
  ![singlecycle](https://slideplayer.com/slide/4943101/16/images/5/Single+Cycle+Implementation.jpg)
 ##### ลักษณะของ Single Cycle มีดังนี้
    - เป็นการทำงานแบบ 1 คำสั่ง ต่อ 1 รอบ(cycle)
    - เป็น Harvard architecture
    - มี ALU 3 ตัว
    - มี 2 Memory
    - มี mux 4 ตัว
    - ทุกคำสั่งใช้เวลาเท่ากัน
    
![multicycle](https://people.cs.pitt.edu/~don/coe1502/current/Unit4a/fig548.jpg)
##### ลักษณะของ Multi Cycle มีดังนี้
    - 1 คำสั่ง อาจใช้หลาย cycle
    - เป็น Von neumann architecture
    - มี ALU 1 ตัว
    - มี mux 5 ตัว
    - แต่ละคำสั่งใช้เวลาไม่เท่ากัน

### ส่งการบ้านครั้งที่ 3
[คลิปงานครั้งที่ 3 อธิบายความแตกต่างระหว่าง Single Cycle และ Multi Cycle](https://www.youtube.com/watch?v=nJ20WSH9OTc)

## สรุปเนื้อหาการบ้านครั้งที่ 4
 **การทำงานของคำสั่ง lw ใน Multi Cycle**
 ![image](https://i.imgur.com/mWXHWpT.png)
**รูปแบบคำสั่ง lw $rt,offset($rs)**
  
  ขั้นตอนการทำงานคำสั่ง lw มีทั้งหมด 5 ขั้นตอนดังนี้
    
    1.เริ่มรับคำสั่งจาก Memory มาเก็บใน IR(IR = Memory[PC]) และนำ PC มาบวก 4 และนำไปแทนที่ใน PC เดิม(PC = PC + 4)
    2.นำค่าจาก $rs และ $rt ไปเก็บไว้ที่ A,B ตามลำดับ((A = Reg[IR[25-21]]) (B = Reg[IR[20-16]]))จากนั้นนำค่า offset 16 bit ผ่าน sing extend เพื่อแปลงเป็น 32 bit แล้วนำไปที่ ALU เพื่อบวกกับ PC แล้วนำไปเก็บที่ (ALUout = PC + (sign-extend(IR[15-0])<<2))
    3.นำค่าจาก A เข้ามาบวกกับ offset และนำค่าไปไว้ที่ ALUout (ALUOut = A + sign-extend(IR[15-0])
    4.ค่าที่ได้จาก ALUout คือ ค่าของ Address ของ Memory ที่จะถูกอ่านค่าออกมา (MDR = Memory[ALUout])
    5.นำค่าที่อ่านมาจาก Memory ไปเก็บไว้ใน $rt (Reg[IR[20-16]] = MDR)

### ส่งการบ้านครั้งที่ 4
 [คลิปงานครั้งที่ 4 การทำงานคำสั่ง lw ใน Multi Cycle](https://www.youtube.com/watch?v=9pEuQuDXgGQ)
 
 ## สรุปเนื้อหาการบ้านครั้งที่ 5
 **การทำงานของคำสั่ง beq ใน Multi Cycle**
 ![image](https://i.imgur.com/mWXHWpT.png)
 **รูปแบบคำสั่ง beq $rs,$rt,$offset**
  ขั้นตอนการทำงานคำสั่ง beq หรือ branch on equal มีทั้งหมด 3 ขั้นตอนดังนี้
    
    1.เริ่มรับคำสั่งจาก Memory มาเก็บใน IR(IR = Memory[PC]) และนำ PC มาบวก 4 และนำไปแทนที่ใน PC เดิม(PC = PC + 4)
    2.นำค่า $rs และ $rt ไปเก็บไว้ที่ A,B ตามลำดับ (A = Reg[IR[25-21]]) (B = Reg[IR[20-16]])
    3.นำค่าจาก A และ B มาเปรียบเทียบกัน หาก A==B จะเก็บผลลัพธ์ที่ได้ไว้ใน ALUout แล้ว JUMP ไปยัง Address ต่อไป หาก A!=B ก็จะข้ามไปทำคำสั่งถัดไปทันที

### ส่งการบ้านครั้งที่ 5
 [คลิปงานครั้งที่ 5 การทำงานของคำสั่ง beq ใน Multi Cycle](https://www.youtube.com/watch?v=xXbt_DfmJ3I&t=5s)

## สรุปเนื้อหาการบ้านครั้งที่ 6
### State Machine ของคำสั่ง R-Type
![image](https://image1.slideserve.com/3211244/the-four-stages-of-r-format-n.jpg)
 #### มีทั้งหมด 4 Cycle ด้วยกันดังนี้
 ### Cycle 1 Instruction Fetch
 ![stateno1](https://image1.slideserve.com/3211244/slide21-n.jpg)
 
 **จากรูปตัวหนังสือสีแดงคือคำสั่งที่ทำงานใน cycle นี้**

    MemRead = 1 คือ ทำการอ่านค่าจาก Memory
    IorD = 1 คือ เช็คว่า PC นั้นชี้ไปที่ Address ใดใน Memory
    IRWrite = 1 คือ นำค่าจาก Address Memory ที่ถูกชี้ ไปเก็บไว้ใน IR
    ALUSrcA = 0 คือ Mux เลือกค่าจาก 0 ซึ่งคือ PC ค่าที่ถูกเขียนใน IR จะมี ALUSrcA ทำการควบคุม
    ALUSrcB = 1 คือ Mux เลือกค่าจาก 1 ซึ่งคือ 4 ค่าที่ถูกเขียนใน IR จะมี ALUSrcB ทำการควบคุม
    ALUOP = ADD คือ การนำค่า PC มาบวกกับ 4
    PCWrite = 1, PCSource = 1 นำผลลัพธ์การคำนวณเขียนทับที่ PC = PC + 4
    
### Cycle 2 Decode & Register Fetch 
![image](https://image1.slideserve.com/3211244/slide23-n.jpg)

**จากรูปตัวหนังสือสีแดงคือคำสั่งที่ทำงานใน cycle นี้**
 
    ALUSrcA = 0 คือ Mux เลือกค่าจาก 0 ซึ่งคือ PC
    ALUSrcB = 3 คือ Mux เลือกค่าจาก 3 ซึ่งคือ Offset
    ALUop = 0 คือ ALUop จะทำการควบคุมคำสั่ง ADD
### Cycle 3 R-Format Execution
![image](https://image1.slideserve.com/3211244/slide25-n.jpg)
 
 **จากรูปตัวหนังสือสีแดงคือคำสั่งที่ทำงานใน cycle นี้**
 
     ALUSrcA = 1 คือ Mux เลือกค่าจาก 1 ซึ่งคือ $rs
     ALUSrcB = 0 คือ Mux เลือกค่าจาก 0 ซึ่งคือ $rt
     ALUop = 2 คือ ALUop จะทำการควบคุมคำสั่งให้เป็นไปตามคำสั่งใน IR
### Cycle 4 R-Format Write Register
![image](https://image1.slideserve.com/3211244/slide27-n.jpg)
 
 **จากรูปตัวหนังสือสีแดงคือคำสั่งที่ทำงานใน cycle นี้**
    
    RegWrite = 1 คือ นำค่าจาก ALUout มาเขียนใน $rd
    MemtoReg = 0 คือ Mux เลือกค่าจาก 0 ซึ่งคือ ALUout
    RegDst = 1 คือ Mux เลือกค่าจาก 1 ซึ่งคือ $rd

### ส่งการบ้านครั้งที่ 6
  [คลิปงานครั้งที่ 6 อธิบาย State Machine ของ คำสั่งชนิด R-Format](https://www.youtube.com/watch?v=mQHrFiMux10)    

## สรุปเนื้อหาการบ้านครั้งที่ 7
 ### Pipelining
**Pipelining** คือ หน่วยความจำที่อยู่ระหว่าง CPU และ Memory ทำหน้าที่เก็บคำสั่ง และลำดับคำสั่งโดยการนำคำสั่งที่ต้องทำงานถัดไปมาต่อรอคำสั่งที่กำลังทำงานอยู่ปัจจุบันโดยไม่ให้ CPU เกิดการว่างงาน ทำให้ CPU สามารถทำคำสั่งถัดไปได้เลยถึงแม้ว่าจะทำคำสั่งปัจจุบันยังไม่เสร็จแต่สามารถนำคำสั่งถัดไปมาทำงานต่อได้เลย สามารถทำให้ CPU ทำงานได้เร็วขึ้น
 
 **ในส่วนด้านล่างจะเป็นการเปรียบเทียบการทำงานระหว่างคอมพิวเตอร์ในรูปแบบปกติและ Pipelining**
 
 **คอมพิวเตอร์ในรูปแบบปกติ**
 
 ![image](https://cs.stanford.edu/people/eroberts/courses/soco/projects/risc/pipelining/laundry1.gif)
 
 จะเปรียบเที่ยบได้จากการที่เราซักผ้า ในคอมพิวเตอร์ปกตินั้น เมื่อเราเริ่มซักผ้ากอง A เราต้องซักผ้า เราต้องปั่นแห้งและเราต้องตาก ให้เสร็จเรียบร้อยก่อนถึงจะสามารถซักผ้ากอง B ต่อได้ จะอธิบายได้ว่าถ้าจะต้องการทำงานคำสั่งต่อไป เราจะต้องทำคำสั่งปัจุบันให้เสร็จเรียบร้อยก่อนถึงจะทำงานคำสั่งต่อไปได้ ซึ่งทำให้ใช้เวลานาน
 
 **คอมพิวเตอร์ที่ทำงานแบบ Pipelining**

![image](https://cs.stanford.edu/people/eroberts/courses/soco/projects/risc/pipelining/laundry2.gif)

คอมพิวเตอร์ที่ทำงานแบบ Pipelining นั้น เมื่อเราเริ่มซ๊กผ้ากอง A เมื่อซักเสร็จก็จะนำผ้ากอง A ไปปั่นแห้งต่อ แต่ทว่าเราไม่ต้องรอจนตากเสร็จ เราจะนำผ้ากอง B มาซักต่อหลังจากที่ซักผ้ากอง A เสร็จ แล้วเมื่อเราปั่นแห้งผ้ากอง A เสร็จ เราก็จะปั่นแห้งผ้ากอง B ต่อทันที เป็นเช่นนี้ไปเรื่อยๆ จะอธิบายได้ว่าการที่เราทำคำสั่ง A เสร็จในขั้นตอนใดๆขั้นตอนนึง เราสามารถนำ คำสั่งถัดไปมาดำเนินการต่อได้เลย ซึ่งจะเป็นการประหยัดเวลาในการทำงาน

### ส่งการบ้านครั้งที่ 7
 [คลิปงานครั้งที่ 7 Pipelining](https://www.youtube.com/watch?v=JuWSNlANVp8)
