# 2022-01-09


## 二分搜索

```Go
//搜素左侧边界
func left_bound(nums []int, target int){
  if len(nums) == 0 {
     return -1
  }
  left, right := 0,len(nums)
  for left < right{
    mid := left + (right - left) / 2
    if nums[mid] == target{
    //找到target时,收缩右侧边界
      right = mid
    }else if nums[mid] < target{
      left = mid + 1  
    }else if nums[mid] > target{
      right = mid
    }
  }
  return left
}

// nums = [1,2,3,3,3,5,7,] target = 3   return 2
//搜索右侧边界
func right_bound(nums int[], target int){
  if len(nums) == 0 {
     return -1
  }
  left, right := 0,len(nums)
  for left < right{
    mid := left + (right - left) / 2
    if nums[mid] == target{
    //找到target时,收缩左侧边界
      left = mid + 1
    }else if nums[mid] < target{
      left = mid + 1  
    }else if nums[mid] > target{
      right = mid
    }
  }
  return left
}

// nums = [1,2,3,3,3,5,7,] target = 3   return 4
```

技巧: 从题目中抽象出一个自变量x , 一个关于x的函数f(x), 以及一个目标值 target , 同时还要满足以下条件 : 1. f(x)必须是在x上的单调函数 2. 题目是让你计算满足约束条件f(x) == target 时的x 的值

例如上例:

```Go
func f(x int, nums []int) int {
  return nums[x]
}
```

题目就相当于求 满足 f(x) == targeet 的x的最小值是多少

框架:

```Go
//函数f是关于自变量x的单调函数
func f(x int){
  //...
}
//主函数,在f(x) == target 的约束下求x的最值
func solution (nums []int, target int) int {
  if len(nums) == 0 {
    return -1 //看题目
  }
  left,right := ..., ... + 1
  for left < right {
    mid := left + (right - left) / 2
    if f(mid) == target {
      //题目是求左边界还是右边界?
    } else if f(mid) < target {
      //怎么让f(mid) 大一点?  
    } else if f(mid) > target {
      //怎么让f(mid) 小一点?
    }
  }
  return left
}
```

# 1011.在D天内送达包裹的能力

![image-20220109203755663](C:\Users\86159\AppData\Roaming\Typora\typora-user-images\image-20220109203755663.png)

分析: 用二分法方法, 找出x , f(x) 还有 target(题目) , target一般和f(x)的值作比较, 题目要找谁的边界值, 谁就是x

所以此题 target 为 days , x为承载能力, f(x) 为承载能力对应的天数. 单调递减, 且求最低运载能力 向左收缩, 收缩右边

```Go
func f(weights []int, x int) int {
  day := 0
  weight := 0
  for _, w := range weights {
    weight += w
    if weight > x {
      weight = w
      day++
    }
  }
  return day + 1
}
func shipWithinDays(weights []int, days int) int {
  //承载能力为x, f(x)为天数, days为target
  //left为weights里面的最大值,如果小于最大值那么这一批货永远运不了, right为一次送完的承载量
  left, right := 0, 1
  for _, x := range weights {
    if x > left {
      left = x
    }
    right += x
  }
  for left < right {
    mid := left + (right-left)/2
    //函数单调递减
    if f(weights, mid) <= days {
      //最低运载,左搜索,右收缩
      right = mid
    } else {
      left = mid + 1
    }
  }
  return left
}
```

# 875.爱吃香蕉的珂珂

![image-20220109203736726](https://s2.loli.net/2022/01/09/etg3r4zZADJvIfm.png)

同上分析: 速度为x  对应的时间为f(x)  H为target ,最小速度 向左搜索, 右侧收缩

```Go
func f(piles []int, x int) int {
  hours := 0
  for i := 0; i < len(piles); i++ {
    hours += piles[i] / x
    if piles[i]%x > 0 {
      hours++
    }
  }
  return hours
}
func minEatingSpeed(piles []int, h int) int {
  //二分搜索速度,target是H,左侧搜索,收缩右侧
  left,right := 1,1000000000 + 1
  for left < right {
    mid := left + (right - left) / 2
    if f(piles,mid) <= h {
      //收缩左侧
      right = mid
    } else {
      left = mid + 1 
    }
  }
  return left
}
```


