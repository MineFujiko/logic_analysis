# logic_analysis

> 本工程用verilog（FPGA）和python实现一个简单的8路输入逻辑分析仪；数据通过uart传入电脑通过python接收，再通过网页显示波形

## 基本功能
+ 本工程使用FPGA 为Altera的EP2C5T144C8N；当然也可以用其它类似芯片，只要能提供两个n*8bit FIFO即可，使用系统时钟为50MHz；
+ FPGA使用了两个Altera官方IP：n*8bit FIFO；n越大所能采集的数据越多；
+ 可以设置采样率50MHz~47.7Hz，分频值计算方法 = [4:0] << ([6:5] * 5),最后采样频率=50000/分频值(单位：KHz)；
+ 可通过uart发送命令，命令有:
    + 8'bxxxx_xxx1:设定采样频率
    + 8'b0000_0000:清除内部FIFO数据
    + 8'b0000_0010:设置发送重复次数
    + 8'b0000_0100:设置发送数据

## web页面操作
1. 选择采样率：由两部分(val1,val2)组成，选择后在下面显示采样率，采样率计算方式为50000/(val1*(2^(val2*5)))(单位KHz)
2. 采样点数：表示要采样的输入值的变化次数
3. 选择采样率和输入采样点数后可点击**开始**，即开始采集数据，多次点击则累计采样
4. 点击**清除**后可以清除绘制的波形
5. 可选择**单光标或多光标**
    + **单光标**：一条竖线跟随鼠标的光标
    + **多光标**：每单击一次产生一个光标，右键单击清除所有光标

## 使用python库：
+ webpy 0.37
+ pyserial 2.7
