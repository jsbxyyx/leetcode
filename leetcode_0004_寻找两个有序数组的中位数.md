给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:
```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```
示例 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

```
/*
解题思路：初看这题可以理解为，把两个数组合并后排序，再找到(m + n) / 2的数即中位数，
而有个要求是时间复杂度是log(m+n),我们知道所有排序算法时间复杂最低也只能是nlogn，所以用排序来解决是不行的
既然题目是要求找中位数，我们可以从中位数的概念入手，中位数是指在数组中有一半数比它小，一半数比它大
由于这两个数组都是有序数组，我们假设为nums1数组和nums2数组，如果我们从下标为i的地方把nums1数组分为两半，左边的数肯定比nums1[i]小，右边比nums2[i]大
同理把nums2数组从下标为j的地方分为两半
left                                 |  right
nums1[0], nums1[1], ..., nums1[i-1]  |  nums1[i], nums1[i+1], ..., nums1[m-1]
nums2[0], nums2[1], ..., nums2[j-1]  |  nums2[j], nums2[j+1], ..., nums2[n-1]
那只要满足两个条件，我们就可以顺利找到中位数
1. len(left)==len(right) 相当于 i+j=m-i+n-j
2. min(right) >= max(left) 相当于 nums2[j] >= nums2[i-1] && nums1[i] >= nums2[j-1]
对于第一个条件:
我们可以推出j = (m + n) / 2 - i，当i的值确定，j的值就确定了，也就是说我们只要把i的值确定下来满足第二个条件就行
对于第二个条件:
我们需要满足 nums2[j] >= nums1[i-1] && nums1[i] >= nums2[j-1]，不满足的条件有两种:
1. nums2[j] < nums1[i-1]，说明i的值太大了，你想想nums1[i]的值肯定大于nums1[i-1]，如果i继续加大，条件永远无法满足，所以要减小i
2. nums1[i] < nums2[j-1]，说明i的值太小了，需要加大i的值
明白了这些，我们只需要根据条件去增加或缩小i的值就可以了，由于是有序数组，我们可以用二分法变化i的值
最后我们再看看满足条件nums2[j] >= nums1[i-1] && nums1[i] >= nums2[j-1]是怎么得到中位
max(left) = max(nums1[i-1], nums2[j-1])
min(right) = min(nums1[i], nums1[j])
如果数组之和是偶数，中位数 = (max(left) + min(right)) / 2
如果数组之和是奇数，中位数 = max(left)或者min(right)，那取哪个呢，哪边多一个就取哪边
我们可以将j = (m + n) / 2 - i改为j = (m + n + 1) / 2 - i来保证j的值大些，从而保证len(left) > left(right)
也可以将j = (m + n) / 2 - i改为j = (m + n - 1) / 2 - i来保证j的值小些，从而保证len(right) > left(left)
最后就是一些特殊情况了:
首先由于j是由i决定的，j = (m + n) / 2 - i, i的取值范围是0到m，也就是说j可能的值是(n - m) / 2 到(m + n) / 2，由于j不能为负，那么反推n >= m

特殊处理:
如果i == 0或者i == m时，nums1数组有一边是没有值的
同理j == 0或者j == n时，nums2数组有一边也是没有值的
*/
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1 == null && nums2 == null) {
            throw new IllegalArgumentException("nums1 and nums2 is null");
        }
        int m = nums1.length;
        int n = nums2.length;
        // 保证 m <= n
        if (m > n) {
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
            int tmp = m;
            m = n;
            n = tmp;
        }
        int iMin = 0, iMax = m;
        //保证len(left) > len(right)
        int halfLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) >>> 1;
            int j = halfLen - i;
            // i的值太大，这里注意i的值不断减小有可能i变为0，j变为n，必须保证大于0，j也必须小于n
            if (i > iMin && nums2[j] < nums1[i - 1]) {
                iMax = i - 1;
            } else if (i < iMax && nums1[i] < nums2[j - 1]) { // i 的值太小
                iMin = i + 1;
            } else {  // i的值刚好
                int maxLeft = 0;
                // 特殊情况i == 0，数组nums1的left为空，只能取nums2的left的最大值
                if (i == 0) {
                    maxLeft = nums2[j - 1];
                } else if (j == 0) {
                    maxLeft = nums1[i - 1];
                } else {
                    maxLeft = Math.max(nums1[i - 1], nums2[j - 1]);
                }
                if ((m + n) % 2 == 1) {
                    return maxLeft;
                }

                int minRight = 0;
                if (i == m) {
                    minRight = nums2[j];
                } else if (j == n) {
                    minRight = nums1[i];
                } else {
                    minRight = Math.min(nums2[j], nums1[i]);
                }

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```
