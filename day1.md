# day1

​																																														时间：2022年4月6日13:05:17

## 一.自增运算符

<img src="D:\桌面\面试题\day1(22_4_6)\1自增运算符.png" alt="1自增运算符" style="zoom:67%;" />

## 二.单例

## 要点

**一是某个类只能有一个实例;(构造器私有化)**

**二是它必须自行创建这个实例;(类的静态变量保存这个实例)**

**三是它必须自行向整个系统提供这个实例;(public或者getter)**

### 1.饿汉式：直接创建，不存在线程安全问题

#### a.静态初始化（简介直观）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649220296398.png" alt="1649220296398" style="zoom:67%;" />

#### b.枚举(最简洁)

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649220321370.png" alt="1649220321370" style="zoom:67%;" />

```
使用
	由于是public，直接点运算符即可
两者的toString不同
    枚举的toString就是INSTANCE
    前者的toString自定义
```

#### c.静态代码块（适合复杂化实例，例如需要配置文件）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649220507116.png" alt="1649220507116" style="zoom:67%;" />

### 2.懒汉式：延迟创建对象

#### a.线程不安全（单线程）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649220618787.png" alt="1649220618787" style="zoom:67%;" />

```
instance必须是private，防止获取null
```

会出现的问题

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649220910285.png" alt="1649220910285" style="zoom:67%;" />



<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649220995348.png" alt="1649220995348" style="zoom:67%;" />

```
使用了sleep之后，直接让出cpu，导致其他线程进入
```



#### b.线程安全（多线程）

版本一（解决安全问题）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649221138092.png" alt="1649221138092" style="zoom:67%;" />

版本二（解决效率问题）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649221220976.png" alt="1649221220976" style="zoom:67%;" />

#### c.静态内部类（多线程，简洁一些）

<img src="C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1649221419897.png" alt="1649221419897" style="zoom:67%;" />