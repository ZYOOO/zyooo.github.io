# 第282场周赛T3


![image-20220227174013645](https://s2.loli.net/2022/02/27/MQCcyxESagJZ12u.png)

这道题用O(N^2)的解法会超时, 所以要想办法降低时间复杂度, 所以使用二分法, 但是这里二分的不是time数组, 而是时间, 而时间的区间如何取定呢, 先把time升序排序, 然后最短的可能是1s, 最长的时间可能是time[0] * totoalTrips, 所以就可以采用二分法了

```Go
func minimumTime(time []int, totalTrips int) int64 {
    sort.Ints(time)
    var left,right int64 = 1,int64(time[0]*totalTrips)
    n := len(time)
    for left < right {
        var tmp int64= 0
        mid := left + (right-left)>>1
        for i := 0; i < n && int64(time[i]) <= mid; i++ {
            tmp += mid/int64(time[i])
        }
        //左搜索, 右收缩
        if tmp >= int64(totalTrips) {
            right = mid
        } else {
            left = mid+1
        }
    }
    return left
}
```

