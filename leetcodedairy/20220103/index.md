# 2022-01-03


#### 1185.[一周中的第几天](https://leetcode-cn.com/problems/day-of-the-week/)

难度简单92

给你一个日期，请你设计一个算法来判断它是对应一周中的哪一天。

输入为三个整数：`day`、`month` 和 `year`，分别表示日、月、年。

您返回的结果必须是这几个值中的一个 `{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}`。

用了一个公式： W = (D + 2 * M + 3 * (M + 1) \ 5 + Y + Y \ 4 - Y \ 100 + Y \ 400+1) Mod 7 

但是一月和二月要用13,14表示。

```java
class Solution {
    public String dayOfTheWeek(int day, int month, int year) {
        String[] days  = {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"};
        if(month == 1){
            month = 13;
            year--;
        }else if(month == 2){
            month = 14;
            year --;
        }
        //W = (D + 2 * M + 3 * (M + 1) \ 5 + Y + Y \ 4 - Y \ 100 + Y \ 400+1) Mod 7 
        return days[(day + 2 * month + 3 * (month + 1) / 5 + year + year / 4 - year / 100 + year / 400 + 1) % 7]; 
        
    }
}
```

#### 23.合并k个有序链表



**难点在于如何快速得到k个节点中的最小节点**

**优先级队列（二叉堆），将链表中的节点放入一个最小堆，每次获得k个结点中最小的节点**

**go里面没有内置的优先队列api，所以要自己写（用最小堆可以实现，重写heap里的方法去实现）**

**或者使用分治的方法去实现**



**最小堆方法实现：**

```go
package main

import "container/heap"

type ListNode struct {
	Val  int
	Next *ListNode
}
type minHeap []*ListNode

func (h minHeap) Len() int {
	return len(h)
}
func (h minHeap) Less(i, j int) bool {
	return h[i].Val < h[j].Val
}
func (h minHeap) Swap(i, j int) {
	h[i], h[j] = h[j], h[i]
}
func (h *minHeap) Push(x interface{}) {
	*h = append(*h, x.(*ListNode)) //.后跟括号，是对象类型转换，只有结构对象才能执行类型动态转换/查询
}
func (h *minHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]     //old[n-1]是最小元素，堆顶？查查看最小堆概念
	*h = old[0 : n-1] //切片删除old[n-1] endIndex-1即 0~n-2
	return x
}

func mergeKLists(lists []*ListNode) *ListNode {
	h := new(minHeap) //创建一个最小堆
	for i := 0; i < len(lists); i++ {
		if lists[i] != nil {
			//k个链表的头结点加入最小堆中，注意加入的方法 heap.Push(h,xxx)
			heap.Push(h, lists[i])
		}
	}
	dummyHead := new(ListNode)
	pre := dummyHead
	for h.Len() > 0 {
		//获取最小堆的堆定元素，即最小的
		tmp := heap.Pop(h).(*ListNode)
		if tmp.Next != nil {
			//被取出的节点还有next的话就加入最小堆
			heap.Push(h, tmp.Next)
		}
		pre.Next = tmp
		pre = pre.Next
	}
	return dummyHead.Next
}
```



**分治方法：**

```go
func mergeKLists(lists []*ListNode) *ListNode {
	length := len(lists)
	if length < 1 {
		return nil
	}
	if length == 1 {
		return lists[0]
	}
	num := length / 2
	left := mergeKLists(lists[:num]) //一直分割然后合并返回，最后一次返回的就是完整的链表
	right := mergeKLists(lists[num:])
	return mergeTwoLists1(left, right)
}

func mergeTwoLists1(l1 *ListNode, l2 *ListNode) *ListNode {
	if l1 == nil {
		return l2
	}
	if l2 == nil {
		return l1
	}
	if l1.Val < l2.Val {
		l1.Next = mergeTwoLists1(l1.Next, l2)
		return l1
	}
	l2.Next = mergeTwoLists1(l1, l2.Next)
	return l2
}
```

傻瓜方法遍历k-1遍

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

func mergeKLists(lists []*ListNode) *ListNode {
    dummy := new(ListNode)
	p := dummy.Next
	for i := 0; i < len(lists); i++{
        p = mergeTwoLists(p,lists[i])
    }
    return p
}
func mergeTwoLists(l1, l2 *ListNode) *ListNode {
	dummy := new(ListNode)
	p := dummy
	p1, p2 := l1, l2
	for p1 != nil && p2 != nil {
		if p1.Val > p2.Val {
			p.Next = p2
			p2 = p2.Next
		} else {
			p.Next = p1
			p1 = p1.Next
		}
		p = p.Next
	}
	if p1 == nil {
		p.Next = p2
	} else {
		p.Next = p1
	}
	return dummy.Next
}
```

**知识点：**

. 后跟括号，是对象类型转换，只有结构对象才能执行类型动态转换/查询

Pop中为什么删除最后一个	

```go
x := old[n-1]     //old[n-1]是最小元素，堆顶？查查看最小堆概念
*h = old[0 : n-1] //切片删除old[n-1] endIndex-1即 0~n-2

源码：
Pop() interface{} // remove and return element Len() - 1. 可能是源码把他放到了最后
func Pop(h Interface) interface{} {
        n := h.Len() - 1
        h.Swap(0, n)
        down(h, 0, n)
        return h.Pop()
}
```

从源码可以看出，实际的弹出步骤是将第一个元素与最后一个元素进行互换（所以要将最后一个删了，因为那是第一个弹出的，然后对一个元素进行**下沉**，最后移除并返回最后一个元素

