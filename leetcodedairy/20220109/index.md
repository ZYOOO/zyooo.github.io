# 2022-01-09


# 田忌赛马,打不过就送人头

代码逻辑

```Go
n := len(nums1)
sort.Ints(nums1) //田忌的马
sort.Ints(nums2) //齐王的马

for i := n-1; i >= 0; i-- {
  if nums1[i] > nums2[i] {
    //比得过跟他比  
  } else {
    //比不过,换的垫底的来送人头
  }
}
int[] advantageCount(int[] nums1, int[] nums2) {
    int n = nums1.length;
    // 给 nums2 降序排序
    PriorityQueue<int[]> maxpq = new PriorityQueue<>(
        (int[] pair1, int[] pair2) -> { 
            return pair2[1] - pair1[1];  //比较方式
        }
    );
    for (int i = 0; i < n; i++) {
        maxpq.offer(new int[]{i, nums2[i]});
    }
    // 给 nums1 升序排序
    Arrays.sort(nums1);

    // nums1[left] 是最小值，nums1[right] 是最大值
    int left = 0, right = n - 1;
    int[] res = new int[n];

    while (!maxpq.isEmpty()) {
        int[] pair = maxpq.poll();
        // maxval 是 nums2 中的最大值，i 是对应索引
        int i = pair[0], maxval = pair[1];
        if (maxval < nums1[right]) {
            // 如果 nums1[right] 能胜过 maxval，那就自己上
            res[i] = nums1[right];
            right--;
        } else {
            // 否则用最小值混一下，养精蓄锐
            res[i] = nums1[left];
            left++;
        }
    }
    return res;
}
```

go的话不用优先队列,用二维数组的排序试一试

## 二维数组排序

```Go
sort.Slice(arr, func(i, j int) bool {
    return arr[i][1] > arr[j][1] //按照每行的第一个元素排序,> 降序, < 升序
  })
func advantageCount(nums1, nums2 []int) []int {
  sort.Ints(nums1)
  var arr [][]int
  res := make([]int, len(nums1))
  left, right := 0, len(nums1)-1
  for i, v := range nums2 {
    tmp := []int{i, v}
    arr = append(arr, tmp)
  }
  sort.Slice(arr, func(i, j int) bool {
    return arr[i][1] > arr[j][1] //按照每行的第一个元素排序
  })
  for i := 0; i < len(nums1); i++ {
    if arr[i][1] < nums1[right] {
      res[arr[i][0]] = nums1[right]
      right--
    } else {
      res[arr[i][0]] = nums1[left]
      left++
    }
  }
  return res
}
```

# 接雨水问题

![image-20220109204026793](https://s2.loli.net/2022/01/09/iOAQmCXJhZdc9GD.png)

思路: water[i] = min( l_max , r_max) - height[i]

暴力:  每次求出两边最大值 O(N^2) O(1)

```Go
int trap(vector<int>& height) {
    int n = height.size();
    int res = 0;
    for (int i = 1; i < n - 1; i++) {
        int l_max = 0, r_max = 0;
        // 找右边最高的柱子
        for (int j = i; j < n; j++)
            r_max = max(r_max, height[j]);
        // 找左边最高的柱子
        for (int j = i; j >= 0; j--)
            l_max = max(l_max, height[j]);
        // 如果自己就是最高的话，
        // l_max == r_max == height[i]
        res += min(l_max, r_max) - height[i];
    }
    return res;
}
```

备忘录 :  提前求出l_max, r_max 数组 O(N) O(N)

```Go
int trap(vector<int>& height) {
    if (height.empty()) return 0;
    int n = height.size();
    int res = 0;
    // 数组充当备忘录
    vector<int> l_max(n), r_max(n);
    // 初始化 base case
    l_max[0] = height[0];
    r_max[n - 1] = height[n - 1];
    // 从左向右计算 l_max
    for (int i = 1; i < n; i++)
        l_max[i] = max(height[i], l_max[i - 1]);
    // 从右向左计算 r_max
    for (int i = n - 2; i >= 0; i--) 
        r_max[i] = max(height[i], r_max[i + 1]);
    // 计算答案
    for (int i = 1; i < n - 1; i++) 
        res += min(l_max[i], r_max[i]) - height[i];
    return res;
}
```

双指针:  O(N) O(1)

```Go
func trap(height []int) int {
  if len(height) == 0 {
    return 0
  }
  n := len(height)
  res, left, right := 0, 0, n-1
  lMax, rMax := height[0], height[n-1]
  for left <= right {
    lMax = int(math.Max(float64(lMax),float64(height[left])))
    rMax = int(math.Max(float64(rMax),float64(height[right])))
    if lMax < rMax {
      res += lMax - height[left]
      left++
    } else {
      res += rMax - height[right]
      right--
    }
  }
  return res
}
```

![image-20220109203948861](https://s2.loli.net/2022/01/09/SG6VmXONedlEhJn.png)

r_max是不是右边最大的并没所谓,只要 l_max < r_max 就行了

# 最长回文子串

**核心思想: 从中间开始向两边扩散来判断回文串, 为了解决abba偶数问题,传入两个参数l,r**

```Go
for 0 <= i < len(s){
  找到以 s[i] / s[i]和s[i+1](要防止数组越界) 为中心的回文串
  更新答案
}
func longestPalindrome(s string) string {
  res := ""
  for i := 0; i < len(s); i++ {
    //以s[i]为中心的最长回文子串
    s1 := palindrome(s, i, i)
    //以s[i]和s[i+1]为中心得最长回文子串
    s2 := palindrome(s, i, i+1)
    //res = longest(res,s1,s2)
    if len(res) < len(s1) {
      res = s1
    }
    if len(res) < len(s2) {
      res = s2
    }
  }
  return res
}
func palindrome(s string, l, r int) string {
  //防止索引越界
  for l >= 0 && r < len(s) && s[l] == s[r] {
    l--
    r++
  }
  sb := []byte(s)
  //返回以s[l] 和 s[r]为中心的最长回文串
  //返回的索引应是l+1 ~ r-1 因为切片是到x - 1,所以这里到r 不包含r
  return string(sb[l+1 : r])
}
```

也可以用动态规划,但是动态规划空间复杂度O(N^2) 这个只需要O(1)

