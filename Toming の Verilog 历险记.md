# Toming の Verilog 历险记

## 一、模块结构

### 模块介绍

```verilog
//1 端口定义
module module_name(端口名1,端口名2,...);
    
    //2 参数定义(可选)
    parameter 参数名 = 参数值;	//适用场景:例如定义信号位宽	
    						   //类似C/C++中的宏定义 #define
    
    //3 I/O说明
    //输入端口
    input 端口名1;
    input [信号位宽-1 : 0] 端口名2;
    ...;
    //输出端口
    output 端口名1;
    output [信号位宽-1 : 0] 端口名2;
    ...;
    //双向端口
    inout 端口名1;
    inout [信号位宽-1 : 0] 端口名2;
    ...;
    
    //4 内部信号声名
    //reg寄存器型
    reg[信号位宽-1 : 0] R 变量1, R 变量2 ......;
    //wire线型
    wire[信号位宽 1 : 0] W 变量1, W 变量2 ......;
    
    //5 功能定义
 	//assign
    //always
    //模块例化	//类似C/C++中类的实例化
endmodule
```

### 模块例化

```verilog
module and (C,A,B);
    input A,B;
    output C;
    //忽略
endmodule

//在下面的"and_2"块中对上一模块"and"进行例化,可以有两种方式：
module and_2(xxxxx);
    ...
	//方式一:实例化时采用位置关联，例如此处T3对应输出端口C,A对应A,B对应B
    and A1(T3,A,B);
    //方式二:实例化时采用名字关联，例如此处.C是and器件的端口,其与信号T3相连(推荐使用)
    and A2（
    	.C(T3),
    	.A(A),
    	.B(B)
    );
    ...
endmodule
```

