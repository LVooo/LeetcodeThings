 # 记录每日一题
## *随笔（C++）* start at 2022.12.11
&nbsp;

## **贪心**
- **12.11：**[1827. 最少操作使数组递增](https://leetcode.cn/problems/minimum-operations-to-make-the-array-increasing/)   
  从左向右依次遍历，使每一个数都满足比前一个数大1，count只需加上两者相减的值

## **循环遍历**
- **12.12：**[1781. 所有子字符串美丽值之和](https://leetcode.cn/problems/sum-of-beauty-of-all-substrings/description/)  
三层循环遍历，遍历到s字符串的每一个子字符串，从i=0开始往下每一次j=i，遍历j，每一次计算最大频率数和最小频率数（i相当于起点，j相当于终点）