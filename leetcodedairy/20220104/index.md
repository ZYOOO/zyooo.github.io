# 2022-01-04


## 19.删除链表的倒数第N个节点

![img](https://s2.loli.net/2022/01/04/QxKTIfwJglMjv34.png)

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := new(ListNode)
    dummy.Next = head
    x := findFromEnd(dummy,n+1) //找到倒数第n+1个然后将倒数第n个删除
    x.Next = x.Next.Next
    return dummy.Next
}
func findFromEnd(head *ListNode,k int) *ListNode{
    p1,p2 := head,head
    for i := 0; i < k; i++{
        p1 = p1.Next
    }
    for p1 != nil{
        p1 = p1.Next
        p2 = p2.Next
    }
    return p2
}
```

## 知识点：找到链表的中点

利用快慢指针，一个步长为1，一个步长为2，此方法在链表长度为偶数的时候，返回的是靠后的那个节点(因为其实最后一个节点的Next为nil也当做一个结点，链表长度相当于+1了)



```go
func middleNode(head *ListNode) *ListNode{
    slow,fast := head,head
    for fast != nil && fast.Next != nil{
    	slow = slow.Next
        fast = fast.Next.Next
    }
    return slow
}
```

## 知识点: 判断链表是否包含环

快慢指针追击,如果fast最终和slow相遇,则说明链表有环,如果fast最后为nil,则说明链表无环.

```go
func hasCycle(head *ListNode) bool{
    slow,fast := head,head
    for fast != nil && fast.Next != nil{
    	slow = slow.Next
        fast = fast.Next.Next
        if(slow == fast){
        	return true
        }
    }
    return false
}
```

## 知识点: 链表含环进阶版: 环的起点![img](https://s2.loli.net/2022/01/04/zB69fUDNvY71qis.jpg)



```go
func detectCycle(head *ListNode) *ListNode{
	slow,fast := head,head
	for fast != nil && fast.Next != nil{
		slow = slow.Next
		fast = fast.Next.Next
		if fast == slow{
			slow = head
			for fast != slow {
				slow = slow.Next
				fast = fast.Next
			}
			return fast
		}
	}
	return nil
}
```

## 知识点: 两个链表是否相交

A ,B长度不一致,相差A-B,但是A+B = B+A,逆过来连接可以把A,B之间的差给补齐,保证了C对齐(如果存在)

如果不存在就nil就相当于c1,解决乐正确返回nil的问题

```go
func getIntersectionNode(headA,headB *ListNode) *ListNode{
	p1,p2 := headA,headB
	for p1 != p2{
		if p1 == nil{ //此时如果有c的话就到了c的末尾了,开始到B
			p1 = headB
		}else{
			p1 = p1.Next
		}
		if p2 == nil{
			p2 = headA
		}else{
			p2 = p2.Next
		}
	}
	return p1
}
```

## 160.相交链表

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

![image-20220104114255332](https://s2.loli.net/2022/01/04/4NDT7gcLJyM3mZq.png)

题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**用上面的相交解决O(1),可以一用map去存储,但是时间复杂度为O(m+n)**



## 26.删除排序数组中的重复项





```go
func removeDuplicates(nums []int) int {
    if len(nums) == 0{
        return 0
    }
    slow,fast := 0,0
    for fast < len(nums){
        if nums[slow] != nums[fast]{
            slow ++
            nums[slow] = nums[fast]
        }
        fast++
    }
    return slow+1
}
```

## 27.移除元素

![img](https://s2.loli.net/2022/01/05/DTz42j6cqGOlKr7.png)

```go
func removeElement(nums []int, val int) int {
	slow, fast := 0, 0
	for fast < len(nums) {
		if nums[fast] != val {
			nums[slow] = nums[fast]
			slow++
		}
		fast++
	}
	return slow
}
```

## 283.移动零

[1,0,2,0,4]  ->  [1,2,4,0,0]

```go
func removeElement(nums []int, val int) int {
	slow, fast := 0, 0
	for fast < len(nums) {
		if nums[fast] != val {
			nums[slow] = nums[fast]
			slow++
		}
		fast++
	}
	return slow
}
func moveZeroes(nums []int){
	p := removeElement(nums,0)
	for ; p < len(nums) ; p++{
		nums[p] = 0
	}
}
```


