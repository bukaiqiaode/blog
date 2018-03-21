# Bitmap在40亿整数中查找特定值

## 题目
给40亿个不重复的unsigned int，没有排序。然后给定一个数，如何判断这个数是否在40亿个里面？

## 分析
unsigned int，就算他是32位的吧，2 ^ 32 = 4 G个数字。使用Bitmap的话，一个字节Byte有8位可以用来控制8个数字。4 GB / 8 = 1 GB / 2 = 512 MB，需要占用的内存还算可接受，因此我们可以使用Bitmap来解决这个问题。

## 思路
用512 MB的内存，建立一个Bitmap数组。扫描那40亿个数，如果某个数字出现，就将Bitmap数组的对应位置赋值1。这样通过一轮扫描，就保存了40亿整数里面都有哪些数。
对于某个给定的数，检查Bitmap对应的位置，就知道这个数字是否在40亿个数字中出现过了。