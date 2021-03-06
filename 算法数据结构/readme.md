### [伪代码](https://zh.wikipedia.org/zh-hans/%E7%BB%93%E6%9E%84%E5%8C%96%E7%BC%96%E7%A8%8B) & 排序

​	结构化编程：一步步执行 至上而下。

计算机程序 除了输入输出，进行两种操作 ： 1 赋值 2 运算

**[算法](https://zh.wikipedia.org/zh-hans/%E7%AE%97%E6%B3%95)**的特征： 输入 ， 输出， 明确性 ， 有限性 ，有效性

一般 先分析问题-----分析数据结构-----写算法----解决



分治的思想：分为一个个互相独立的模块去处理。

动态规划思想：局部最优解当整体最优解，若提供更多的相关信息，则算出再局部的最优

贪婪思想：比喻：短视，所能解决的方式就是最优的。

---

[冒泡排序](http://bubkoo.com/2014/01/12/sort-algorithm/bubble-sort/)：体育委员两两摸头法： 从左到右 i 与 i+1 比较，矮的放左边，高的放右边。至最后一个。 

&emsp;&emsp;&emsp;&emsp;再循环从开头第一个开始到n-1个 循环。

**两两比较找出最小的 按循环的次数 顺序放**



&nbsp;&nbsp;&nbsp;&nbsp;***选择排序***：体育老师一指禅法：指定第一个与剩余的比，最矮的站左边。第二次 第二个与 n-2比 最矮的站第二个位置 以此类推

**从第一个开始 与之后的所有数比较 拿出最小的 按循环次数 顺序放最小的**



&nbsp;&nbsp;&nbsp;&nbsp;***插入排序***：起扑克牌法： 抓第二张与第一张比高的站左边，抓第三张与手上两张比大则左，循环。

**按顺序取，与手上拥有的牌比较，排序，循环N-1次**



&nbsp;&nbsp;&nbsp;***基数排序***：强迫症收扑克 ： 取牌 按数字放数字坑里，然后按坑的顺序 取尽依次。

&nbsp;&nbsp;&nbsp;***快速排序***：1. &nbsp;取一个数，剩下的 比它大的放一边，小的放另外一边。2.&nbsp;左边按前面步骤循环一次，右边也是。3. 按步骤12循环

&nbsp;&nbsp;&nbsp;***随机快速排序***：避免运气很差的概率：取数 在剩余的里面随机取 而不是取固定的下标数。



##  数据结构

**链接**：www.visualgo.net

**哈希** ｛计数排序，[桶排序](http://bubkoo.com/2014/01/15/sort-algorithm/bucket-sort/)，[基数排序](http://bubkoo.com/2014/01/15/sort-algorithm/radix-sort/)｝

**队列** { 先进先出 ：push() shit() }

**栈**    ｛先进后出 push() pop()｝

**链表** ｛插入删除方便，改变指向即可｝ 数组需要将删除之后的下标+1 长度-1  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数组查询快方便  链表慢，要**从前往后找**。

**树**｛二叉树 满二叉树 完全二叉树  数组存树｝



----**哈希**：作为计数工具使用。形式为k:v对的数据结构即是Hash。

&nbsp;***&nbsp;&nbsp;&nbsp;计数排序***：&nbsp;安排一组数组 a [ ] 的 MAx值+1 个桶 ，从‘0’开始。然后按桶序存，每存一个该桶 "V" +1 （计数）。

取a.length 长度的次数按桶顺序取出数据。

&nbsp;&nbsp;&nbsp;&nbsp;***桶排序***：&nbsp;安排一个范围内的数据(如0~9)为一个桶，按数据的范围存入桶，存好后各自桶内进行快排或其他。

&nbsp;&nbsp;&nbsp;&nbsp;***基数排序***：&nbsp;安排10个桶（0~9），按数据个位数字放入对应桶，出桶，按百位数字进桶 循环。最后出桶。



#### 小结：

&nbsp;&nbsp;&nbsp;&nbsp;**空间 / 时间  不可兼得**。计数排序快但占空间，桶排序与之相反。 计数排序**缺点**：只能算 0 ~ +无穷 不能有小数或负数。

链表只需要改变引用

#### [树](http://bubkoo.com/2014/01/14/sort-algorithm/heap-sort/)：

层数：数字上小下大

深度：几层

节点个数：每一个Hash

每个哈希就是一个节点。没有儿子的就是叶子节点

[二叉树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%A0%91)：满二叉树：长满了叶子/二叉树 是节点数不大于二。

有 2^n -1个节点数

完全二叉树： 最后一层是满的 或者右边只有1/0个节点

数组存树：a = [1,&nbsp;&nbsp;&nbsp;2,3,&nbsp;&nbsp;&nbsp;&nbsp;4,5,6,7...]   分治法

**堆**：特殊树---儿子总比老子小

最大堆/最小堆 。 最大堆调整。

**[堆排序](https://www.cs.usfca.edu/~galles/visualization/HeapSort.html)**：最底部的最右边小树开始：进行最大堆调整即：比较俩子节点大小 大的与父亲相比，若大于父亲则父亲与其交换位置。

此层所有排完后，进行第二次 上一层的比较，若调整了父子则 往下在看调整的节点其俩儿子组成是否是最大堆。

循环至顶层时 所有都是最大堆后则将顶层与最下层最右边的一个子孙交换位置后删除该值（每次取出按顺序倒排），子孙当做顶层节点。

按初始的算法继续进行最大堆调整 循环以上步骤。即输出最后的排序结果。为从小到大的数组(正序则从大到小)

[B树](https://zh.wikipedia.org/wiki/B%E6%A0%91)、[红黑树](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)、[AVL树](https://zh.wikipedia.org/wiki/AVL%E6%A0%91)

-----------------end



