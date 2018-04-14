# Verilog Tutorial

首先以三个例子来介绍一下 `Verilog`。

```
// 一位全加器
module add1(a,b,cin,sum,cout);
    // 输入输出端口声明
    input a,b,cin;
    output sum,cout;

    // 内部信号声明
    wire q = a & b;
    wire g = a ^ b;

    // 功能定义
    assign sum = cin ^ g;
    assign cout = cin & g | q;

endmodule
```

```
// 二路选择器
module mux2(a,b,s,y);

    // I/O端口说明
    input s;
    input [7:0] a,b;
    output[7:0] y;

    // 功能定义
    assign y = (s==0) ? a : b;

endmodule
```

```
// 模16计数器
module counter16(clk,reset,d);
    input clk,reset;
    output[3:0] d;
    reg[3:0]    d;   // 声明d为寄存器类型
    
    always@(posedge clk) // 功能定义
        if(!reset)
            d <= 0;      // 复位
        else
            d <= d + 1;
endmodule
```

`Verilog`是一种类C的语言，所以通过上面三个例子差不多就能初步了解`Verilog`的语法了。



> 在Verilog中，wire永远是wire，就是相当于一条连线，用来连接电路，**不能存储数据**，无驱动能力，是组合逻辑,只能在assign左侧赋值，不能在always @ 中赋值；但reg可以综合成register，latch，甚至wire(当其只是中间变量的时候)，可以用于组合逻辑或者时序逻辑，能存储数据，有驱动能力，在always @模块表达式左侧被赋值。两个共同具有性质：都能用于assign与always @模块表达式的右侧。

-----

#### `Verilog` 与 `C` 的区别

> - Verilog: 描述硬件 语句并行执行
> - C语言： 描述逻辑算法 语句顺序执行


## Verilog 语法摘要

### 数字的写法

<位数>'<进制>数字

`_` 纯粹为了方便阅读

### Verilog 语法
- 常量 变量
- 运算符及表达式
- 语句 assign/ = / if-else / case / 语句块
- 实例原件语句
- 结构说明语句 always / initial

### 常量
- 数字
- `parameter`常量

### 变量
- wire型变量
	- 总线
- reg型变量
	- 定义： 在过程块中被赋值的信号
- memory型变量——reg数组
	- 定义： 有若干个相同宽度的reg型向量构成的数组  `reg[7:0] RAM[63:0];`

### 运算符

#### 拼接运算符

```
reg[0:7] a_8, b_8;
reg[0:15] a_16, b_16;
reg[0:19] a_20;

always...begin
	a_16 = {a_8, b_8};
	a_20 = {a_8[7:4], a_16};
	{a_8, b_8} = 16'haacc;
end
```

#### 优先级
建议使用括号

### Verilog 模块基本结构

- assign： 无论右边表达式操作数何时发生变化，右边表达式都会重新计算，并且给左边变量赋值

#### 实例元件或子模块语句

```
// 四路选择器
module mux4(d0, d1, d2, d3, s, y);

    input[3:0] d0, d1, d2, d3;
    input[1:0] s;
    output[3:0] y;

    wire[3:0] low, high;

    mux2 lowmux(d0,d1,s[0],low);
    ......

endmodule
```

### 结构说明语句

- always 块 （有触发条件）

- initial 块 （没触发条件，只在仿真零时刻执行一次）

更多关于`Verilog`尚未总结，可以查阅一下资料：

[Verilog.pptx](https://github.com/RayZhao1998/MIPSTutorial/res/Verilog.pptx)

[Verilog第二次课.pptx](https://github.com/RayZhao1998/MIPSTutorial/res/Verilog第二次课.pptx)