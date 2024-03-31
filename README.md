# QEMU-of-Android
virtualization android

基于android模拟器方案的android双系统之opengl接口透穿方案

1、host_qemu --- 从qemu原生版本拉取的，拥有对接虚拟机系统内核的能力，收取虚拟机系统下发的opengl接口指令序列化数据，转发给OpenglRender；运行于原生系统。

2、OpenglRender --- 移植android 模拟器源码，执行opengl指令、显示绘制数据；运行于原生系统。

3、vm_kernel_qemupipe --- 移植android goldfish源码，虚拟机系统内核管道设备，透穿虚拟机系统内核，将数据下发给运行于原生系统的qemu(host_qemu)。




性能：

1、操作桌面、一般的app都是比较流畅的

2、操作复杂的游戏APP比较卡，会出现丢帧现象

3、使用gfxinfo工具检测性能，会看见15～20MS的延迟每一帧


完整过程：

1、需要实体机的代码，调通kvm特性

2、下载google android源码，最好是android 5.0 ，这份代码是vm android，内核使用goldfish版本

3、将qemu交叉编译到实体机上，尝试跑通android 虚拟机，这个过程有很多BUG要解，最好使用我提供的qemu版本，我在那上面解过BUG，且我的qemu是从标准qemu分支拉出来的，最重要的我这个qemu能穿透opengl接口，有GPU虚拟化方案和input虚拟化办法，并没有使用android模拟器的qemu，你们可以试一试android的qemu

4、将OpenglRender编出来，运行在实体机中，它通过socket与qemu通信，执行一系列的opengl操作，并将虚拟机显示在一张surface中，最终虚拟机界面会出现在实体机中

5、对android比较熟悉的人，会很快实现的，当时我实现这个的时候对android一窍不通，一步一步的摸索

6、android虚拟机双系统论文地址http://systems.cs.columbia.edu/projects/kvm-arm/

   各种设备虚拟化理论http://www.virtualopensystems.com/
