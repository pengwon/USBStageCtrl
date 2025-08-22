# USBStageCtrl

之前[这篇文章](https://mp.weixin.qq.com/s/8q9XC7vppK6EpHaf3VdBqA)简单说明了一个转台的设计，今天这个小板子就是被设计用来控制显微镜的载物台，通过USB连接PC，用户可以通过电脑软件来控制载物台的旋转和光源亮度。

板子支持USB-A和USB-C，A口板载USB转UART，上位机直接通过串口与之通信。C口则直连MCU，开发者可以通过USB-C口进行更高效的数据传输和控制。支持2路带编码器直流有刷电机，2路恒流源。

## 原理图

![主控](https://imgs.boringhex.top/blog/20250822105230.png)

主控采用STM32F103CBT6，当然用一些国产替代也完全没问题，这个系列的兼容芯片是最多的。

![USB-A](https://imgs.boringhex.top/blog/20250822105537.png)

MCU系统由USB供电，USB转串口使用了CH340，最高支持2Mbps的串口通信速率。DC005插座用于恒流源供电，这里用于驱动LED光源。

![恒流调光](https://imgs.boringhex.top/blog/20250822110139.png)

用运放搭建恒流源驱动LED光源，这个MCU没有DAC，使用PWM滤波得到直流信号，控制LED的亮度。实测效果良好，亮度调节范围广，LED无频闪，可以满足不同场景的需求。

但这个电路有一点小问题，大家可以在评论区留言讨论下。

![直流有刷电机驱动](https://imgs.boringhex.top/blog/20250822110612.png)

TI的DRV8837，性价比很高的芯片，未来跟国产芯片竞争，TI搞起价格战来也是够狠的！接口支持编码器反馈，方便实现闭环控制。接口适配下面这种微型减速电机：

![](https://imgs.boringhex.top/blog/20250822110936.png)

![TYPE-C](https://imgs.boringhex.top/blog/20250822111139.png)

最后是type-c接口，支持更高效的数据传输和控制。注意这里R1 R2的建议值是5.1k，我是因为库存4.7k，所以才使用了4.7k的电阻。
