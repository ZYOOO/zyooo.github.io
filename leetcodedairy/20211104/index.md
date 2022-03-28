# 2021-11-04




## 两个数组的交集

```java
给定两个数组，编写一个函数来计算它们的交集。

 

示例 1：

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
示例 2:

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
 

说明：

输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
我们可以不考虑输出结果的顺序。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

我的思路是先把一个数组存入hashmap中,用值来当key,value为这个数组中这个数字的出现次数,然后遍历第二个数组,当map中有这个值存在而且value大于0,就将这个值加入到list中,最后再将list转化为int[],但是不能直接转换成int[],只能直接转换成Integer[],所以就采用了遍历赋值的方法.

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
		Map<Integer,Integer> map = new HashMap<>();
		List<Integer> list = new ArrayList<>();
		for (int x : nums1) {
			if(map.containsKey(x)){
				map.put(x,map.get(x)+1);
			}else{
				map.put(x,1);
			}
		}
		for(int x : nums2){
			if(map.get(x) != null && map.get(x) > 0){
				list.add(x);
				map.replace(x,map.get(x)-1);
			}
		}
		int[] ans = new int[list.size()];
		for(int i=0; i < list.size(); i++){
			ans[i] = list.get(i);
		}
        return ans;
    }
}	
```



官方题解思路差不多,但是为了降低空间复杂度，首先遍历较短的数组并在哈希表中记录每个数字以及对应出现的次数，然后遍历较长的数组得到交集。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return intersect(nums2, nums1);
        }
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int num : nums1) {
            int count = map.getOrDefault(num, 0) + 1;
            map.put(num, count);
        }
        int[] intersection = new int[nums1.length];
        int index = 0;
        for (int num : nums2) {
            int count = map.getOrDefault(num, 0);
            if (count > 0) {
                intersection[index++] = num;
                count--;
                if (count > 0) {
                    map.put(num, count);
                } else {
                    map.remove(num);
                }
            }
        }
        return Arrays.copyOfRange(intersection, 0, index);
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/solution/liang-ge-shu-zu-de-jiao-ji-ii-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



##  买卖股票最佳时机

```java
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

 

示例 1：

输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2：

输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

自己的思路: 建立一个新的数组,将明天减去今天的利润存入,然后通过贪心算法求出最大利润,解法如之前做过的最大连续子序和题目一样,时间复杂度O(n),空间复杂度O(n-1).

```java
class Solution {
    public int maxProfit(int[] prices) {
        int[] profit = new int [prices.length];
        int maxSum = 0;
        int currentSum = 0;
        int lastSum = 0; 
        for(int i = 0 ; i < prices.length-1; i++){
            profit[i] = prices[i+1]-prices[i];
        }
        for(int i = 0; i < profit.length; i++){
            int num = profit[i];
            if(lastSum > 0){
                currentSum = num + lastSum;
            }else{
                currentSum = num;
            }
            lastSum = currentSum;
            maxSum = (currentSum > maxSum) ? currentSum : maxSum;
        }
        return maxSum;
    }
}
```

官方题解还是牛逼,一次遍历,动态的取一个最低价格,动态的假装卖出并且记录最大利润,最后返回最大利润即可.

- 时间复杂度：O(n)，只需要遍历一次。
- 空间复杂度：O(1)，只使用了常数个变量。

```java

public class Solution {
    public int maxProfit(int prices[]) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for (int i = 0; i < prices.length; i++) {
            if (prices[i] < minprice) {
                minprice = prices[i];
            } else if (prices[i] - minprice > maxprofit) {
                maxprofit = prices[i] - minprice;
            }
        }
        return maxprofit;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/solution/121-mai-mai-gu-piao-de-zui-jia-shi-ji-by-leetcode-/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```


