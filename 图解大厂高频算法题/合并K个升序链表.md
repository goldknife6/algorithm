## [题目介绍](https://leetcode-cn.com/problems/merge-k-sorted-lists/ "原题链接")

给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

### 示例 1

> _输入_：lists = [[1,4,5],[1,3,4],[2,6]]
>
> _输出_：[1,1,2,3,4,4,5,6]
>
> _解释_：链表数组如下：
> [
> 1->4->5,
> 1->3->4,
> 2->6
> ]
> 将它们合并到一个有序链表中得到。
> 1->1->2->3->4->4->5->6

## 题目解答

这道题的解答依赖 [「图解大厂面试高频算法题」链表专题-合并两个有序链表](https://blog.csdn.net/Flames_and_Lights/article/details/121881512)，如果有不知道如何合并两个有序链表或没有思路的同学们，可以先学习一下。

### 两两合并(分而治之)

#### 思路和算法

![](https://img-blog.csdnimg.cn/7c63f1e5a3fe457089788ae018d02a9a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA55-l5pil6Lev6YeR5YiA,size_18,color_FFFFFF,t_70,g_se,x_16)

用分治的方法进行合并，将多个链表配对并将同一对中的链表进行合并，重复这一过程，直到我们得到了最终的有序链表。

**初始状态**

初始状态一共有六个链表。

![](https://img-blog.csdnimg.cn/e29987dbfdd34b37bea2d96bf80edb39.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA55-l5pil6Lev6YeR5YiA,size_20,color_FFFFFF,t_70,g_se,x_16)

**第一次迭代**

依次对链表 _{j=0, j+i=1}_ ，_{j=2, j+i=3}_，_{j=4, j+i=5}_ 中的链表进行合并，合并完之后我们还剩下 6/2=3 个链表需要继续合并。

![](https://img-blog.csdnimg.cn/0c272f8431534d6cb04e2eb1be20c760.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA55-l5pil6Lev6YeR5YiA,size_20,color_FFFFFF,t_70,g_se,x_16)

**第二次迭代**

依次对链表 _{j=0, j+i=2}_ ，_{j=4, j+i=6}_ 中的链表进行合并，其中*灰色节点*并没有链表，也没有对链表对 _{j=4, j+i=6}_ 进行合并，这里只是画了出来方便讲解。最终还剩下两条链表。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fc4224bb424e4a8c83976acfe41d52ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA55-l5pil6Lev6YeR5YiA,size_20,color_FFFFFF,t_70,g_se,x_16)

**第三次迭代**

最后对链表 _{j=0, j+i=4}_ 中的链表进行合并，最终只剩下一条链表，这条链表就是我们的答案。

![](https://img-blog.csdnimg.cn/f95c5049720f4ee1ac0fed16d9d09aee.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA55-l5pil6Lev6YeR5YiA,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 代码实现

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        for (int i = 1; i < lists.length; i = i*2) {
            for (int j = 0; j + i < lists.length; j = j + i*2) {
                lists[j] = mergeTwoLists(lists[j], lists[j+i]);
            }
        }
        return lists[0];
    }
    ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dump = new ListNode();
        ListNode head = dump;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                head.next = list1;
                list1 = list1.next;
            } else {
                head.next = list2;
                list2 = list2.next;
            }
            head = head.next;
        }
        if (list1 != null) {
            head.next = list1;
        } else {
            head.next = list2;
        }
        return dump.next;
    }
}
```

#### 复杂度分析

- _时间复杂度_：每一轮都需要对 N 个节点进行合并，总共需要合并 logN 轮，所以时间复杂度为 O(NlogN) 。
- _空间复杂度_：O(1) 。

## 其他

> **图解大厂面试高频算法题**专题文章主旨是: 根据*二八法则*的原理，以付出 20%的时间成本，获得 80%的刷题的收益，让那些想进互联网大厂或心仪公司的人少走些弯路。
>
> 本专题还在持续更新 ing~ 所有文章、图解和代码全部是金刀亲手完成。内容全部放在了[github](https://github.com/glodknife "github")和[gitee](https://gitee.com/goldknife6 "gitee")方便小伙伴们阅读和调试，另外还有更多小惊喜等你发现~
>
> 如果你喜欢本篇文章，PLZ 一键三连（关注、点赞、在看）。
