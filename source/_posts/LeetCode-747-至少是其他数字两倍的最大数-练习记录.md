---
title: LeetCode-747.至少是其他数字两倍的最大数-练习记录
date: 2020-04-13 17:02:32
tags:
  - LeetCode
  - 数组
  - easy
categories: 码不停题
---

一道十分简单但仍然学到不少的题目。
同样属于`数组`类，是int型数组的计算问题。

将问题抽象转换为一个关系式，然后用最巧妙的方式判断是否满足，就可以得到最优解。

<!--more-->

### 题目

**至少是其他数字两倍的最大数**

在一个给定的数组nums中，总是存在一个最大元素 。
查找数组中的最大元素是否至少是数组中每个其他数字的两倍。
如果是，则返回最大元素的索引，否则返回-1。

{% noteblock quote green %}
来源：力扣（LeetCode），详见👇
链接：https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others
{% endnoteblock %}

### 理解

最大元素至少是其它数字的两倍，那么 `最大元素 > 第二大元素的两倍` 。
所以把数组排个序，然后判断上面这个是否成立即可，后面的其它数字自然成立。

### 常规思路

知道Java有个sort方法，上手就是一个「直译」脑中的想法：

```Java
class Solution {
    public int dominantIndex(int[] nums) {
        if (nums.length == 1)
            return 0;
        if (nums.length == 2)
            return nums[0] >= nums[1] ? 0 : 1;
        int[] bNums = (int[]) Arrays.copyOf(nums,nums.length) ;
        Arrays.sort(nums);
        int sln = nums.length - 2;
        for (int i = nums.length - 2; nums[i] == nums[nums.length - 1]; --i) {
            sln = i;
            if (i == 0)
                break;
        }
        int res = -1;
        if (nums[nums.length - 1] >= 2 * nums[sln]){
            // 不用 binarySearch 方法：需要首先进行排序，这里会打乱原有下标
            // return Arrays.binarySearch(bNums, nums[nums.length - 1]);
            for (int i = 0; i < bNums.length; ++i){
                if (bNums[i] == nums[nums.length - 1]){
                    res = i;
                    break;
                }
            }
        }
        return res;
    }
}
```

开头照样是两个直接判断，也是防止后面 `length - 2` 取值的时候溢出。
其中 binarySearch 这个方法，使用之前需要先排序，所以只能手动查找了。

用自带的方法有点讨巧，复杂度是O(n)，用时很少：2ms。

但是定睛一看：

```
执行用时：2 ms ，在所有Java提交中击败了 14.90% 的用户
```

纳尼？排名这么后。看来排序之后再判断，使不得。

### 开始探究

其实还是一开始偷懒了，如果不排序，而是直接在遍历的时候确定最大元素和第二大元素，就会更快一点。
原因就是，排序了之后也还要遍历两次，不慢才怪 🤣

重新来，减少遍历次数：

```Java
class Solution {
    public int dominantIndex(int[] nums) {
        if (nums.length == 1)
            return 0;
        if (nums.length == 2)
            return nums[0] >= nums[1] ? 0 : 1;
        int[] bNums = Arrays.copyOf(nums, nums.length);
        Arrays.sort(nums);
        int res = -1;
        if(nums[nums.length - 1] >= 2 * nums[nums.length -2]){
            for (int i = 0; i < bNums.length; ++i){
                if (bNums[i] == nums[nums.length - 1])
                    res = i;
            }
        }
        return res;
    }
}
```

这次只遍历一遍，应该会快点了吧——执行——诶，怎么还是 2 ms ？
看来问题主要出在sort排序上。单纯的循环，多一遍既不会影响时间复杂度，也不怎么耗时（当然是数据量少的情况下）。
而且排序还会导致后面找原先位置的时候，得复制保留一个原样数组。这也增加了空间复杂度。

去掉排序试一下，直接通过遍历取两个值：

```Java
class Solution {
    public int dominantIndex(int[] nums) {
        if (nums.length == 1)
            return 0;
        if (nums.length == 2)
            return nums[0] >= nums[1] ? 0 : 1;
        int max = 0, secondMax = 0;
        int index = 0;
        for (int i = 0; i < nums.length; ++i){
            if(nums[i] > max){
                secondMax = max;
                max = nums[i];
                index = i;
            }else if(nums[i] > secondMax){
                secondMax = nums[i];
            }
        }
        return max >= 2 * secondMax ? index : -1;
    }
}
```

一次循环，解决问题。
就是一定要看清题目的测试用例范围，不然在大于等于以及边界这些细节上，会白白耗费精力。

提交结果：

```
执行用时：1 ms ，在所有Java提交中击败了 69.41% 的用户
```
嗯？难道还有更快的方法？

### 快枪手方案

名字是我瞎起的，因为实在是太快了哈哈哈。

```Java
class Solution {
    public int dominantIndex(int[] nums) {
        int length = nums.length;
        if (length == 1)
            return 0;
        if (length == 2)
            return nums[0] >= nums[1] ? 0 : 1;
        int max = 0;
        for (int i = 0; i < length; ++i){
            if(nums[i] > nums[max])
                max = i;
        }
        for (int i = 0; i < length; ++i){
            if(i != max && 2 * nums[i] > nums[max])
                return -1;
        }
        return max;
    }
}
```

来自官方解答。嗯...看起来简洁易懂多了。
而且在第二个判断时用了反向思维。有不满足条件的就直接返回 -1 ，否则循环完毕后返回 max 。
非常精妙，尤其比起第一个复杂的做法时。

提交效果自然是最棒的：

![最终结果](final.png)

### 结语

总结一下：

- 同时间复杂度下，循环次数多不一定速度就慢，还要看你在循环里的操作。
- 数组取长度同样有消耗，如果需要多次使用，最好用个临时变量储存起来，拿一小点空间换时间。
- 有时候反向判断可能是更直接更好的方法。

今天顺带把博客的主题升级了一下，体验了一次 merge upstream conflict 的感觉，手动解决所有冲突（自己魔改了过主题的代码）后，果然——跑不起来了。最后还是顺着报错提示和 issue 找到了问题所在，更改了错误的配置项。

开始做题阻力很大，几个小时才能彻底搞懂一道，手写通过。但是也有好消息，比如又有人想买我的源码2333，还有 [天意](https://www.coolapk.com/apk/com.nerosong.godknows) 继创造者日报推荐后，今天又被 [少数派](https://sspai.com/post/59981) 文章推荐啦 😊虽然是个小玩意，但用心做的东西能被别人体会称赞，还是很有成就感滴。

最后想说的是，不要因为怕麻烦或担心失败就拖着不动，如果是应该做的事情，努力做好就总能向成功更进一步。