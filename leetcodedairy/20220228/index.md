# 2022-02-28


# 1601.最多可达成的换楼请求目的 hard

![image-20220228075110748](https://s2.loli.net/2022/02/28/tPkgsild7Urh8bn.png)

![image-20220228075124493](https://s2.loli.net/2022/02/28/ejJ275VqaizA6YR.png)

![image-20220228075133724](https://s2.loli.net/2022/02/28/CzRvX4fold6BiNp.png)

对于每个选择, 都有选或者不选两个操作, delta数组记录每栋楼的员工变化量, 以及变量cnt记录被选择的请求数量, 如果选择当前请求, delta[x]值减1, delta[y]加1, 不选择则不做操作, 枚举完了之后判断delta中所有值是否为0, 并且根据cnt和res大小更新答案.

```go
func maximumRequests(n int, requests [][]int) int {
    res := 0 
    delta := make([]int,n)
    cnt,res,zero,l := 0,0,n,len(requests)
    var dfs func(pos  int)
    dfs = func(pos int){
        if pos == l {
            if zero == n && cnt > res {
                res = cnt
            }
            return 
        }
        //not
        dfs(pos+1)
        //chose
        z := zero
        cnt++
        x,y := requests[pos][0],requests[pos][1]
        //减少之前
        if delta[x] == 0 {
            zero--
        }
        delta[x]--
        //减少之后
        if delta[x] == 0 {
            zero++
        }
        if delta[y] == 0 {
            zero--
        }
        delta[y]++
        if delta[y] == 0 {
            zero++
        }
        dfs(pos+1)
        delta[x]++
        delta[y]--
        cnt--
        zero = z
    }
    dfs(0)
    return res
}
```


