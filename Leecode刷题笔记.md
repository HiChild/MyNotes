# Leecode刷题笔记

### 贪心算法

#### **455. Assign Cookies (Easy)**

标签：贪心算法，一次遍历

**题目描述**

有一群孩子和一堆饼干，每个孩子有一个饥饿度，每个饼干都有一个大小。每个孩子只能吃

最多一个饼干，且只有饼干的大小大于孩子的饥饿度时，这个孩子才能吃饱。求解最多有多少孩

子可以吃饱。

**输入输出样例**

输入两个数组，分别代表孩子的饥饿度和饼干的大小。输出最多有多少孩子可以吃饱的数

量。

Input: [1,2], [1,2,3]

Output: 2

在这个样例中，我们可以给两个孩子喂 [1,2]、[1,3]、[2,3] 这三种组合的任意一种

**题解**

这是一个非常典型的贪心算法的问题，我们考虑这道题最多的情况只需要满足：

1.尽量让饱食度低的小孩先吃

2.尽量给小孩吃大于等于小孩饱食度的最低值

由于题目所给的数组是乱序的，所以我们不难想到将两个数组进行排序。

算法流程如下

![image-20210522154822587](C:\Users\BigCool\AppData\Roaming\Typora\typora-user-images\image-20210522154822587.png)

代码如下

```java
public static int findContentChildren(int[] g, int[] s) {
    Arrays.sort(g);
    Arrays.sort(s);
    int i = 0, j = 0;
    int m = g.length, n = s.length;
    int cnt = 0;
    while (i < m && j < n) {
        if (g[i] <= s[j]) {
            cnt++;
            i++;
            j++;
        } else {
            j++;
        }
    }
    return cnt;
}
```

进一步简洁化代码，**cnt和i其实是一个值**

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int child = 0, cookie = 0;
        while (child < g.length && cookie < s.length) {
            if (g[child] <= s[cookie]) child++;
            cookie++;
        }
        return child;
    }
}
```

**思考：**

本体的**特点**如下

- 求最优解
- 可证明当前局部最优的情况下，全局是最优的
- 只返回数量
- 与原先数组顺序无关 （可以排序）

当我们遇到可以在局部进行求解最优而使得全局最优时，我们就可以使用贪心算法。

其次：我们还需要掌握使用代码模拟每一种情况的技能。

#### **135. Candy (Hard)**

标签：贪心算法，位置不可变，**正反两次遍历**

**题目描述**

一群孩子站成一排，每一个孩子有自己的评分。现在需要给这些孩子发糖果，规则是如果一

个孩子的评分比自己身旁的一个孩子要高，那么这个孩子就必须得到比身旁孩子更多的糖果；所

有孩子至少要有一个糖果。求解最少需要多少个糖果。

**输入输出样例**

**示例 1：**

```java
输入：[1,0,2]
输出：5
解释：你可以分别给这三个孩子分发 2、1、2 颗糖果。
```

**示例 2：**

```
输入：[1,2,2]
输出：4
解释：你可以分别给这三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这已满足上述两个条件。
```

**题解：**

我的题解

我们先考虑如果**评分都是升序排列**的情况，如[0,1,2,4,6,8];

> 为什么能想到先考虑升序排列的情况？
>
> 因为一开始我也没想到，所以在测试用例的时候发现了错误，经过检测发现算法是对升序有效的。在升序时，我们可以保证先考虑的是评分最低的元素。

在本题中，我们需要考虑到位置分别为 **i - 1, i , i + 1**三个位置，对于位置为i的元素来说只需考虑如下三种情况：

- ratings[i - 1] < ratings[i] && ratings[i] > ratings[i + 1] ,即i位置的元素比两边都大
- ratings[i - 1] < ratings[i]，即i位置的元素比左边的元素大，比右边的元素小
- ratings[i] > ratings[i + 1]，即i位置的元素比右边的元素大，比左边的元素小

这三种情况分别对应如下的处理

- res[i] = Math.max(res[i - 1], res[i + 1]) + 1; 即取左右两边最大的糖果数再加一
- res[i] = res[i - 1] + 1; 即取左边的糖果数 + 1
- res[i] = res[i + 1] + 1; 即取右边的糖果数 + 1

> 注：res数组为保存每个位置糖果数的数组

因为我们要考虑位置i上两边的元素，所以只能**先忽略数组的边界**，即下标分别为**0和n-1**的位置

等以上操作都处理完后，再对边界条件进行处理

- if (ratings[0] > ratings[1]) res[0] = res[1] + 1; 即如果0位置元素的评分大于1位置的评分，则在1位置的糖果数上+1
- if (ratings[n - 1] > ratings[n - 2]) res[n - 1] = res[n - 2] + 1; 即如果最后一个元素的评分比倒数第二个元素的评分高，则最后一个元素的糖果数为倒数第二个元素糖果数+1

所以我们循环的变量的区间也可以很明确的确定了下来，range为[1, n-2],[]为闭区间的意思

但是，以上的算法**会出现一些问题**，如给定数组[4,3,2,1] 

我们期望的res数组是[4,3,2,1],但经过我们的算法得到的却是[3,2,2,1]。

**为什么会出现这种问题呢？**

我们根据算法推导一遍后发现，是因为我们从大到小的话，**后面小的数字的糖果数一旦发生变化，那么我们前面的糖果数也应该做出相应的修改**,那么我们**应该**每一次**都从评分最低的地方开始发糖果**。

但对于这道题目来说，在不能排序的基础上，我们很难让每一次都从评分最小的地方开始排序。

但是，针对于这道题，如果我们是**升序的评分，那么算法正确**，降序就错误，我们只需要求**满足题意的最优解**

那么我们可以思考，将**遍历分为两次，一次从前往后，一次从后往前**，两次便利取最优解即可，即可保证算法的正确性。

也就是说，针对于这种对于**某一位置，前后大小关系对最优解有重要的影响**时，我们可以考虑正遍历一次，再反着遍历一次，取最优解。

**代码如下**：

```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        if (n < 2) return n;
        int[] res = new int[n];
        Arrays.fill(res,1);
        for (int i = 1; i < n - 1; ++i) {
            if (ratings[i - 1] < ratings[i] && ratings[i] > ratings[i + 1]) {
                 res[i] = Math.max(res[i - 1], res[i + 1]) + 1;
            } else if (ratings[i - 1] < ratings[i]) {
                res[i] = res[i - 1] + 1;
            } else if (ratings[i] > ratings[i + 1]) {
                res[i] = res[i + 1] + 1;
            }else{

            }
        }
        for (int i =  n - 2; i > 0; --i) {
            if (ratings[i - 1] < ratings[i] && ratings[i] > ratings[i + 1]) {
                 res[i] =  Math.max(Math.max(res[i - 1], res[i + 1]) + 1,res[i]);
            } else if (ratings[i - 1] < ratings[i]) {
                res[i] = Math.max(res[i - 1] + 1, res[i]);
            } else if (ratings[i] > ratings[i + 1]) {
                res[i] =  Math.max(res[i + 1] + 1, res[i]);
            }else{

            }
        }
        if (ratings[0] > ratings[1]) res[0] = res[1] + 1;
        if (ratings[n - 1] > ratings[n - 2]) res[n - 1] = res[n - 2] + 1;
        int sum = 0;
        for (int x : res) {
            sum += x;
        }
        return sum;
    }
}
```

**优化！！！**

**更优秀的题解：**

做完了题目 455，你会不会认为存在比较关系的贪心策略一定需要排序或是选择？

虽然这一道题也是运用贪心策略，但我们只需要简单的两次遍历即可：

把所有孩子的糖果数**初始化为 1**；

先从**左往右遍历一遍**，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的糖果数加 1；

再从**右往左遍历一遍**，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加 1。

通过这两次遍历，分配的糖果就可以满足题目要求了。**这里的贪心策略即为，在每次遍历中，只考虑并更新相邻一**
**侧的大小关系。**

在样例中，我们初始化糖果分配为 [1,1,1]，第一次遍历更新后的结果为 [1,1,2]，第二次遍历更新后的结果为 [2,1,2]。

因为我们两次遍历的区间分别是[1,n-1],和[n-2,0]所以边界条件的0和n-1都已经被考虑进去了，不需要在进行边界条件的赋值。

**代码如下：**

```java
  public int candy(int[] ratings) {
        int n = ratings.length;
        if (n < 2) return n;
        int[] res = new int[n];
        Arrays.fill(res,1);
        for (int i = 1; i < n ; ++i) {
            if (ratings[i - 1] < ratings[i]) {
                res[i] = res[i - 1] + 1;
            }
        }
        for (int j = n - 2; j >= 0; --j) {
            if (ratings[j] > ratings[j + 1]) {
                if (res[j] <= res[j + 1]) {
                    res[j] = res[j + 1] + 1;
                }
            }
        }
        //可替换为
      	// for (int j = n - 2; j >= 0; --j) {
        //     if (ratings[j] > ratings[j + 1]) {
        //         res[j] = Math.max(res[j], res[j + 1] + 1);
        //     }
        // }
        int sum = 0;
        for (int x : res) {
            sum += x;
        }
        return sum;
    }
```

**再优化并简洁代码：**

```java
public static int candy(int[] ratings) {
    int n = ratings.length;
    if (n < 2) return n;
    int[] res = new int[n];
    Arrays.fill(res,1);
    for (int i = 1; i < n ; ++i) {
        if (ratings[i - 1] < ratings[i]) {
            res[i] = res[i - 1] + 1;
        }
    }
   for (int j = n - 2; j >= 0; --j) {
            if (ratings[j] > ratings[j + 1]) {
                res[j] = Math.max(res[j], res[j + 1] + 1);
            }
        }

    //一句话写完对数组求和但是速度比较慢，大数据可能会比普通方法快
    //return Arrays.stream(res).reduce(0, Integer::sum);
    int sum = 0;
    for (int x : res) {
        sum += x;
    }
    return sum;
}
```

**思考**

本道题的**特点**如下：

- 在局部取最优解，可以证明在全局是最优解
- 求最优解，只返回数量
- 答案与原数组的**顺序有关** （不可排序）
- 只考虑 i - 1, i, i + 1位置的大小关系
- 需要我们**从值最小的元素**开始不断遍历。 （之后都是比此元素值大的元素，确保了一旦赋值就不需要重复修改）
- 数组可以采取**正反遍历，取最优解**。
- 只对某一个方向贪心，只考虑一次的大小关系

当我们对于某一数组，**需要从最大（最小）值开始遍历**时，且**求最优解**时，我们就可以使用**正反两次遍历**，并且**取最优解**的方式来进行代码的编写。即可相当于我们的遍历都是从最值开始。

#### **435. Non-overlapping Intervals (Medium)**

**题目描述**

给定多个区间，计算让这些区间互不重叠所需要移除区间的最少个数。起止相连不算重叠

**注意:**

1. 可以认为区间的终点总是大于它的起点。
2. 区间 [1,2] 和 [2,3] 的边界相互“接触”，但没有相互重叠。

**示例 1:**

```java
输入: [ [1,2], [2,3], [3,4], [1,3] ]

输出: 1

解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

**示例 2:**

```java
输入: [ [1,2], [1,2], [1,2] ]

输出: 2

解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
```

**示例 3:**

```java
输入: [ [1,2], [2,3] ]

输出: 0

解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
```

题解：

​	区间类的问题是我们刷题中经常见到的高频问题，对于区间类问题，我们就可以根据题目的条件，一般着手于区间的左右端点来进行。在本题中，我们要求出需要移除的**最少**区间数量，很明显这又是一道最优解的问题。换而言之，我们希望尽可能的将**尽量多**的数字区间**不重复**的放置在数轴上。那么我们让我们每次选择的右区间尽量小，就能让我们在数轴上剩下更多的空间，也就能放下更多的数字区间。因为我们需要**选最小的右区间**，所以我们不妨**对数字的右区间进行排序**，并记录当前合法的最小右区间的值，然后遍历排序后的每个区间：

- 如果当前区间的左区间 < 当前的合法右区间 : 说明已经出现重叠，记录并更新重叠总数
- 如果当前区间的左区间 > 当前的合法右区间, 且右区间大于当前合法的右区间：则更新当前的合法右区间

```java
public static int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1] - o2[1];
            }
        });

        int min = intervals[0][1];
        int cnt = 0;
        for (int i = 1; i < intervals.length; ++i) {
            if (intervals[i][0] < min) {
                cnt++;
            }else if (intervals[i][1] > min) {
                min = intervals[i][1];
            }
        }
        return cnt;
    }
```

优化,我们不难发现，当 **当前区间的左区间 > 当前的合法右区间** 这一条件**不成立**后，则另一条件必然成立。

```java
public static int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1] - o2[1];
            }
        });

        int min = intervals[0][1];
        int cnt = 0;
        for (int i = 1; i < intervals.length; ++i) {
            if (intervals[i][0] < min) {
                cnt++;
            } else {
                min = intervals[i][1];
            }
        }
        return cnt;
    }
```

**更优秀的题解**

​	在选择要保留区间时，区间的结尾十分重要：选择的区间结尾越小，余留给其它区间的空间
就越大，就越能保留更多的区间。因此，我们采取的贪心策略为，优先保留结尾小且不相交的区
间。

​	具体实现方法为，先把区间按照结尾的大小进行增序排序，每次选择结尾最小且和前一个选
择的区间不重叠的区间。我们这里使用 C++ 的 Lambda，结合 std::sort() 函数进行自定义排
序。

​	在样例中，排序后的数组为 [[1,2], [1,3], [2,4]]。按照我们的贪心策略，首先初始化为区间
[1,2]；由于 [1,3] 与 [1,2] 相交，我们跳过该区间；由于 [2,4] 与 [1,2] 不相交，我们将其保留。因
此最终保留的区间为 [[1,2], [2,4]]。

**思考**

本题的**特点**如下：

- 区间问题
- 求最优解
- 数组的顺序和题解无关 （可以排序）

在我们遇到**区间类问题**的时候，第一，可以在区间的左右端点入手，进行分析。第二，可以考虑按某一端点排序。

在我们遇到**最优解问题**的时候，第一，可以考虑贪心算法，第二我们可以考虑动态规划。

本题的贪心思想的巧妙之处就在于求右端点的最小值，以保证剩余的空间最大，可以放下更多的区间。

#### 练习

#### 605. 种花问题

**题目描述**

假设有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给你一个整数数组  flowerbed 表示花坛，由若干 0 和 1 组成，其中 0 表示没种植花，1 表示种植了花。另有一个数 n ，能否在不打破种植规则的情况下种入 n 朵花？能则返回 true ，不能则返回 false。

**示例 1：**

```
输入：flowerbed = [1,0,0,0,1], n = 1
输出：true
```

**示例 2：**

```
输入：flowerbed = [1,0,0,0,1], n = 2
输出：false
```

**题解：**

我们分析这道题的核心逻辑：相邻的花不能种在一起，即对于flowerbed[i]来讲，必须**flowerbed[i] = 0，flowerbed[i - 1] = 0, flowerbed[i + 1] = 0,**这三个条件**同时满足**才可以种花，如果可以种花，我们就把**flowerbed[i]置1**，**记录可以种花的数量**，再判断下一个位置。**我们的贪心策略为：遍历每一个位置，只要能进行种花的地方都种花。**

但我们需要考虑首尾两个边界条件，

- 当i=0时，没有i-1
- 当i=数组长度-1时，没有i+1

我们不需要进行特判，当i=0我们就忽略前面的位置，当i=数组长度-1,我们就忽略后面的位置。所以可以得到我们判断是否可以种花的条件如下

```java
flowerbed[i] == 0 
    && (i == 0 || flowerbed[i - 1] == 0) 
    && (i == flowerbed.length - 1 || flowerbed[i + 1] == 0)
```

至此我们的算法就呼之欲出了：循环遍历每一个位置，判断是否可以种花，如果可以，就让计数器++，最后返回计数器是否大于n即可；也可以每次可以种花让n--，最后判断返回 n <= 0

代码如下：

```java
class Solution {
   public boolean canPlaceFlowers(int[] flowerbed, int n) {
		for (int i = 0; i < flowerbed.length; ++i) {
            if (flowerbed[i] == 0 
                && (i == 0 || flowerbed[i - 1] == 0) 
                && (i == flowerbed.length - 1 || flowerbed[i + 1] == 0)){
                    flowerbed[i] = 1;
                    n--;
                }
        }
        return n <= 0;
	}
}
```

本道题的**特点**如下

- 与顺序有关，（不可排序）
- **边界值若不存在，则可忽略**，不用特殊处理
- 数组类问题。

在面对边界需要特殊判断的问题的时候，我们不妨可以考虑一下边界条件是否影响我们的解题，如果不影响的话，我们就不必对数组的边界值进行特判了。

#### **452. Minimum Number of Arrows to Burst Balloons (Medium)**

**题目**

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 points ，其中 points [i] = [xstart,xend] ，返回引爆所有气球所必须射出的最小弓箭数。

**示例 1：**

```java
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：对于该样例，x = 6 可以射爆 [2,8],[1,6] 两个气球，以及 x = 11 射爆另外两个气球
```

**示例 2：**

```
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
```

**示例 3：**

```
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
```

**示例 4：**

```
输入：points = [[1,2]]
输出：1
```

**示例 5：**

```
输入：points = [[2,3],[2,3]]
输出：1
```

**题解**

只要出现最多次的重叠，我们就可以用最少的箭扎破气球。对于一个气球的开始区间和结束区间，我们思考**什么情况下我们需要用另一只箭**？即当一个气球的开始区间，超过了我们上一个气球的结束区间，我们就必定需要用另一根箭。所以我们不妨**先将数组按终止位置排序，维护一个目前为止最靠后的气球终止位置endPos**，**一旦下一个气球的开始位置超过此位置，我们就将计数器+1，并修改endPos为当前气球的结束位置**，我们的**贪心策略**为:**让气球的位置为最少射箭数的的最大位置**，只要下一个气球小于等于这个最大位置，就说明我们可以用之前的箭来射爆气球。

**代码**

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) return 0;
        Arrays.sort(points, new Comparator<int[]>(){
            @Override
            public int compare(int[] point1, int[] point2){                
                if (point1[1] > point2[1]) {
                    return 1;
                } else if (point1[1] < point2[1]) {
                    return -1;
                } else {
                    return 0;
                }
            }
        });
        int minEnd = points[0][1];
        int cnt = 1;
        for (int i = 0; i < points.length; ++i) {
            
            if (points[i][0] > minEnd) {
                minEnd = points[i][1];
                cnt++;
            } 
        }
        return cnt;
    }
}
```

**本体的特点如下**

- 对顺序无关（可以排序）
- 求最优值
- 局部最优解即为全局最优解

思考：当我们**自定义排序时**，重写compare方法，要**注意数字的范围**，如果数字的范围很小的话，我们就可以直接返回 int1 - int2，但是如果数字的范围很大的话，如整个整型的数都有可能出现，那么如果我们直接返回两数相减的值就会**发生溢出**,或者我们可以使用Integer的compare方法;

####  763.**Partition Labels (Medium)**

**题目** 

字符串 `S` 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

 **示例：**

``` 
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
```

**题解1**

分析题目后，我们可以知道，在一个片段中的字母一定满足在本片段中，所有的字母出现的位置不会超过本片段中某个字母的最大结束位置，即我们通过一边循环统计每一个字母的开始位置和结束位置，形成一个二维的区间，我们要做的就是遍历字符串中的每一个元素，并维护一个最大可达结束位置，当元素的开始位置小于最大可达结束位置时，我们就更新最大可达结束位置，并令计数器++；如果当前元素的开始位置大于最大可达结束位置，那么说明已经是另一个区间了，所以我们将计数器存入List中,并将cnt置为1（置为1是因为我们当前已经遍历了一个字符了）,在最后我们将最后一段区间的个数也加入List中即可；

**代码**

```java
  public static List<Integer> partitionLabels(String s) {
        if (s == null || s.length() == 0) return null;
        Map<Character,int[]> map = new HashMap<>();
        List<Integer> res = new ArrayList<>();
//        记录字符串的开始和结束位置
        char[] chArr = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            if (!map.containsKey(chArr[i])) {
                map.put(chArr[i],new int[]{i, i});
            } else {
                int[] pos = map.get(chArr[i]);
                pos[1] = i;
                map.put(chArr[i], pos);
            }
        }

        int end = map.get(s.charAt(0))[1];
        int cnt = 1;
        for (int i = 1; i < s.length(); ++i) {
            int curStart = map.get(s.charAt(i))[0];
            int curEnd = map.get(s.charAt(i))[1];
            if (curStart <= end) {
                end = Math.max(end, curEnd);
                cnt++;
            } else {
                res.add(cnt);
                end = curEnd;
                cnt = 1;
            }
        }
        res.add(cnt);
        return res;
    }
```

**优化1** 

其实通过观察，我们不难发现，我们只需要记录每一个元素最后出现的位置，并维护一个最远可达位置即可。当我们遍历每一个元素当前出现的位置，当当前位置小于最远可达位置时，我们更新**最远可达位置**为**当前元素的最后位置和最远可达位置中的最大值**。并且我们因为只需要预处理当前的元素和它最后的位置，且**元素都是小写字母**，那么我们可以**使用一个大小为26的数组**来记录，会比HashMap快很多。

```java
class Solution {
    public static List<Integer> partitionLabels(String s) {
        if (s == null || s.length() == 0) return null;
        int[] map = new int[26];
        List<Integer> res = new ArrayList<>();
//        记录字符和结束位置
        for (int i = 0; i < s.length(); i++) {
            //遍历一遍后记录的即为元素的最大值
            map[s.charAt(i) - 'a'] = i;
        }

        int end = map[s.charAt(0) - 'a'];
        int cnt = 1;
        for (int i = 1; i < s.length(); ++i) {
            //i 就是 curStart
            int curEnd = map[s.charAt(i) - 'a'];;
            if (i <= end) {
                end = Math.max(end, curEnd);
                cnt++;
            } else {
                res.add(cnt);
                end = curEnd;
                cnt = 1;
            }
        }
        res.add(cnt);
        return res;
    }
}
```

速度比之前要快好多![image-20210529213436561](C:\Users\BigCool\AppData\Roaming\Typora\typora-user-images\image-20210529213436561.png)

优化以前的

![image-20210529213502277](C:\Users\BigCool\AppData\Roaming\Typora\typora-user-images\image-20210529213502277.png)

进一步优化

```java
public static List<Integer> partitionLabels(String s) {
        if (s == null || s.length() == 0) return null;
        int[] map = new int[26];
        List<Integer> res = new ArrayList<>();
//        记录字符和结束位置
        for (int i = 0; i < s.length(); i++) {
            //遍历一遍后记录的即为元素的最大值
            map[s.charAt(i) - 'a'] = i;
        }

        int end = 0;
        int start = 0;
        for (int i = 0; i < s.length(); ++i) {
            //i 就是 curStart
            int curStart = i;
            int curEnd = map[s.charAt(i) - 'a'];;
            if (i <= end) {
                end = Math.max(end, curEnd);
            } else {
                res.add(end - start + 1);
                end = curEnd;
                start = curStart;
            }
        }
        res.add(end - start + 1);
        return res;
    }
```

**最简形式**

我们也可以当i==end时就进行记录，还少进行一些条件判断。

```java
   public static List<Integer> partitionLabels(String s) {
        int[] last = new int[26];
        int length = s.length();
        for (int i = 0; i < length; i++) {
            last[s.charAt(i) - 'a'] = i;
        }
        List<Integer> partition = new ArrayList<Integer>();
        int start = 0, end = 0;
        for (int i = 0; i < length; i++) {
            end = Math.max(end, last[s.charAt(i) - 'a']);
            if (i == end) {
                partition.add(end - start + 1);
                start = end + 1;
            }
        }
        return partition;
    }
```

我有**一个惊人的发现**，当我们频繁需要使用循环时，将长度计算出来，要比调用str.length()方法快1ms秒；

本题的**特点**如下

- 和顺序有关 （无法排序）
- 求局部的最优解
- 可以近似为区间问题

在本问题中，我们通过**预处理**，得到每一个元素最后出现的位置，使得我们的时间复杂度降得很低，在很多时候，我们也可以使用这种与处理的方法

#### **122. Best Time to Buy and Sell Stock II (Easy)**

**题目**

给定一个数组 prices ，其中 prices[i] 是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 **示例 1:**

```java
输入: prices = [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**

```java
输入: prices = [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3:**

```
输入: prices = [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**题解**：

本题非常简单，我们的贪心策略：只要后一天比前一天高，就证明我们可以获利，那么我们就可以在前一天买，后一天卖。在局部最优的情况下，全局也是最优的。

**代码：**

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int res = 0;
        for (int i = 0; i < n - 1; ++i) {
            if (prices[i + 1] > prices[i]) {
                res += prices[i + 1] - prices[i];
            }
        }
        return res;
    }
}
```

**特点**

- 求最优解
- 局部最优即为全局最优
- 不需要考虑边界情况

**思考**：当我们局部最优不影响全局最优的时，我们就每一次都取局部的最优解即可。

#### **406. Queue Reconstruction by Height (Medium)**

**题目**

假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 正好 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 people 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

 **示例 1：**

```java
输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。
```

**示例 2：**

```java
输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

**题解：**

题目分析：题目有点长，但是意思其实很简单：所有男生都有一个原本的位置，并且使用了一个二维数组记录**男生的身高**和他们前面比他们高或者和他们一样高的**人数**，现在将他们的顺序打乱，我们要做的就是根据每个男生的条件，将他们的顺序安排到合适的位置上。

**注意：**最高的人的数组的**第二个值一定为0**,因为不管他在哪里都不会有比他更高的人了

我们面对这种问题，我们可以按照数组的**第一个元素逆序排序，在按第二个元素正序排序**，排好序后，我们就可以根据前面有多少个比自己高的人**进行插队**了，因为按身高逆序排序，**当前元素前面的元素都是比他们自身高的**，所以我们**直接将当前的元素按照数组第二位的数字插在相应的位置**即可。即**先排序，再插队**。

本题的**贪心思想**为：先将人数从高到低排列，这样每个人当前的位置下标（从0开始），即为前面有多少个比自己高的人。然后再根据题目所给的条件，让矮个子的人向前插队到位置k（k即为前面比他个子高的人数），而**矮个子插入到高个子前面并不影响到高个子的位置**。

**代码**

```java
public int[][] reconstructQueue(int[][] people) {

    Arrays.sort(people, new Comparator<int[]>() {
        @Override
        public int compare(int[] o1, int[] o2) {
            return o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0];
        }

    });
    List<int[]> list = new ArrayList<>();
    for (int[] i : people) {
        list.add(i[1],i);
    }

    return list.toArray(new int[people.length][2]);
}
```

**优化** 

我们可以使用JDK1.8的lambda表达式来简写排序，

```java
/**
 * 解题思路：先排序再插入
 * 1.排序规则：按照先H高度降序，K个数升序排序
 * 2.遍历排序后的数组，根据K插入到K的位置上
 *
 * 核心思想：高个子先站好位，矮个子插入到K位置上，前面肯定有K个高个子，矮个子再插到前面也满足K的要求
 *
 * @param people
 * @return
 */
public static int[][] reconstructQueue(int[][] people) {
    // [7,0], [7,1], [6,1], [5,0], [5,2], [4,4]
    // 再一个一个插入。
    // [7,0]
    // [7,0], [7,1]
    // [7,0], [6,1], [7,1]
    // [5,0], [7,0], [6,1], [7,1]
    // [5,0], [7,0], [5,2], [6,1], [7,1]
    // [5,0], [7,0], [5,2], [6,1], [4,4], [7,1]
    Arrays.sort(people, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0]);

    LinkedList<int[]> list = new LinkedList<>();
    for (int[] i : people) {
        list.add(i[1], i);
    }

    return list.toArray(new int[list.size()][2]);
}

作者：pphdsny
链接：https://leetcode-cn.com/problems/queue-reconstruction-by-height/solution/406-gen-ju-shen-gao-zhong-jian-dui-lie-java-xian-p/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

本体的**特点**：

- 可以排序 （基本都要排序）
- 二维数组
- 第二个数组只有两个元素
- 插队类型

**思考**

在面对本类型的题目是，我们往往可以让**第一个元素逆序第二个元素正序排序，或者我们选择第一个元素正序，第二个元素来逆序排序**，往往可以降低我们解题的难度。并且，对于插队类型的题目我们可以利用**List插入的有序性**，以及**重载的add（int index,T element）**方法来实现**插队操作**，而不改变其他元素的相对位置。

#### **665. Non-decreasing Array (Easy)**

**题目**

给你一个长度为 n 的整数数组，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。

我们是这样定义一个非递减数列的： 对于数组中任意的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]。

 **示例 1:**

```java
输入: nums = [4,2,3]
输出: true
解释: 你可以通过把第一个4变成1来使得它成为一个非递减数列。
```

**示例 2:**

```java
输入: nums = [4,2,1]
输出: false
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。
```

**题解：**

**简单题却不简单**

本题的贪心思想：我们只对一个方向进行判断即，值判断当前位置i，位置i-1,和位置i-2。

我们**维护一个计数器cnt使其初始值为0**，当**(nums[i] < nums[i - 1])** 时，就说明我们**至少得改变一个元素**，故cnt++;

因为我们需要维护一个非递减序列，所以我们**优先考虑将i-1的元素变小，而不是令i位置的元素变大**（思考为什么），**即令nums[i - 1] = nums[i]**，但**如果i-2位置的元素大于i位置的元素**时,如 `i-2`, `i-1`,`i`分别为**[5，6，2]**，则我们**不能**改变nums[i-1]，**只能令nums[i]=nums[i-1]**。**其余情况下我们让nums[i - 1] = nums[i]即可**。边界情况使用`||`处理一下即可。

故得到的代码如下

**代码**

```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        int cnt = 0;
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i] < nums[i - 1]) {
                cnt++;
                if (i == 1 || nums[i - 2] <= nums[i]) {
                    nums[i - 1] = nums[i];
                } else {
                    nums[i] = nums[i - 1];
                }
            }
        }  
        return cnt <= 1;
    }
}
```

本体的**特点**

- 不可进行排序
- 相当于求最优解，cnt即为最少改变的次数
- 边界若不存在不影响结果，即可用`||`来处理边界条件
- 只考虑一个方向
- 题目要求维护**非递减数列**

对于本体我们应该有如下的**思考**

在本题中，我们再一次见到了使用`||`来**处理边界值**的情况，我们不难发现，只要边界值若不存在可忽略的情况下，我们就可以使用`||`来忽略对边界值的特殊处理。这里只有多看多练才能熟悉。

并且，在本题中我们又一次**只考虑了一个方向**，这类题我们以后可以经常见到。

### 玩转双指针

由于在java中没有指针，所以双指针一般指代的是元素的下标。并且我们一般也都是将双指针应用于线性结构的数据结构之中，如数组，链表。

常见的双指针玩法如下：

滑动窗口：两个指针向同一方向前进，但却不会相交

遍历：我们使用两个指针分别指向数组或链表的首位置和尾位置，然后以不同的方向，分别向中间逼近。

二分法：双指针的变体，可以在有序的数据结构之中每一次筛选掉一般的遍量，使得时间复杂度降低为logn;

#### **167. Two Sum II - Input array is sorted (Easy)**

给定一个已按照 升序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

**题目：**

给定一个已按照 升序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素

**题目描述**

在一个增序的整数数组里找到两个数，使它们的和为给定值。已知有且只有一对解

**示例 1：**

```java
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
```

**题解：**

本题目十分简单，因为所给的数组已经有序（升序），故我们可以让两个指针l，r分别指向开始和结束的位置，并求他们的元素之和，如果元素之和小于target，说明我们需要更大的元素，将l++,如果大于target则说明我们需要更小的元素，将r--.最后因为是下标从1开始，所以返回l+1和r+1即可。

**代码：**

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0,  r = numbers.length - 1;
        while (l < r) {
            if (numbers[l] + numbers[r] < target) {
                l++;
            } else if (numbers[l] + numbers[r] > target) {
                r--;
            }else {
                return new int[]{++l,++r};
            }
        }
        return new int[0];
    }
}
```

**题解：**

本题若无序的话也可以使用哈希表进行求解，我们将每一次的元素和下标存入hashMap中，在遍历时不断寻找target-当前的元素 是否在map中已经存在，若存在则返回map中的值和当前的下标即可。

**代码：**

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < numbers.length; ++i) {
            if (!map.containsKey(target - numbers[i])) {
                map.put(numbers[i], i);
            }else{
                return new int[]{map.get(target - numbers[i]) + 1, i + 1};
            }
        }
        return new int[0];
    }
}
```

**特点**：

- 升序数组
- 一维数组
- 遍历每一个元素即可，元素的大小有关系

**思考**

​	我们面对简单题的时候，我们可以多尝试一些方法，去进行思考。

#### **88. Merge Sorted Array (Easy)**

**题目**

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
```

**题解：**

首先我们可以看到数组1是的容量就是之后排序完数组的容量。如果我们新开辟一个数组的话，这道题目就很简单了，开辟一个和数组1大小相同的数组，然后在数组1和数组2有效的元素中比较，谁比较小就将谁放入新数组中即可。直到将所有的有效元素全部放入。

那么如果我们需要在原地的放入该怎么做呢？

数组的插入和移动的时间复杂度其实是非常高的，所以我们一般都会考虑覆盖的情况。我们发现在数组1中，后面长度为数组2长度的元素全是0，也就是无意义的，那我们就可以思考先从后往前覆盖元素。即从后向前遍历两数组，将其中的较大值从后面放入数组1中。此时会遇到一些情况

- 数组2从后往前已经全部放入，数组一还没有遍历到最前面：

  ​	其实着这种情况下，说明我们的数组1已经有序了，因为数组2的位置都在对的位置上，那么剩下的肯定就是数组1的元素，那么按序的顺序也是不变的。

- 数组1已经遍历完成了数组第一个元素，但是数组2还没有遍历到数组第一个元素：

  ​	这种情况下说明了数组二中剩下的整体的元素数组1中第一个元素小，所以我们将数组2中的**剩下的元素**对数组1从前到后进行覆盖即可。

**代码**：

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int idx = m + n - 1;
        n--;m--;
        while (n >= 0 && m >= 0) {
            nums1[idx--] = nums1[m] >= nums2[n] ? nums1[m--] : nums2[n--]; 
        }
        for (int i = 0; i <= n; ++i) {
            nums1[i] = nums2[i];
        }
    }
}

//简写

class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int idx = m + n - 1;
        n--;m--;
        while (n >= 0 && m >= 0) {
            nums1[idx--] = nums1[m] >= nums2[n] ? nums1[m--] : nums2[n--]; 
        }
        for (int i = 0; i <= n; ++i) nums1[i] = nums2[i];   
    }
}

```

**特点：**

- 有序
- 覆盖数组

思考：其实面对这一类型的题目，我们要想空间复杂度最优的话，我们就需要将插入和移动操作转化为覆盖操作。并且在覆盖的时候也要讲一些技巧，比如我们这道题后面的0值元素没有什么意义，我们就从后往前进行覆盖。之后再将剩下的元素从前往后进行覆盖即可

#### **142. Linked List Cycle II (Medium)**

**题目描述**

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

你是否可以使用 O(1) 空间解决此题？

**输入输出样例**

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

**题解：**

本题其实是一个数学公式题

```java
对于链表找环路的问题，有一个通用的解法——快慢指针（Floyd 判圈法）。给定两个指针，
分别命名为 slow 和 fast，起始位置在链表的开头。每次 fast 前进两步，slow 前进一步。如果 fast
可以走到尽头，那么说明没有环路；如果 fast 可以无限走下去，那么说明一定有环路，且一定存
在一个时刻 slow 和 fast 相遇。当 slow 和 fast 第一次相遇时，我们将 fast 重新移动到链表开头，并
让 slow 和 fast 每次都前进一步。当 slow 和 fast 第二次相遇时，相遇的节点即为环路的开始点。
```

**代码：**

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        if (slow == null || slow.next == null) return null;
        while (null != slow && null != fast) {
            slow = slow.next;
            fast = fast.next == null ? fast = fast.next : fast.next.next;
            if (slow == fast) {
                fast = head;
                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow;
            }
        }
        return null;
    }
}

//优化

public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;
        do {
            if (fast == null || fast.next == null) return null;
            slow = slow.next;
            fast = fast.next.next;
        } while (slow != fast);
        fast = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return fast;
    }
}
```

**特点：**

- 链表
- 求是否有环

#### **76. Minimum Window Substring (Hard)**

**题目描述**

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

**注意：**

- 对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 `t` 中该字符数量。
- 如果 `s` 中存在这样的子串，我们保证它是唯一的答案。

**输入输出样例**

示例 1：

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```

示例 2：

```
输入：s = "a", t = "a"
输出："a"
```

示例 3:

```
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

**解题思路**

求最小的覆盖子串，首先读题要清楚，我们要找到包含t字符串内所有字符的子字符串，但是不要求按照t字符的顺序；

也就是说，t字符串给出了两个条件

- 出现了哪些字符串
- 出现的字符串出现了多少次

所以我们要找到满足以上两个条件的s的子字符串，即s的**最短**子串要包含t串中所有的符号和符号出现的次数;

针对于这种问题我们不妨可以思考一下，最简单的思路就是不断找到每一个满足条件的子字符串，维护一个最短的子字符串长度，发现比它更短的就替换他;

我们定义两个指针，右指针不断向前滑动，左指针一开始不变，待左右指针之间的字符串符合条件后，就将左指针不断向前移动，但左指针移动会带来左右指针之间的字符串不符合条件

所以我们在移动左指针之前，必须要记录当前左指针的值。然后不断根据条件将左指针和右指针向右移动，直至遍历完全部字符串；

**时间复杂度O（n）**

**代码实现**

```java
public class MinOverlapSeq {

    public static void main(String[] args) {
        MinOverlapSeq minOverlapSeq = new MinOverlapSeq();
        String res = minOverlapSeq.minWindow("a", "aa");
        System.out.println("res -> :" + res);
    }
    // ABCASDFDAFG AFG
    // ABA AB
    public String minWindow(String s, String t) {
        char[] s2Char = s.toCharArray();
        int[] charButton = new int[128];
        boolean[] flags = new boolean[128];

        //init
        //初始化需将t出现的频率和是否出现记录下来
        for (char c : t.toCharArray()) {
            charButton[c]++;
            flags[c] = true;
        }

        //开始遍历 min_l记录目前的最大开始位置，l为左指针，记录一开始的位置，r为右指针不断向后探测；
        // cnt 记录目前探测到的字母数量 min_size记录我们需要返回的长度,+ 1是为了防止特殊边界情况出现，或者直接取Integer.MAX也是可以的
        int min_l = 0, l = 0, cnt = 0, min_size = s2Char.length + 1;
        for (int r = 0; r < s2Char.length; ++r) {
            if (flags[s2Char[r]]) {
                if (--charButton[s2Char[r]] >= 0) {
                    cnt++;
                }
                while (cnt == t.length()) {
                    if (r - l + 1 < min_size) {
                        min_l = l;
                        min_size = r - l + 1;
                    }
                    if (flags[s2Char[l]] && ++charButton[s2Char[l]] > 0) {
                        cnt--;
                    }
                    ++l;
                }
            }
        }
        return min_size > s2Char.length ? "" : s.substring(min_l, min_l + min_size );
    }
}
```

#### 练习

**633. Sum of Square Numbers (Easy)**

Two Sum 题目的变形题之一。 

**680. Valid Palindrome II (Easy)**

Two Sum 题目的变形题之二。
 **524. Longest Word in Dictionary through Deleting (Medium)**

归并两个有序数组的变形题。

#### 进阶

**340. Longest Substring with At Most K Distinct Characters (Hard)**

需要利用其它数据结构方便统计当前的字符状态。

### 第**4**章 居合斩!二分查找

二分查找也常被称为二分法或者折半查找，每次查找时通过将待查找区间分成两部分并只取 一部分继续查找，将查找的复杂度大大减少。对于一个长度为 *O*(*n*) 的数组，二分查找的时间复 杂度为 *O*(log *n*)。

举例来说，给定一个排好序的数组 {3,4,5,6,7}，我们希望查找 4 在不在这个数组内。第一次 折半时考虑中位数 5，因为 5 大于 4, 所以如果 4 存在于这个数组，那么其必定存在于 5 左边这一 半。于是我们的查找区间变成了 {3,4,5}。(注意，根据具体情况和您的刷题习惯，这里的 5 可以 保留也可以不保留，并不影响时间复杂度的级别。)第二次折半时考虑新的中位数 4，正好是我们 需要查找的数字。于是我们发现，对于一个长度为 5 的数组，我们只进行了 2 次查找。如果是遍 历数组，最坏的情况则需要查找 5 次。

我们也可以用更加数学的方式定义二分查找。给定一个在 [*a*, *b*] 区间内的单调函数 *f* (*x*)，若 *f* (*a*) 和 *f* (*b*) 正负性相反，那么必定存在一个解 *c*，使得 *f* (*c*) = 0。在上个例子中，*f* (*x*) 是离散函数 *f*(*x*) = *x*+2，查找4是否存在等价于求 *f*(*x*)−4 = 0是否有离散解。因为 *f*(1)−4 = 3−4 = −1 < 0、 *f* (5) − 4 = 7 − 4 = 3 > 0，且函数在区间内单调递增，因此我们可以利用二分查找求解。如果最后

二分到了不能再分的情况，如只剩一个数字，且剩余区间里不存在满足条件的解，则说明不存在 离散解，即 4 不在这个数组内。

具体到代码上，二分查找时区间的左右端取开区间还是闭区间在绝大多数时候都可以，因此 有些初学者会容易搞不清楚如何定义区间开闭性。这里我提供两个小诀窍，第一是尝试熟练使用 一种写法，比如左闭右开(满足 C++、Python 等语言的习惯)或左闭右闭(便于处理边界条件)， 尽量只保持这一种写法;第二是在刷题时思考如果最后区间只剩下一个数或者两个数，自己的写 法是否会陷入死循环，如果某种写法无法跳出死循环，则考虑尝试另一种写法。

二分查找也可以看作双指针的一种特殊情况，但我们一般会将二者区分。双指针类型的题， 指针通常是一步一步移动的，而在二分查找里，指针每次移动半个区间长度。

#### **69. Sqrt(x) (Easy)**

实现 `int sqrt(int x)` 函数。

计算并返回 *x* 的平方根，其中 *x* 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去

**示例 1:**

```
输入: 4
输出: 2
```

**示例 2:**

```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

**解题思路**

1.二分法

2.牛顿迭代法：牛顿迭代法，运用了高等数学线性逼近的思想，其核心思想为

若n^2 = x，我们猜一个数字i，则必有i * n/i = n; 而我们取（i + n/i） /  2 这个值作为新猜的值，继续猜，不断重复这个过程就会不断逼近真正的n，最后 i 和  n/i必然趋于相等; 我们可以利用java，double类型的精度损失来作为推出递归的条件;



**代码**

```
//i随便填非零值即可
private static double sqrt(double i, int x) {
		if (x == 0) return 0;
    double n = (x/i + i) / 2;
    if (n == i) return n;
    else return sqrt(n, x);
}

//更简洁的写法

class Solution {
    public int mySqrt(int x) {
        return (int) sqrt(x);
    }

    public long sqrt(int x) {
        long a = x;
        while (a * a > x) {
            a = ( a + x / a) / 2;
        }
        return a;
    }
}
```

#### **34. Find First and Last Position of Element in Sorted Array (Medium)**

#### 81. Search in Rotated Sorted Array II (Medium)

练习

**154. Find Minimum in Rotated Sorted Array II (Medium)**

**540. Single Element in a Sorted Array (Medium)**

进阶

**4. Median of Two Sorted Arrays (Hard)**

