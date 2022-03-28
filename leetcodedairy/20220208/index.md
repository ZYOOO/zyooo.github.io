# 2022-02-08


# 1001.网格照明  hard  2022.02.08

![](image\image.png "")

![](image\image_1.png "")

![](image\image_2.png "")

数据规模太大了, 原本想着硬模拟的后来还是放弃了

打算用map去存储行列和两个对角的情况, 但是又不知道怎么去表示同一条对角



官方题解:

```Go
func gridIllumination(n int, lamps, queries [][]int) []int {
    type pair struct{ x, y int }
    points := map[pair]bool{}
    row := map[int]int{}
    col := map[int]int{}
    diagonal := map[int]int{}
    antiDiagonal := map[int]int{}
    for _, lamp := range lamps {
        r, c := lamp[0], lamp[1]
        p := pair{r, c}
        if points[p] {
            continue
        }
        points[p] = true
        row[r]++
        col[c]++
        //用这个来表示哪条对角, 妙啊
        diagonal[r-c]++
        antiDiagonal[r+c]++
    }

    ans := make([]int, len(queries))
    for i, query := range queries {
        r, c := query[0], query[1]
        if row[r] > 0 || col[c] > 0 || diagonal[r-c] > 0 || antiDiagonal[r+c] > 0 {
            ans[i] = 1
        }
        for x := r - 1; x <= r+1; x++ {
            for y := c - 1; y <= c+1; y++ {
                if x < 0 || y < 0 || x >= n || y >= n || !points[pair{x, y}] {
                    continue
                }
                delete(points, pair{x, y})
                row[x]--
                col[y]--
                diagonal[x-y]--
                antiDiagonal[x+y]--
            }
        }
    }
    return ans
}
```




学习点: 

对于在map里面存储一个坐标, 可以定义一个结构体

```Go
type pair struct{
  x int
  y int
}
pairs := map[pair]bool{}
//加了花括号就不用make了?
```




如何表示在同一条对角线

```Go
  p := pair{r,c}
  if points[p] {//这里不管有没有添加,可以这样判断, 应该是有默认值的
      continue
  }
  points[p] = true
  row[r]++
  col[c]++
  //用这个来表示哪条对角, 妙啊
  diagonal[r-c]++
  antiDiagonal[r+c]++
```


