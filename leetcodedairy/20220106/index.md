# 2022-01-06


# 15.三数之和

![image-20220106165502365](https://s2.loli.net/2022/01/06/PYwRCcyt5rI74Bd.png)

穷举,确定了第一个数字之后, 剩下两个数字就是 target = target - nums[i] 的twoSum问题

```Go
func twoSumTarget2(nums []int, start, target int) [][]int {
  var ans [][]int
  //sort.Ints(nums)
  lo, hi := start, len(nums)-1
  for lo < hi {
    sum := nums[lo] + nums[hi]
    left, right := nums[lo], nums[hi]
    if sum < target {
      for lo < hi && nums[lo] == left { //跳过重复项,保证答案唯一
        lo++
      }
    } else if sum > target {
      for lo < hi && nums[hi] == right {
        hi--
      }
    } else {
      ans = append(ans, []int{nums[lo], nums[hi]})
      for lo < hi && nums[lo] == left {
        lo++
      }
      for lo < hi && nums[hi] == right {
        hi--
      }
    }
  }
  return ans
}

func threeSumTarget(nums []int, target int) [][]int {
  sort.Ints(nums)
  var ans [][]int
  for i := 0; i < len(nums); i++ {
    tuples := twoSumTarget2(nums, i+1, target-nums[i])
    //存在二元组,就加上nums[i]成为三元组
    for j := 0; j < len(tuples); j++ {
      tuples[j] = append(tuples[j], nums[i])
      ans = append(ans, tuples[j])
    }
    //跳过第一个数字重复的情况,避免出现重复结果
    for i < len(nums)-1 && nums[i] == nums[i+1] {
      i++
    }
  }
  return ans
}
```

# 71.简化路径

![image-20220106165610926](https://s2.loli.net/2022/01/06/sRfirE76AFLJvTQ.png)

```go
func simplifyPath(path string) string {
    stack := []string{}
    for _, name := range strings.Split(path, "/") {
        if name == ".." {
            if len(stack) > 0 {
                stack = stack[:len(stack)-1]
            }
        } else if name != "" && name != "." {
            stack = append(stack, name)
        }
    }
    return "/" + strings.Join(stack, "/")
}
```


