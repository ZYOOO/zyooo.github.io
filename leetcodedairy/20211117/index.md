# 2021-11-17


## 318.最大单词长度乘积

```java
给定一个字符串数组 words，找到 length(word[i]) * length(word[j]) 的最大值，并且这两个单词不含有公共字母。你可以认为每个单词只包含小写字母。如果不存在这样的两个单词，返回 0。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-product-of-word-lengths
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处.
```

本来以为简简单单，结果发现看漏了题目中不含有公共字母的两个单词。然后感觉难度暴增，最后也是成功写出了屎一般的代码，11%+6%。看了题解之后学到了新的东西，思路其实差不多，最优的时间复杂度也是要O(n²)，但是在比较是否出现公共字母的时候，不想用int[26]去表示一个字符串出现过的字母，那样子空间和时间都花费很多，最后看了题解发现可以用掩码去解决，就是用一个int的低26位表示，效果和数组一样，而且比较只需要&运算就能算出，快太多了。

官方题解是将mask（掩码）作为map的key，length作为map的val，然后再去双重遍历keySet得出最大的答案。按照这个思路做了之后，还是不够快，最后去看高速度的题解，才发现有更快的方法！真是厉害，直接不用map，用于words长度相等的一个int[]去存放掩码，然后通过对应的数组下标可以直接得到words[i].length()空间上更加节省了，同时因为是有顺序去二重遍历的，所以一重循环i<words.length-1，而二重循环的ｊ的开始是ｉ＋１，因为在在前面ｉ的遍历中已经和当前ｊ匹配过了，所以无需再匹配了。

```java
class Solution {
    public int maxProduct(String[] words) {
        int ans = 0;
        //words中每个string对应的掩码
        int[] masks = new int[words.length];
        for(int i = 0; i < words.length; i++){
            for(char c : words[i].toCharArray()) masks[i] |= 1<<(c-'a');   
        }
        //二重遍历
        for(int i = 0; i < words.length-1; i++){
            for(int j = i+1; j < words.length; j++){
                //字符串无公共字母
                if((masks[i] & masks[j]) == 0) 
                    ans = Math.max(ans,words[i].length() * words[j].length());
            }
        }
        return ans;
    }
}
```




