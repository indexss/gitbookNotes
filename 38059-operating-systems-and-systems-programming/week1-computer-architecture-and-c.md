# Week1 Computer Architecture & C

### 冯·诺伊曼系统

从应用场景的角度来说，曾经计算机分为两种：

* Application Specific Computer
* General Purpose Computer

Application Specific Computer的bottleneck在于，Programming required major re-wiring.

为了改进这一点，冯·诺伊曼发明了von Neumann Architecture system，是stored-program computer， a computer that stores program instructions in memory.

VNA的优点就是Re-prrogramming doesn't requrire any hardware modifications

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328063807830.png" alt="" width="375"><figcaption></figcaption></figure>

#### CPU

由两个主要部件组成：Control Unit(CU) & Arithmetic and Logic Unit(ALU)

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328064026497.png" alt="" width="375"><figcaption></figcaption></figure>

ALU负责实施具体的运算。

CU负责：

* Retrieves the operands
* Ask ALU to compute symbol "\*" or "+" ..
* Store the result
* Jumps to next line

**ALU**

ALU是执行arthmetic和logical operations的电路的union。

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328065234890.png" alt="" width="563"><figcaption></figcaption></figure>

**CU**

负责程序运行阶段指令一条一条的执行。

**Register**

Register是靠近ALU的small storage elements。

register用于暂存计算中的数据

ALU可以很快从register中读取数据

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328065441478.png" alt="" width="375"><figcaption></figcaption></figure>

**register的优点**

VNA computer的三个主要部件中没有寄存器，可以用Memory去存数据。缺点就是内存的读写相比register的读写还是慢，且对于多次使用的元素，每次使用都要读取。用寄存器的话可以把多次要使用到的元素存在寄存器中，读取一次，而ALU从register中读data是0 time overhead的，所以更快。

**改进后CPU**

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328065846884.png" alt="" width="375"><figcaption></figcaption></figure>

#### Memory

内存用于存both program instructions and data.

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328064409407.png" alt="" width="375"><figcaption></figcaption></figure>

对于程序员来说，我们将memory视为storage element

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240328070157342.png" alt="" width="375"><figcaption></figcaption></figure>

Memory由很多cells组成，每个cell能存一小段数据，每个cell都有自己的address。 E.g. 0 \~ N-1

在数据read/write操作发生时，CPU生成要存/取的内存地址，Memory Management Unit(MMU，一个CPU模块)从对应的地址读写内容。

C语言中，用指针来操纵内存。

#### I/O interfaces

I/O用来接收和发送来自连接设备的信息。链接的设备（monitor, keyboard, mouse）叫作peripheral devices（外围设备）。

#### Run Program on VNA computer

```
 main(){
   readIO a;
   readIO b;
   c = a + b;
   store c;
   d = a - b;
   print d;
 }
```

1. 程序存在memory中
2. CU读到readIO，理解到数据a需要由用户输入
3. 用户从外围设备输入data，传到了ALU
4. b也传到了ALU
5. CU指挥ALU进行加运算，算出c
6. CU将ALU中计算得到的c拷贝到Memory
7. CU指挥ALU进行减操作，得到d
8. CU将ALU中计算得到的d发送到外围设备display

\
