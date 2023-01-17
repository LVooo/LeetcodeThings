 # 记录每日一题
## *随笔（C++）* start at 2022.12.11
&nbsp;

## **贪心**
- **12.11：**[1827. 最少操作使数组递增](https://leetcode.cn/problems/minimum-operations-to-make-the-array-increasing/)   
从左向右依次遍历，使每一个数都满足比前一个数大1，count只需加上两者相减的值
- **12.16：**[1785. 构成特定和需要添加的最少元素](https://leetcode.cn/problems/minimum-elements-to-add-to-form-a-given-sum/description/)（详解在数组部分）
- **12.21：**[1753. 移除石子的最大得分](https://leetcode.cn/problems/maximum-score-from-removing-stones/description/)  
先排序，排序后分为两种情况：
1. a+b <= c，此时可以拿c去跟a或b中每一个石子两两配对，答案为a+b
2. a+b > c，此时先拿c去跟a或b中最大的匹配，当c为0时，a和b再开始两两配对。所以答案为c+(a+b-c)/2  
我们可以用max(a,b,c)来求出排序后的c，然后用abc的和减去最大值求出排序后的a+b来模拟排序
- **12.24**[1754. 构造字典序最大的合并字符串](https://leetcode.cn/problems/largest-merge-of-two-strings/)  
拿两个字符串做排序合并时，可以用while两个字符串遍历是否到结尾当判断条件。本题因为要找后缀和最大的，所以当遍历到i和j的元素相等时需要考虑他们后面的元素谁更大；此时加入k来遍历到他们后缀不同的位置，判断大小进行push_back操作即可。最后别忘了加上没有遍历完的那个的剩余元素。  
- **12.27**[2027. 转换字符串的最少操作次数](https://leetcode.cn/problems/minimum-moves-to-convert-string/)  
贪心模拟不重复滑动窗口只需一次遍历如果找到对应元素直接```i += 2```即可
- **1.4**[1802. 有界数组中指定下标处的最大值](https://leetcode.cn/problems/maximum-value-at-a-given-index-in-a-bounded-array/description/)  
贪心＋二分+等差数列的一道题，找到中点最大值，左右分别递减1求和后小于最大Sum值即可，注意减到1之后后面的数就都为1了

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
- **12.31：**[2037. 使每位学生都有座位的最少移动次数](https://leetcode.cn/problems/minimum-number-of-moves-to-seat-everyone/description/)  
对数组进行排序后再遍历，找对位差
- **1.9：**[1806. 还原排列的最少操作步数](https://leetcode.cn/problems/minimum-number-of-operations-to-reinitialize-a-permutation/description/)  
复制原数组，进行一些操作变换使之再回到原数组所需要的操作步数

## **位运算**
- **12.13：**[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)
由于字符集仅有26个，我们可以使用一个长度为26的二进制数字来表示字符集合，该二进制数字使用32位带符号整型变量（int类型）即可。出现第几个字符就将其左移对应位数，再进行或运算。最后判断是否为$2^{26}-1$（即所有0-25位上的数字都为1，其余为0）返回true。
- **12.29：**[2032. 至少在两个数组中出现的值](https://leetcode.cn/problems/two-out-of-three/description/)  
因为该题一共只有三个数组，可以用二进制位运算代表三位，如果只出现一次的话那么他只存在于第一位中，此时他的二进制最低位的值-1为0，所以做与运算即可
- **1.01：**[2351. 第一个出现两次的字母](https://leetcode.cn/problems/first-letter-to-appear-twice/)  
找一个字符串中最先出现两次的字母，可以用哈希表或者位运算

## **模拟**
- **12.15：**[1945. 字符串转化后的各位数字之和](https://leetcode.cn/problems/sum-of-digits-of-string-after-convert/description/)  
int类型到字符串的转换：```to_string()```  
字符串类型转int：```stoi()```  
字符或字符串类型到int类型的转换：```(c - 'a') (c - '0')```or通过ascii码转换
```cpp
char a = '0';
int ia = (int) a; // 输出48，因为ascii码的数字0从48开始
int x = ia - 48; // 输出0
```
- [2011. 执行操作后的变量值](https://leetcode.cn/problems/final-value-of-variable-after-performing-operations/)  
一次遍历找标志性+或—来对结果进行加减，使用字符串的find函数
```cpp
operation.find('-') != string::npos
```
- [2042. 检查句子中的数字是否递增](https://leetcode.cn/problems/check-if-numbers-are-ascending-in-a-sentence/)  
查看一段字符串中存在的数字是否是遵循严格递增的；因为是字符串所以要注意连续数字即如何把两个数字的字符合成一个数
```cpp
while (pos < n && isdigit(s[pos]))
{
    cur = cur*10 + s[pos] - '0';
    pos ++;
}
```
- **1.8：**[2185. 统计包含给定前缀的字符串](https://leetcode.cn/problems/counting-words-with-a-given-prefix/description/)  
可以用compare函数比较两个字符串是否相等
```cpp
if (word.compare(0, pref.size(), pref) == 0) { // 参数分别为起始索引，包含字符，与哪个字符串进行比较；如果相等值为0
    res++;
}
```
- **1.12：**[1807. 替换字符串中的括号内容](https://leetcode.cn/problems/evaluate-the-bracket-pairs-of-a-string/description/)  
熟练使用哈希计数以及字符串模拟；可以开创新的字符串来存储结果数据
- **1.15：**[2293. 极大极小游戏](https://leetcode.cn/problems/min-max-game/)  
在循环外开辟的数组vector容易内存泄漏，因为如果要重复使用到索引的话需要提前分配空间`vector<int> newNums(nums.size()/2);`

## **字符串匹配**
- **12.17：**[1764. 通过连接另一个数组的子数组得到一个数组](https://leetcode.cn/problems/form-array-by-concatenating-subarrays-of-another-array/description/)  
用一个数组的子数组来构成不相交的另外一个二维数组。利用双指针，第一个i指向需要匹配的二维数组groups，第二个指针k指向遍历数组nums。如果找到对应数组，则k加上二维数组group[i]的长度，并将groups的下标i加1；若nums[k]与group[i]不相同，那么直接令k加1。**贪心**遍历结束时判断i是否等于groups本身长度即可。
```cpp
bool check(vector<int> &g, vector<int> &num, int k)
{
    for (int i = 0; i < g.size(); i ++)
    {
        if (num[k+i] != g[i])
        {
            return false;
        }
    }
    return true;
}

bool canChoose(vector<vector<int>>& groups, vector<int>& nums) {
      int i = 0;
      for (int k = 0; k < nums.size() && i < groups.size();)
      {
          if (check(groups[i], nums, k))
          {
              k += groups[i].size();
              i ++;
          } else
          {
              k ++;
          }
      }

      return i == groups.size();
}
```

## **图论**
- **12.19：**[1971. 寻找图中是否存在路径](https://leetcode.cn/problems/find-if-path-exists-in-graph/description/)  
判断源点与目标点是否连通，结合DFS、BFS、并查集于一身的简单好题。

## **二分查找**
- **12.20：**[1760. 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/description/)  
此题使用操作数作为判断条件来夹出数组内每一个数在满足最大操作数的条件下能分成的最小值。

## **找规律**
等差数列：
- **12.25：**[1739. 放置盒子](https://leetcode.cn/problems/building-boxes/description/)  
摆放盒子，如果一个盒子上面有盒子，则这个盒子的四周应被完全环绕，所以此时靠墙是用盒最少的办法，求最后摆放在地上的所有盒子数量。
- **12.27：**[1759. 统计同构子字符串的数目](https://leetcode.cn/problems/count-number-of-homogenous-substrings/)  
一个字符串所有字符相同则为同构子字符串，且子字符串是字符串中的一个连续序列。双层for循环会超时，用等差数列求和方法一次遍历即可。

## **双指针**
- **12.28：**[1750. 删除字符串两端相同字符后的最短长度](https://leetcode.cn/problems/minimum-length-of-string-after-deleting-similar-ends/)  
利用双指针删除相同的前缀和后缀，要多加注意几个判定条件。
- **1.16：**[1813. 句子相似性 III](https://leetcode.cn/problems/sentence-similarity-iii/)  
利用双指针找出其中小的字符串通过**一次**插入是否能组成大的字符串

## **数学**
- **1.6：**[2180. 统计各位数字之和为偶数的整数个数](https://leetcode.cn/problems/count-integers-with-even-digit-sum/)  
对一个整数取余10即为它个位上的数字，再对它除以10下次再取余则能得到它十位上的数字
- **1.17：**[1814. 统计一个数组中好对子的数目](https://leetcode.cn/problems/count-nice-pairs-in-an-array/description/)  
通过给出的公式我们可以将其转换变为f(i) = f(j)的形式，f(i)代表数组i位置元素与其逆置后元素的和，逆置方法：
```cpp
while (temp > 0) // 逆置元素方法，temp代表原数据的备份，j代表逆序后的值
{
    j = j*10 + temp % 10;
    temp = temp/10;
}
```

## **滑动窗口**
- **1.7：**[1658. 将 x 减到 0 的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/description/)  
移除的前缀和加上后缀和的和恰好等于x；可以使用left表示前缀的边界，如果left=-1则代表空前缀，如果right=n则代表空后缀。当left右移时，为了控制和为x，right也应右移，类似于滑动窗口。

## **哈希表**
- **1.11：**[2283. 判断一个数的数字计数是否等于数位的值](https://leetcode.cn/problems/check-if-number-has-equal-digit-count-and-digit-value/)  
字符串转数字：```int k = num[i] - '0'```然后再把k当作键值对存入哈希表中；查找对应值时记得也要使用```nums[i] - '0'```

- **1.13：**[2287. 重排字符形成目标字符串](https://leetcode.cn/problems/rearrange-characters-to-make-target-string/)  
拿空间换时间，建立两个哈希表分别对应两个字符串，最后遍历目标字符串找对应字符出现次数的最小公倍数即为答案