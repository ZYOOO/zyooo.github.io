# 2022-02-24


![image-20220227170214699](https://s2.loli.net/2022/02/27/6MhVFXtB8ugp3cz.png)

开始是直接遍历的方法得到数字, 后来想着如果是严格按照一个形式的话, 那么 一个复数中只会出现一个 '+'

并且将虚部和实部分割, 可以利用这个去直接得到abcd, 不过不知道底层代码, 估计速度也是差不多的

代码: 100% 100%

```Go
func complexNumberMultiply(num1 string, num2 string) string {
    a,b := parse(num1) 
    c,d := parse(num2)
    return fmt.Sprintf("%v+%vi",(a*c-b*d),(a*d+b*c))
}
func parse (num string)(int,int){
    a,b := 0,0
    i,fa,fb := 0,1,1
    flag := true
    n := len(num)
    if num[0] == '-' {
        fa = -1
        i = 1
    }
    for ;i < n; i++{
        if num[i] >= '0' && num[i] <= '9'{
            if flag {
                a = a*10 + int(num[i] - '0')
            } else {
                b = b*10 + int(num[i] - '0')
            }
        }
        if num[i] == '+' {
            flag = false
        } else if num[i] == '-' {
            flag = false
            fb = -1
        }
    }
    return a*fa,b*fb
}
```

代码:100% 94%

```Go
func complexNumberMultiply(num1 string, num2 string) string {
    //注意消除最后一位i
    a1 := strings.Split(num1[:len(num1)-1],"+")
    a2 := strings.Split(num2[:len(num2)-1],"+")
    a,_ := strconv.ParseInt(a1[0],10,32)
    b,_ := strconv.ParseInt(a1[1],10,32)
    c,_ := strconv.ParseInt(a2[0],10,32)
    d,_ := strconv.ParseInt(a2[1],10,32)
    return fmt.Sprintf("%v+%vi",(a*c-b*d),(a*d+b*c))
}
```


