# 2021.10.08


## 1.旅行终点站 简单 2021.10.08

给你一份旅游线路图，该线路图中的旅行线路用数组 paths 表示，其中 paths[i] = [cityAi, cityBi] 表示该线路将会从 cityAi 直接前往 cityBi 。请你找出这次旅行的终点站，即没有任何可以通往其他城市的线路的城市。题目数据保证线路图会形成一条不存在循环的线路，因此恰有一个旅行终点站。

解题的思路就是遍历一遍paths，取cityB，第二重遍历取cityA，当第二重遍历的cityA等于第一重遍历的cityB时说明这条路线是有出路的，反之当第二重遍历结束后依然没有对应的cityA，说明此时第一重遍历的cityB即为终点。

但是实现方式过于单一，没有去向其他更快捷的方式，导致速度太慢，参考官方题解当中，利用哈希表HashSet去储存一组需要遍历的量，提高速度。

```java
class Solution {
    public String destCity(List<List<String>> paths) {
        for(int i=0; i<paths.size(); i++){
            for(int j=0; j<paths.size(); j++){
                if(i!=j && paths.get(i).get(1).equals(paths.get(j).get(0))){
                    break;
                }else if(j==paths.size()-1){
                    return paths.get(i).get(1);
                }
            }
        }
        return "";
    }
}
```

```java
//官方题解
class Solution {
    public String destCity(List<List<String>> paths) {
        Set<String> citiesA = new HashSet<String>();
        for (List<String> path : paths) {
            citiesA.add(path.get(0));
        }
        for (List<String> path : paths) {
            if (!citiesA.contains(path.get(1))) {
                return path.get(1);
            }
        }
        return "";
    }
}
/***
作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/destination-city/solution/lu-xing-zhong-dian-zhan-by-leetcode-solu-pscd/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
***/
```

官方题解速度2ms，自己的12ms。自己时间复杂度O(n²)  

官方答案  时间复杂度：O(nm)，其中n是数组paths 的长度，m是城市名称的最大长度。

查看 contains

- ```java
  public boolean contains(Object o)
  ```

  如果此集合包含指定的元素，则返回`true` 。  

  更正式地，返回`true`当且仅当该集合包含元素`e`  ，使得`(o==null ? e==null : o.equals(e))` 。 


