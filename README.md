# LeetcodeThings
## *随笔（C++）*
&nbsp;
## 1. 字符串
- **字符串与数字转换**<br>
C++中当需要将字母与数字进行转换时（加减乘除）可以直接用其减去对应的起始字符（‘a’）再与数字相加，
```cpp
coordinates[0] - 'a'
```
如[1812. 判断国际象棋棋盘中一个格子的颜色](https://leetcode.cn/problems/determine-color-of-a-chessboard-square/description/)
- 


---
## 2. 哈希表
- **用数组vector模拟哈希表计数**  
当涉及26位字母计数时，可以使用vector来模拟26位字母哈希表，每一位代表对应字母，当出现一次时对应位置数值加1，
```cpp
vector<int> count(26);
```
如[1781. 所有子字符串美丽值之和](https://leetcode.cn/problems/sum-of-beauty-of-all-substrings/description/)
- 


---
## 3. 数组
- **在数组原位置操作**
为了提高效率，通常可以在数组原位置进行操作（赋值等），避免多次循环影响时间复杂度。  
如[1827. 最少操作使数组递增](https://leetcode.cn/problems/minimum-operations-to-make-the-array-increasing/) 
- **模拟全字母**  
用vector数组来表示26位字母位数，每出现一次在对应位置为true（或计数+1）
```cpp
vector<int> exist(26);
for (auto c : sentence) {
    exist[c - 'a'] = true;
}
```
如[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)


---
## 4. 数学
- **进制转换**<br>
十进制转任何进制都是采用整数除n取余倒序排列，小数乘n取整顺序排列的方法。
三进制问题比如32.12转三进制<br>
整数部分：<br>
32除以3商10余2<br>
10除以3商3余1<br>
3除以3商1余0<br>
1除以3商0余1<br>
所以整数部分是: 1012<br>
再比如用三进制表示就是120，是这样算的，1*9+2*3+0*0=15<br>
可以循环判断数字转换成三进制后是否某一位出现2来判断它是否是不同三的幂的和，
```cpp
while (n) {
    if (n % 3 == 2) {
        return false;
    }
    n /= 3;
}
```
如[1780. 判断一个数字是否可以表示成三的幂的和](https://leetcode.cn/problems/check-if-number-is-a-sum-of-powers-of-three/description/)
- 


---
## 5. 位运算
- **模拟全字母**<br>
由于字符集仅有26个，我们可以使用一个长度为26的二进制数字来表示字符集合，该二进制数字使用32位带符号整型变量（int类型）即可。出现第几个字符就将其左移对应位数，再进行或运算。最后判断是否为$2^{26}-1$（即所有0-25位上的数字都为1，其余为0）返回true。
```cpp
state |= 1 << (c - 'a');
```
如[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)