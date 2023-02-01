# some important

+ CH1

  &nbsp;

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/20c5fadb-e998-43d8-85d6-4292c1d1099d/image.png#left =30%x)file to program CALL

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/665a0141-54ff-4a2c-a16d-d1b8a79dc1d2/image.png#center =100%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/5ef109e9-57ee-4a55-a910-378aac244d87/image.png#left =60%x)java 透過java compiler 編譯成bytecode，接著透過不同的JVM將bytecode直譯

  JITC 會將執行頻率高的bytecode組譯機器語言，下次再看到同樣的bytecode就可以直接從cache執行機器語言

  ## compile three step

  * register assignment (NPC)

  * Translation

  * Optimization (NPC)

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/132c815e-1142-46e0-be42-e955f795cbb4/image.png)```webassembly
  not $t0,$t1 // not在mips用nor實現
  nor $t0,$t1,$zero
  
  swap $t0,$t1 // swap在mips用xor實現
  xor $t0,$t0,$t1
  xor $t1,$t0,$t1
  xor $t0,$t0,$t1
  ```

  immediate instruction 有16bit

  因為要考量正負號，所以實際上 能標得值為-2^15\~2^15-1

  R 6 5 5 5 5 6 add and sub srl sll

  I 6 5 5 16 lw sw beq bne addi

  J 6 26 j jal

  ## 注意位置

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/fdcff1de-ade5-4243-8fdc-b683408f2bef/image.png#left =70%x)## beq bne  跳躍範圍、計算

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/d11cc74d-333e-4315-8695-e3ae9af48ff5/image.png#left =80%x)


+ CH2
collapsed:: true

+ CH3

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/f2468ddc-89bb-4fb8-90c1-d1c3e822adc1/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/93baa93c-00c8-4d84-b6e4-8411aba77e2a/image.png#left =70%x)Instruction time \= CPI \* cycle time

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/6a7bf7a7-31dd-46aa-959f-74c7c6dca47a/image.png#left =80%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/3d6a2f00-fc41-4f57-a8e8-14bac23c18ec/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/606afc9f-1736-4bad-a76f-fc48d65bc73c/image.png#left =50%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/6b2de9dd-10df-4c74-97e3-6a994bddc31d/image.png)&nbsp;


+ CH4

  ALU 中 如果運算結果非0的話， zero就會輸出1

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/b25abbfe-0184-48ba-a6ed-25512dfd31a1/5bNKHngl.jpg)

  | &nbsp; | I-Mem | Reg | Mux | Alu | D-Mem | Mux | Regs | 
  |---|---|---|---|---|---|---|---|
  | R | y | y | y | y | &nbsp; | y | y | 
  | LW | y | y | &nbsp; | y | y | y | y | 
  | SW | y | y | &nbsp; | y | y | &nbsp; | &nbsp; | 
  | branch | y | y | y | y | &nbsp; | y | &nbsp; | 
  | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | 
  


+ CH5

  S \= stage 

  N \= instruction count 

  T \= cycle time

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/fe8f5ee0-a13b-406a-b161-c97d34959976/image.png)two stage Memory load use panalty 要變2  其餘不變\
  two stage EXE load use penalty 變2 其餘變1

  &nbsp;

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/a04d09e4-a4fe-488b-9c18-7e9875b895a2/image.png#left =40%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/af8d1565-1f27-4f21-b3e9-76f856d136cf/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/e75626c6-a066-4608-9fb9-906c3b8e3cb4/image.png#left =80%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/8a48b488-78a8-41d0-9bbc-0c2b60e06e47/image.png#left =80%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/57139971-9e09-4bff-a5d0-19b11a7e48f8/image.png#left =50%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/a6cc2758-7319-4aef-a06a-1cc8cf8925c9/image.png#left =100%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/f30bfc42-6278-4ba7-a87d-201514413218/image.png#left =80%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/de216fbf-b915-4241-b78d-fdf484b1cff0/image.png#left =80%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/040e1508-9d67-4920-aaf8-cf62ea3a080a/image.png#left =80%x)ILP method base on sw or hw

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/fd03c694-00ee-4caa-bb67-3425ff1b0e56/image.png#left =100%x)


+ CH6

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/f5af75bb-12a9-45d4-a884-73de9982e1ce/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/b9a5f922-48e5-4d01-b1f4-865a2aec0943/image.png#left =50%x)&nbsp;

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/db524d0e-2984-4e00-b383-f358bac3dfc4/image.png)block address \= byte address / 一個區塊所包含的byte個數(白話說就是區塊大小，然後以byte為單位)

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/29ef2fd3-1254-4399-b5c9-2ecacc66a360/image.png)s

+ CH7

  access time \= seek time\+ 0.5\*RPS \+datamount/data transfer rate \+ control overhead

  data transfer rate \= RPS  *number of sector* \*sector size(如果題目沒有給的話這樣算)

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/e03a1b9c-7e42-448b-8264-cf643c8a8056/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/57987931-c2d9-43bc-abb2-7180cea2a795/image.png#left =70%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/8f9b696c-fe35-4267-932d-67b479479f7b/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/1a68b92d-6e1e-45b5-bcb9-fd5d5c1043eb/image.png)&nbsp;

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/29200992-8009-4611-9e8e-ac475947b669/image.png#left =70%x)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/ff78cd9c-d6d5-4a24-a264-25e8f0f577ff/image.png#left =30%x)8760 \= 一年的小時數

  RAID3 少量寫入效能低(沒有parallel)， 

  RAID4大少量都可以，parity是其效能bottleneck parity 

  RAID5透過把parity分散到所有硬碟改善

  RAID6可以修復第二個損壞的硬碟，但也因此有RADI5兩倍的硬碟

  DMA不適用於資料很小的時候，設定DMA controller 的overhead會傳輸benfit還大

&nbsp;

+ CH8

  GPU用來增加bandwidth

  + flynn’s taxonomy

    ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/ab833ae8-5500-4483-9b75-2d3512e9ecfc/image.png#left =60%x)* SISD(single instruction stream, single data stream)

    * SIMD

    * MISD(multiple)

    * MIMD

  &nbsp;

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/948e32c3-e0d5-4771-938d-6fce6cbce8a1/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/927d165a-d4c9-4212-be69-8caf9d853536/image.png)Snoopy/snooping:在UMA multiprocessors system會有有快取不一致的問題，snoopy用來使快取保持一致性。

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/afb0b9af-083d-4aa8-9abe-7b40740d9333/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/62a907f3-94ce-470e-9958-3c47b92b693e/image.png#left =70%x)&nbsp;

  ![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/03d3dc0c-7cb9-44b8-88de-5c6558e38a89/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/26de627d-4c41-43ff-bcd6-996bcf794f20/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/547a8226-a948-4863-8b6b-14c4a9b8c85e/image.png)![](https://media.heptabase.com/v1/images/ca2a135c-18dd-4239-bb15-4280ac069121/d5311d21-1f42-4d94-af77-b26dd43801b1/image.png)

&nbsp;

&nbsp;