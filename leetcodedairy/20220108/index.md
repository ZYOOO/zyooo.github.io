# 2022-01-09


# 滑动窗口问题

```Go
int left = 0, right = 0
for right < len(s) {
  //增大窗口
  window.add(s[right])
  right++
  for window needs shrink{
    window.remove(s[left])
    left++
  }
}
```

O(N)

## 框架

```Go
//滑动窗口算法框架
func slidingWindow(s, t string) {
  need := make(map[string]int)
  window := make(map[string]int)
  for _,c := range t{
    need[string(c)]++
  }
  left,right,valid := 0,0,0
  for right < len(s) {
    //c是将移入窗口的字符
    c := s[right]
    //右移窗口
    right++
    //对窗口内数据的一系列更新
    ...
    //debug输出的位置
    fmt.Printf("window: [%d,%d)\n",left,right)
    //判断左侧窗口是否要收缩
    for window needs shrink{
      //d 是将移出窗口的字符
      d := s[left]
      //左移窗口
      left++
      //进行窗口内数据的一系列更新
      ...
    }
  }
}
```

# 76.Minimum Window Substring

![image-20220109203852531](https://s2.loli.net/2022/01/09/5AMxZqplrgsnbft.png)

```Go
func minWindow(s, t string) string {
  need := make(map[string]int)
  window := make(map[string]int)
  for _, c := range t {
    need[string(c)]++
  }
  left, right, valid := 0, 0, 0
  start, length := 0, len(s)+1
  for right < len(s) {
    //c是将移入窗口的字符
    c := string(s[right])
    //右移窗口
    right++
    //对窗口内数据的一系列更新
    if _, ok := need[c]; ok {
      window[c]++
      if window[c] == need[c] {
        valid++
      }
    }
    //判断左侧窗口是否要收缩
    for valid == len(need) {
      if right-left < length {
        start = left
        length = right - left
      }
      //d 是将移出窗口的字符
      d := string(s[left])
      //左移窗口
      left++
      //进行窗口内数据的一系列更新
      if _, ok := need[d]; ok {
        if window[d] == need[d] {
          valid--
        }
        window[d]--
      }
    }
  }
  if length == len(s)+1 {
    return ""
  }
  return s[start : start+length]
}
```

