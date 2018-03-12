最近看到一个面试题：<br/>
10亿个url,每个url不超过56B,有内存4G,要求去重.

其实这也是我在baidu面试的时候被问到过的一个题目。一别经年，又见面了，这次就把它做掉吧。

## 准备测试数据
10亿啊，哪里来这么多的url？自己造呗。因此第一个工作就是生成测试数据。不管算法好使不好使，都是要用到测试数据的。<br/>
上网查了下，url中的合法字符是有定义的。实际使用中，url还会包含一些斜杠(/)等可以用来减小问题难度的字符。<br/>
为了给自己增加难度，我们假设目标数据中没有什么能够用来降低问题难度的字符，每个url都实打实的用足了56B。<br/>
于是，问题转化为，有10亿个56B的字符串，请去重。我们的准备工作也就变成了如何生成56B的随机字符串。
```python
  # -*- coding: UTF-8 -*-
  import random

  def generate_url():
      '''
      Generate a url, length = 56B
      一个简单粗暴的生成56B字符串的辅助函数
      '''
      ret_val = ""
      char_pool = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_.~!*'();:@&=+$,/?#[]"

      lower_bound = 0
      upper_bound = len(char_pool) - 1

      while len(ret_val) < 56:
          index = random.randint(lower_bound, upper_bound)
          key = char_pool[index]
          ret_val = ret_val + key
      return ret_val
```
辅助函数，生成指定数目的随机url数组
```python
    def generate_data_group(num):
        arr_out = []
        counter = 0
        while counter < num:
            counter += 1
            arr_out.append(generate_url())        
        return arr_out
```
辅助函数，将url数组的内容写到指定的文件中
```python    
    import os
    
    def generate_data_file(arr_out, filename):
        if os.path.exists(filename):
            os.remove(filename)
        f_out = open(filename, "a")
        for item in arr_out:
            f_out.write(item + '\n')
        f_out.close()
        return
```
辅助函数，生成10个文件，每个文件包含1000w个随机的url
```python
    import time
    
    def prepare_data():    
        num = 1000 * 10000

        for item in file_names:
            arr_out = []
            start = time.time()
            arr_out = generate_data_group(num)    
            generate_data_file(arr_out, item)
            end = time.time()
            print "prepared: ", item , " in " , end - start

        return
```


## 降低问题规模
我们知道，1GB = 1024MB = 1024 * 1024KB = 1024 * 1024 * 1024B ~= 10^9 B<br/>
10亿个字符串，每个56B，那就 = 56B * 10 * 10^8 = 56 * 10^9 B,也就是56GB。<br/>
显然，题目中4GB的内容也就是按时，内存一次是放不下的，想想办法吧。<br/>
我们也显然不能真的用10亿个数据去测试，因此需要将问题划归为若干小问题，逐个解决。<br/>
我用上面的辅助函数测试了一下，生成10万个测试数据需要9s左右，还是可以接受的。
如果我们把问题化解为10w单位大小，那么通过对10w量级的数据进行操作，就可以估算出10亿级别的数据需要消耗的时间了。<br/>
10 * 10^8 = 10 * 10^4 * 10^4 = 10^4 * 10w,也就是需要将目标集合分成1w份。<br/>
假如，我们能够构建出一个hash函数，将问题集合分成1w个子集合，我们就可以得到一个10w数据量的子问题了。

## 构建hash函数
hash函数的选择是一个技术活，有很多思考的方向，但是最终的选择还是要结合数据源的特点，以及需求是什么。<br />
首先来看源数据：
- 如果源数据足够分散，重复程度比较均匀，我们就可以使用简单的 hash(key)/10000的方法来将它们映射出去。在均匀度好的情况下，得到的1w个文件大小合适。
- 如果重复程度比较高，例如10亿个url完全一样，那么任何hash都会产生另外一个10亿数据的大集合，就很困难了。这种情况下，第一轮不使用hash，而是简单的循环制定比较好。

### 源数据比较分散
这种情况下，我们可以设计一个hash函数，将url映射到1w个子集合上，然后逐个处理子集合。如果A和B是重复的，那么他们的hash值必然相同，进而它们将被存进同一个文件，在后续去重的时候就会被发现。如果A和B不重复但是hash值刚好相同，它们会进入同一个文件，但是在后续的去重中会被区分开。

### 源数据比较集中
我们可以粗暴些。
- 读取第一组10w个url，去重并排序后，写入第1个文件；
- 读取第二组10w个url，去重并排序后，写入第二个文件；
- 重复，直到10亿个url读取结束....反正最后都要归并的。这种粗暴的办法还可以减少IO的次数。<br />
- 再将去重后的1w个文件整体进行处理。例如先各自排序，然后采用归并排序的方法进行筛选，就将问题转化为了外部排序。
