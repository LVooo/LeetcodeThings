 # 记录每日一题
## *随笔（C++）* start at 2022.12.11
&nbsp;

## **贪心**
- **12.11：**[1827. 最少操作使数组递增](https://leetcode.cn/problems/minimum-operations-to-make-the-array-increasing/)   
  从左向右依次遍历，使每一个数都满足比前一个数大1，count只需加上两者相减的值
- **12.16：** [1785. 构成特定和需要添加的最少元素](https://leetcode.cn/problems/minimum-elements-to-add-to-form-a-given-sum/description/)（详解在数组部分）

## **循环遍历**
- **12.12：**[1781. 所有子字符串美丽值之和](https://leetcode.cn/problems/sum-of-beauty-of-all-substrings/description/)  
三层循环遍历，遍历到s字符串的每一个子字符串，从i=0开始往下每一次j=i，遍历j，每一次计算最大频率数和最小频率数（i相当于起点，j相当于终点）

## **数组**
- **12.13：**[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)  
用vector数组来表示26位字母位数，每出现一次在对应位置上加1
- **12.16：** [1785. 构成特定和需要添加的最少元素](https://leetcode.cn/problems/minimum-elements-to-add-to-form-a-given-sum/description/)（需解决**溢出问题**！）  
先使用accumulate函数求出原始数组的总和(注意要用long值，int会溢出内存)，用其绝对值，再计算剩余所需值：
```cpp
long long left = abs(accumulate(nums.begin(), nums.end(), 0l) - goal); // 0L代表返回的和是长整型
return left / limit + (left % limit != 0); // 记住这个求剩余数的公式！！
```

## **位运算**
- **12.13：**[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)
由于字符集仅有26个，我们可以使用一个长度为26的二进制数字来表示字符集合，该二进制数字使用32位带符号整型变量（int类型）即可。出现第几个字符就将其左移对应位数，再进行或运算。最后判断是否为$2^{26}-1$（即所有0-25位上的数字都为1，其余为0）返回true。

## **字符串模拟**
- **12.15：**[1945. 字符串转化后的各位数字之和](https://leetcode.cn/problems/sum-of-digits-of-string-after-convert/description/)  
int类型到字符串的转换：```to_string()```  
字符串类型转int：```stoi()```  
字符或字符串类型到int类型的转换：```(c - 'a') (c - '0')```or通过ascii码转换
```cpp
char a = '0';
int ia = (int) a; // 输出48，因为ascii码的数字0从48开始
int x = ia - 48; // 输出0
```