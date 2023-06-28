# Virtual DOM
虚拟DOM是一个抽象的DOM表示，它是一个JavaScript对象，用来描述真实DOM的结构和内容。当React中的数据发生变化时，React会使用虚拟DOM来生成新的DOM树，并使用diff算法来比较新旧DOM树之间的差异，从而只对需要更新的部分进行操作，减少了对DOM的操作次数。

查阅[深入理解React虚拟DOM](https://www.cnblogs.com/yumingxing/p/9438457.html)