---
title: VLOOKUP函数
date: 2021-10-17 23:08:08
tags:
    - 表格数据关联
    - 多个表格之间快速导入数据
categories:
    - 办公软件
    - excel表格
    - VLOOKUP函数
keywords: ""
description:
top_img: 
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fgss0.baidu.com%2F-vo3dSag_xI4khGko9WTAnF6hhy%2Fzhidao%2Fwh%253D450%252C600%2Fsign%3D5330c8e1d009b3deebeaec6cf98f40b7%2F3b87e950352ac65c154609fafaf2b21192138af3.jpg&refer=http%3A%2F%2Fgss0.baidu.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1638096673&t=c0dac2e286046202bd9c5d0f14db1d8f
---
成都的公务员报名，每天都有两次公布报考缴费人数比例，这样可以有效避免报考人数扎堆。对我这种学渣，肯定是要选择报考人数少的。在选择的时候发现，一个一个的查找数据太麻烦，因为涉及到好几个表格。为了节省时间和方便比较那就要用到excel的vlookup函数公式了。

## 函数定义

> VLOOKUP函数是Excel中的一个纵向查找函数，它与LOOKUP函数和HLOOKUP函数属于一类函数，在工作中都有广泛应用，例如可以用来核对数据，多个表格之间快速导入数据等函数功能。功能是按列查找，最终返回该列所需查询序列所对应的值；与之对应的HLOOKUP是按行查找的。

这是百度百科的定义，我这学渣是看的云里雾里的，只能慢慢理解消化了。定义看不懂，会用就行，说不定用着用着就明白了。

## 参数定义

* Lookup_value 为需要在数据表第一列中进行查找的数值。Lookup_value 可以为数值、引用或文本字符串。当vlookup函数第一参数省略查找值时，表示用0查找。
* Table_array为需要在其中查找数据的数据表。使用对区域或区域名称的引用。
* col_index_num为table_array 中查找数据的数据列序号。col_index_num 为 1 时，返回 table_array 第一列的数值，col_index_num 为 2 时，返回 table_array 第二列的数值，以此类推。如果 col_index_num 小于1，函数 VLOOKUP 返回错误值 #VALUE!；如果 col_index_num 大于 table_array 的列数，函数 VLOOKUP 返回错误值#REF!。
* Range_lookup为一逻辑值，指明函数 VLOOKUP 查找时是精确匹配，还是近似匹配。如果为FALSE或0，则返回精确匹配，如果找不到，则返回错误值 #N/A。如果 range_lookup 为TRUE或1，函数 VLOOKUP 将查找近似匹配值，也就是说，如果找不到精确匹配值，则返回小于 lookup_value 的最大数值。应注意VLOOKUP函数在进行近似匹配时的查找规则是从第一个数据开始匹配，没有匹配到一样的值就继续与下一个值进行匹配，直到遇到大于查找值的值，此时返回上一个数据（近似匹配时应对查找值所在列进行升序排列）。如果range_lookup 省略，则默认为1。

## 结果出现#N/A的情况
参考[blog](http://www.360doc.com/content/19/1205/20/66005516_877693931.shtml)
* 查找的数值不存在，关联表中没有对应的数据
* 参数错误
* 数据源第二个参数没有使用绝对引用，使得选中区域范围发生变化
* 第一个参数或者第二个参数选中的区域存在空格，将空格替换
* 查找值第一个参数与查找区域第二个参数匹配的内容类型不一致
* 存在非打印字符


## 总结
* 在关联查询的时候一直出现#N/A，这是什么鬼？参数都是正确的啊！后面才知道原来第二个参数Table_array选中的范围第一列必须是第一个参数Lookup_value相匹配的列。第三个参数col_index_num的范围是第二个参数table_array选择的区域。
* 如果查询的关联数据不存在，我们可以使用 IFERROR 公式，做处理
> =IFERROR(参数1， 参数2 错误信息)
