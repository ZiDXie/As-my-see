# PID控制

> *PID( Proportional Integral Derivative)*控制是最早发展起来的控制策略之一，由于其算法简单、鲁棒性好和可靠性高，被广泛应用于工业过程控制，尤其适用于可建立精确数学模型的确定性控制系统。
>
>   在工程实际中，应用最为广泛的调节器控制规律为**比例、积分、微分**控制，简称PID控制，又称PID调节，它实际上是一种算法。它适用于温度、压力、流量、液位等几乎所有现场，不同的现场，只要参数设置得当均可以达到很好的效果。均可以达到0.1%，甚至更高的控制要求。
>
>  但它的不足之处在于：在实际工业生产过程往往具有非线性、时变不确定，难以建立精确的数学模型，常规的PID控制器不能达到理想的控制效果；，由于受到参数整定方法烦杂的困扰，常规PID控制器参数往往整定不良、效果欠佳，对运行工况的适应能力很差。





### PID控制器各校正环节



#### 比例环节

> 成比例地反映控制系统的偏差信号e(t)，偏差一旦产生，控制器立即产生控制作用，以减小偏差。当仅有比例控制时系统输出存在稳态误差（Steady-state error）。

​                                                                                    <img src="https://all.czse7cxw.xyz/item/61ebc534184cd92e8ce62a51.png" alt="img" style="zoom:67%;" /> 

![img](https://bkimg.cdn.bcebos.com/pic/472309f79052982263522f37ddca7bcb0b46d4e4?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5/format,f_auto)

​                                                                                   $$上图展示一个稳态误差,可见随Kp增大，曲线始终保持着一个误差$$

P参数越大比例作用越强，动态响应越快，消除误差的能力越强。但实际系统是有惯性的，控制输出变化后，实际y(t)值变化还需等待一段时间才会缓慢变化。也正如上图所示，当Kp增大到2.7时，在数值首次到达1后仍然以较大的惯性最终达到了1.3附近。



#### 积分环节

>  控制器的输出与输入误差信号的积分成正比关系。主要用于消除静差，提高系统的无差度。积分作用的强弱取决于积分时间常数T,T越大，积分作用越弱，反之则越强。比例作用的输出与误差的大小成正比，误差越大，输出越大，误差越小，输出越小，误差为零，输出为零,引入积分环节的目的在于减小系统的稳态误差（静态误差）。

​                                       

​                                                                               <img src="https://all.czse7cxw.xyz/item/61ebc534184cd92e8ce62a4d.png" alt="img" style="zoom: 50%;" />



<img src="https://all.czse7cxw.xyz/item/61ebc534184cd92e8ce62a4f.png" alt="img" style="zoom:67%;" />

​                                                                         $$引入积分后的问题在于系统稳定裕度减小，曲线波动增大，许久后才会趋于平整$$

``` c
积分作用消除静差的原理是，只要有误差存在，就对误差进行积分，使输出继续增大或减小，一直到误差为零，积分停止，输出不再变化，系统的PV值保持稳定，y(t)值等于u(t)值，达到无差调节的效果。
```





#### 微分环节

>   反映偏差信号的变化趋势，并能在偏差信号变得太大之前，在系统中引入一个有效的早期修正信号，从而加快系统的动作速度，减少调节时间。在微分控制中，控制器的输出与输入误差信号的微分（即误差的变化率）成正比关系。



![img](https://i0.hdslb.com/bfs/album/4e581e20b87efae77a3ae44789b0abc038aed8d9.png@518w.webp)



引入微分环节的目的在于：比例作用和积分作用是事后调节(即发生误差后才进行调节)，而微分作用则是事前预防控制，即一发现y(t)有变大或变小的趋势，马上就输出一个阻止其变化的控制信号，以防止出现过冲或超调等。

![img](https://all.czse7cxw.xyz/item/61ebc7d5184cd92e8ce669e7.png)

​                                                                                     $$经过比例，积分，微分环节后，曲线呈现较好的变化趋势$$

K~d~越大，微分作用越强，K~d~越小，微分作用越弱。系统调试时通常把K~d~从小往大调，具体参数由试验决定。

``` c
注：微分作用可以在产生误差之前一发现有产生误差的趋势就开始调节，是提前控制，所以它的及时性更好，可以最大限度地减少动态误差，使整体效果更好。但微分作用只能作为比例和积分控制的一种补充，不能起主导作用，微分作用不能太强，太强也会引起系统不稳定，产生振荡，微分作用只能在P和I调好后再由小往大调，一点一点试着加上去。
```





### PID调试

> 根据被控过程的特性的不同，其所对应的比例系数，积分时间，微分时间



**PID手动整定(试凑法)**

通过被调参数变化的情况去调整参数，直到合适为止。

增大比例系数K~p~会加快系统响应速度，但过大会导致超调，甚至稳定性下降。减小积分系数T~i~会减小积分作用(越小则越强)，有利于减小超调与稳定误差，不过系统消除静态误差的速度将会下降。微分系数T~d~有利于加快系统响应，进行事前调节，减小超调，增加稳定性。但对干扰敏感，抗干扰能力差整定不当反而会有反效果。

由上述特点，分配各参数大小。

![img](https://all.czse7cxw.xyz/item/61ebd128184cd92e8ce743b2.png)





**PID衰减曲线法**

![img](https://all.czse7cxw.xyz/item/61ebd129184cd92e8ce743b7.png)

​                                                $$通过调节K~p~使图像的两个峰值达成4：1或10：1的比例，相应的K~i~，k~d~即可推导得出$$



①先把积分时间放至最大，微分时间放至零，使控制系统运行，比例度放至较大的适当值，“纯P降低比例度”，就是使控制系统按纯比例作用的方式投入运行。然后慢慢地减少比例度，观察调节器的输出及控制过程的波动情况，直到找出4:1的衰减过程为止。这一过程就是“找到衰减4:1”。
 ②对有些控制对象，用4:1的衰减比感觉振荡过强时，这时可采用10:1的衰减比。但这时要测量衰减周期是很困难的，可采取测量第一个波峰的上升时间Tr，其操作步骤同上。
 ③根据衰减比例度s和衰减周期Ts、Tr按表1进行计算，求出各参数值。

``` c
本方法对于变化较快的压力、流量、小容量的液位控制系统，但在曲线上读出衰减比有一定难度，因此在负荷变化比较大的时候，就需要重新整定
```





