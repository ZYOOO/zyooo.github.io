# 2021-11-05


## 重塑矩阵

```java
在 MATLAB 中，有一个非常有用的函数 reshape ，它可以将一个 m x n 矩阵重塑为另一个大小不同（r x c）的新矩阵，但保留其原始数据。

给你一个由二维数组 mat 表示的 m x n 矩阵，以及两个正整数 r 和 c ，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的 行遍历顺序 填充。

如果具有给定参数的 reshape 操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

 

示例 1：
输入：mat = [[1,2],[3,4]], r = 1, c = 4
输出：[[1,2,3,4]]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reshape-the-matrix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

![](https://i.loli.net/2021/11/05/fGDHuFsPSLcl14b.jpg)

原本想的就是先判断面积是否相等,然后将速度按照rc去放入,但是赶着吃饭没想明白用取余的具体操作,就直接暴力了.

```java
class Solution {
    public int[][] matrixReshape(int[][] mat, int r, int c) {
        if(r*c != mat.length*mat[0].length){
            return mat;
        }
        int[] arr = new int[r*c];
        int index = 0;
        for(int i = 0; i < mat.length; i++){
            for(int j = 0; j < mat[0].length; j++){
                arr[index++] = mat[i][j];
            }
        }
        int[][] ans =  new int[r][c];
        index = 0;
        for(int i = 0; i < r; i++){
            for(int j = 0; j < c; j++){
                ans[i][j] = arr[index++];
            }
        }
        return ans;
    }
}
```

官方题解

```java

class Solution {
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        int m = nums.length;
        int n = nums[0].length;
        if (m * n != r * c) {
            return nums;
        }

        int[][] ans = new int[r][c];
        for (int x = 0; x < m * n; ++x) {
            ans[x / c][x % c] = nums[x / n][x % n];
        }
        return ans;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/reshape-the-matrix/solution/zhong-su-ju-zhen-by-leetcode-solution-gt0g/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



## 杨辉三角

感觉写的又长又臭啊

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
                List<List<Integer>> ans = new ArrayList<List<Integer>>();
        for(int i = 0; i < numRows; i++){
            List<Integer> list = new ArrayList<>();
            list.add(1);
            if(i == 0){
                ans.add(list);
                continue;
            }else if(i == 1){
                list.add(1);
            }else{
                for(int j = 1; j < i; j++){
                    list.add(ans.get(i-1).get(j) + ans.get(i-1).get(j-1));
                }
                list.add(1);
            }
            ans.add(list);
        }
        return ans;
    }
}
```

官方题解,这个++j 就是差距,学到了.

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ret = new ArrayList<List<Integer>>();
        for (int i = 0; i < numRows; ++i) {
            List<Integer> row = new ArrayList<Integer>();
            for (int j = 0; j <= i; ++j) {
                if (j == 0 || j == i) {
                    row.add(1);
                } else {
                    row.add(ret.get(i - 1).get(j - 1) + ret.get(i - 1).get(j));
                }
            }
            ret.add(row);
        }
        return ret;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/pascals-triangle/solution/yang-hui-san-jiao-by-leetcode-solution-lew9/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```


