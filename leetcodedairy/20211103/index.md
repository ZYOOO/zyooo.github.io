# 2021-11-03




## 合并两个有序数组

给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

 ```java
 示例 1：
 
 输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
 输出：[1,2,2,3,5,6]
 解释：需要合并 [1,2,3] 和 [2,5,6] 。
 合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
 
 来源：力扣（LeetCode）
 链接：https://leetcode-cn.com/problems/merge-sorted-array
 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 ```

使用双指针比较两数组尾,即排序后的最大数值,倒序放入nums1中,如果两个数组中的一个已经遍历完,则剩余的都为另一数组剩余的.

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        //从尾部开始比谁大再放入tail指向的nums1中值
        int p1 = m-1;
        int p2 = n-1;
        int tail = m+n-1;
        while(p2 >= 0 || p1 >= 0){
            //m个已经全部放完,剩下的全部放nums2
            if(p1 == -1){
                nums1[tail--] =  nums2[p2--];
            }else if(p2 == -1){
                nums1[tail--] = nums1[p1--];
            }else if(nums2[p2] >= nums1[p1]){
                nums1[tail--] =  nums2[p2--];
            }else{
                nums1[tail--] =  nums1[p1--];
            }
        }
    }
}
```



## 两数之和

```java
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

    
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

将数组遍历放进哈希表中,数组值作为key,数组索引作为value,一边添加一边去判断,当前添加的数组的值,是否已经有另一半答案已经添加了,即是否存在  目标值减去当前值的差已经在表中,有的话得出value 即数组的下标,输出答案

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0; i<nums.length; i++){
            if(map.containsKey(target - nums[i])){
                return new int [] {i,map.get(target - nums[i])};
            }
            map.put(nums[i],i);
        }
        return null;
    }
}
```


