# 2021-10-31


## 最大子序和

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```
示例:
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

用贪心算法去解决此问题,遍历数组,得到当前的值,如果lastCount小于零的话就抛弃,当前和就等于当前值,如果lastCount大于等于0的话就和当前值相加作为当前和,lastCount等于当前和,当前和和最大和比较,得出最大和,遍历完一边之后返回最大和.

时间复杂度:O(n)  		空间复杂度: O(1)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int index = 0;
        int lastCount = 0;
        int maxCount = 0;
        for(int currentNum : nums){
            int currentCount;
            if(index == 0){
                index++;
                currentCount = currentNum;
                lastCount = currentCount;
                maxCount = currentCount;
            }else{
                if(lastCount < 0){
                    currentCount = currentNum;
                    maxCount = (maxCount > currentCount) ? maxCount : currentCount;
                    lastCount = currentCount;
                }else{
                    currentCount = currentNum + lastCount;
                    maxCount = (maxCount > currentCount) ? maxCount : currentCount;
                    lastCount = currentCount;
                }
            }
        }
        return maxCount;
    }
}

```

自己写的又长又臭,看看题解的动态规划

```
class Solution {
    public int maxSubArray(int[] nums) {
        int pre = 0, maxAns = nums[0];
        for (int x : nums) {
            pre = Math.max(pre + x, x);
            maxAns = Math.max(maxAns, pre);
        }
        return maxAns;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/maximum-subarray/solution/zui-da-zi-xu-he-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





## 存在重复元素

给定一个整数数组，判断是否存在重复元素。

如果存在一值在数组中出现至少两次，函数返回 `true` 。如果数组中每个元素都不相同，则返回 `false` 。

比较简单利用哈希表的add去判断是否有重复添加

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        for(int num : nums){
            if(!set.add(num)){
                return true;
            }
        }
        return false;
    }
}
```


