给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

解题：
1. 
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode root = new ListNode(0);
        ListNode cursor = root;
        int carry = 0;
        while(l1 != null || l2 != null || carry != 0) {
            int l1Val = l1 != null ? l1.val : 0;
            int l2Val = l2 != null ? l2.val : 0;
            int sumVal = l1Val + l2Val + carry;
            carry = sumVal / 10;
            
            ListNode sumNode = new ListNode(sumVal % 10);
            cursor.next = sumNode;
            cursor = sumNode;
            
            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;
        }
        
        return root.next;
    }
}
```

2. 先把两个链表转成数字，然后相加，注意溢出（BigInteger）
```
import java.math.BigInteger;
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        StringBuilder sb1 = new StringBuilder().append(l1.val);
        ListNode temp = l1;
        while (temp.next != null) {
            temp = temp.next;
            sb1.append(temp.val);
        }
        BigInteger i1 = new BigInteger(sb1.reverse().toString());

        StringBuilder sb2 = new StringBuilder().append(l2.val);
        temp = l2;
        while (temp.next != null) {
            temp = temp.next;
            sb2.append(temp.val);
        }
        BigInteger i2 = new BigInteger(sb2.reverse().toString());

        BigInteger sum = i1.add(i2);
        String sumStr = sum.toString();
        ListNode result = new ListNode(Integer.parseInt(sumStr.charAt(sumStr.length() - 1) + ""));
        ListNode tempResult = result;
        for (int i = sumStr.length() - 2; i >= 0; i--) {
            tempResult.next = new ListNode(Integer.parseInt(sumStr.charAt(i) + ""));
            tempResult = tempResult.next;
        }
        return result;
    }
}
```
