# 2022-01-05


# 1576.替换所有的问号

![image-20220105075332984](https://s2.loli.net/2022/01/05/Y3ibXvEIWC19HT4.png)

```go
func modifyString(s string) string {
     data := []byte(s)
	for i := 0; i < len(data); i++ {
		if data[i] == '?' {
			for b := byte('a'); b <= 'c'; b++ {
				if !(i > 0 && data[i-1] == b || i < len(data)-1 && data[i+1] == b) {
					data[i] = b
					break
				}
			}
		}
	}
	return string(data[:])
}
```

 一个点在于, 实际上只需要遍历三个字母,就可以找到适合的答案, 还有就是里面那个 if 语句很好的解决了i=0 和

i = length-1的问题.

[]byte和string之间的转化方式 res := []byte( s )              string(res)

# 剑指offer II 027.回文联表

![image-20220106111356960](https://s2.loli.net/2022/01/06/yfL5J63I2UETFYd.png)

**递归判断回文链表**

```Go
var left *ListNode

func isPalindrome(head *ListNode) bool {
  left = head
  return traverse(head)
}
func traverse(right *ListNode) bool {
  if right == nil {
    return true
  }
  res := traverse(right.Next)
  res = res && (right.Val == left.Val)
  left = left.Next
  return res
}
```

**优化空间复杂度:**

先找到链表的中点

```Go
伪代码
slow,fast := head,head
for fast != nil && fast.next != nil{
  slow = slow.Next
  fast = fast.Next.Next
}
长度为奇数的时候slow为中点
长度为偶数的时候slow为靠后的那个
如果fast没有指向nil,说明链表长度为奇数,slow还要向前一步
if fast != nil{ //说明fast.Next == nil 
  slow = slow.Next
}
```

然后从slow开始反转后面的链表, 然后开始比较回文串, 整合一下

```Go
func isPalindrome(head *ListNode)bool{
  slow,fast := head,head
  for fast != nil && fast.Next != nil{
    slow = slow.Next
    fast = fast.Next.Next
  }
  if fast != nil{
    slow = slow.Next
  }
  left := head
  right := reverse(slow)
  for right != nil{
    if left.Val != right.Val{
      return false
    }
    left = left.Next
    right = right.Next
  }
  return true
}
func reverse(head *ListNode) *ListNode{//让指针掉个头
  var pre *ListNode = nil
  cur := head
  for cur != nil{
    next := cur.Next
    cur.Next = pre
    pre = cur
    cur = next
  }
  return pre
}
```

时间复杂度O(n),空间复杂度O(1),只改变了链表的指向

