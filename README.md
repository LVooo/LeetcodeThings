 # LeetcodeThings
## *随笔（C++）*
&nbsp;
## 1. 字符串
### 1.1 字符串与数字转换
C++中当需要将字母与数字进行转换时（加减乘除）可以直接用其减去对应的起始字符（‘a’）再与数字相加，
```cpp
coordinates[0] - 'a'
```
如[1812. 判断国际象棋棋盘中一个格子的颜色](https://leetcode.cn/problems/determine-color-of-a-chessboard-square/description/)

### 1.2 模拟
- **用字符串模拟数字串**    
int类型到字符串的转换：```to_string()```  
字符串类型转int：```stoi()```  
字符或字符串类型到int类型的转换：```(c - 'a') (c - '0')```or通过ascii码转换
```cpp
char a = '0';
int ia = (int) a; // 输出48，因为ascii码的数字0从48开始
int x = ia - 48; // 输出0
```
如[1945. 字符串转化后的各位数字之和](https://leetcode.cn/problems/sum-of-digits-of-string-after-convert/description/) 
- **查看一段字符串中存在的数字是否是遵循严格递增的**  
因为是字符串所以要注意连续数字即如何把两个数字的字符合成一个数
```cpp
while (pos < n && isdigit(s[pos]))
{
    cur = cur*10 + s[pos] - '0';
    pos ++;
}
```
如[2042. 检查句子中的数字是否递增](https://leetcode.cn/problems/check-if-numbers-are-ascending-in-a-sentence/) 
- **模拟加减**  
一次遍历找标志性+或—来对结果进行加减，使用字符串的**find**函数
```cpp
operation.find('-') != string::npos
```
如[2011. 执行操作后的变量值](https://leetcode.cn/problems/final-value-of-variable-after-performing-operations/) 
- **字符串比较**    
可以用compare函数比较两个字符串是否相等
```cpp
if (word.compare(0, pref.size(), pref) == 0) { // 参数分别为起始索引，包含字符，与哪个字符串进行比较；如果相等值为0
    res++;
}
``` 
如[2185. 统计包含给定前缀的字符串](https://leetcode.cn/problems/counting-words-with-a-given-prefix/description/)
- **！模拟替换操作**  
熟练使用哈希计数以及字符串模拟；可以开创新的字符串来存储结果数据  
```cpp
if (hash.count(key))
{
    res += hash[key];
}
else
{
    res += '?';
}
flag = false;
key.clear();
```
如[1807. 替换字符串中的括号内容](https://leetcode.cn/problems/evaluate-the-bracket-pairs-of-a-string/description/)
- **模拟栈**
```cpp
string removeDuplicates(string s) {
    string stk = "";
    for (char &c : s)
    {
        if (!stk.empty() && stk.back() == c) // C++最后一位索引不能用-1
        {
            stk.pop_back();
        }
        else
        {
            stk.push_back(c);
        }
    }
    return stk;
}
```
如[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)


### 1.3 贪心
- **字符串合并最大**  
拿两个字符串做排序合并时，可以用while两个字符串遍历是否到结尾当判断条件```while (word1[i] != '\0' && word2[j] != '\0')```  
加到字符串合并结果末尾时可以用```merge.push_back(word1[i++]);``` 或 ```merge += word1[i++];```  
若要合并后缀和最大的，需要在遍历到i和j的元素相等时需要考虑他们后面的元素谁更大；此时加入k来遍历到他们后缀不同的位置，判断大小后放入元素即可。最后别忘了加上没有遍历完的那个的剩余元素。  
或者：  
我们直接使用```substr()```来比较两个字符串i和j位置的后缀和大小，要记得加上越界限定条件```if (i < word1.size() && word1.substr(i) > word2.substr(j))```  
如[1754. 构造字典序最大的合并字符串](https://leetcode.cn/problems/largest-merge-of-two-strings/) 
- **模拟不重复滑动窗口**  
贪心模拟不重复滑动窗口只需一次遍历如果找到对应元素直接```i += 移动范围```即可
```cpp
for (int i = 0; i < s.size(); i ++)
    {
        if (s[i] == 'X')
        {
            res ++;
            i += 2;
        }
    }
```
如[2027. 转换字符串的最少操作次数](https://leetcode.cn/problems/minimum-moves-to-convert-string/) 
- **二分**
如[1802. 有界数组中指定下标处的最大值](https://leetcode.cn/problems/maximum-value-at-a-given-index-in-a-bounded-array/description/)  
贪心＋二分+等差数列的一道题，合理运用求和公式，二分夹出来的为最大值（尽管判断条件是拿index去找的左右求和，但用的最大值是mid，因为这里假设了index的数为最大值），注意返回长整型long。  
等差公式：
```cpp
long cal(int big, int length) // 注意返回long
{
    if (big > length)
    {
        int small = big - length;
        return (long) (big - 1 + small) * length / 2; // 求和公式(a1+an)*n/2
    }
    else
    {
        int ones = length - (big - 1); // 大于等于index的数通通为1，累加起来
        return (long) big * (big - 1) / 2 + ones; // n*(n-1)*d/2 此时公差d为1；n(n-1)/2是从1加到n-1的和，n*(n+1)/2是从1加到n的和
    }
}
```


---
## 2. 哈希表
### 2.1 用数组vector模拟哈希表计数
当涉及26位字母计数时，可以使用vector来模拟26位字母哈希表，每一位代表对应字母，当出现一次时对应位置数值加1，
```cpp
vector<int> count(26);
```
如[1781. 所有子字符串美丽值之和](https://leetcode.cn/problems/sum-of-beauty-of-all-substrings/description/)

### 2.2 纯哈希
- 用```unordered_map<int, int> count```来记录数组中出现数字的个数
```cpp
int majorityElement(vector<int>& nums) {
    unordered_map<int, int> count;
    int n = nums.size();
    for (int i = 0; i < n; i ++)
    {
        count[nums[i]]++;
        if (count[nums[i]] > (n/2))
        {
            return nums[i];
        }
    }
    return -1;
}
```
如[169. 多数元素](https://leetcode.cn/problems/majority-element/) 

- 记录数组中存在元素的下标，满足对应条件
```cpp
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    int n = nums.size();
    unordered_map<int, int> dict;
    for (int i = 0; i < n; i ++)
    {
        int num = nums[i];
        if (dict.count(num) && i - dict[num] <= k)
        {
            return true;
        }
        dict[num] = i;
    }
    return false;
}
```
如[219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

### 2.3 哈希集合
可以用```unordered_set<int>```哈希集合来存储数组中的元素再用```s2.count(s1中的num)```来判断两集合是否有相同元素
```cpp
unordered_set<int> s1, s2;
for (auto &num : nums1)
{
    s1.insert(num);
}
```
如[349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

### 2.4 字符转换  
- **字符串转数字：**  
```int k = num[i] - '0'```然后再把k当作键值对存入哈希表```unordered_map<int, int> hash```中；查找对应值时记得也要使用```nums[i] - '0'```  
如[2283. 判断一个数的数字计数是否等于数位的值](https://leetcode.cn/problems/check-if-number-has-equal-digit-count-and-digit-value/)
- **年月日转换：**  
年份转换：`int year = stoi(date.substr(0, 4));`  
每月多少天的数组：`vector<int> arr = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};`  
如[1154. 一年中的第几天](https://leetcode.cn/problems/day-of-the-year/description/)

### 2.5 目标字符串  
拿空间换时间，建立两个哈希表分别对应两个字符串，最后遍历目标字符串找对应字符出现次数的最小公倍数即为答案
```cpp
unordered_map<char, int> hashs, hasht;
for (auto &c : s)
{
    hashs[c] ++;
}

for (auto &t : target)
{
    hasht[t] ++;
}

int res = INT_MAX;
for (auto &t : target)
{
    res = min(res, hashs[t] / hasht[t]);   
}
return res;
```
如[2287. 重排字符形成目标字符串](https://leetcode.cn/problems/rearrange-characters-to-make-target-string/)

### 2.6 分词
可以设置一个起始位置变量和一个结束位置变量，再使用`substr`来截取一段字符串单词
```cpp
vector<string> words;
int s = 0, e = 0, n = text.size();
while (s < n)
{
    if (s < n && text[s] == ' ')
    {
        s ++;
    }

    e = s + 1;
    while (e < n && text[e] != ' ')
    {
        e ++;
    }
    words.push_back(text.substr(s, e-s));
    s = e+1;
}
```
如[1078. Bigram 分词](https://leetcode.cn/problems/occurrences-after-bigram/description/)


---
## 3. 数组
### 3.1 模拟
- **模拟全字母**：  
用vector数组来表示26位字母位数，每出现一次在对应位置为true（或计数+1）
```cpp
vector<int> exist(26);
for (auto c : sentence) {
    exist[c - 'a'] = true;
}
```
如[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)
- **模拟数学运算**
可以使用```iota()```函数来初始化数组从0到n  
可以使用```if(i & 1)```可以位运算来判断奇偶，**奇数** i 的最低位是1，**偶数** i 的最低位是0  
如[1806. 还原排列的最少操作步数](https://leetcode.cn/problems/minimum-number-of-operations-to-reinitialize-a-permutation/description/)  
复制原数组，进行一些操作变换使之再回到原数组所需要的操作步数；可以初始化相同的两个数组，改变其中一个另一个则为目标原数组  
如[2293. 极大极小游戏](https://leetcode.cn/problems/min-max-game/)  
在循环外开辟的数组vector容易内存泄漏，因为如果要重复使用到索引的话需要提前分配空间`vector<int> newNums(nums.size()/2);`

### 3.2 贪心
- **在数组原位置操作**  
为了提高效率，通常可以在数组原位置进行操作（赋值等），避免多次循环影响时间复杂度。  
如[1827. 最少操作使数组递增](https://leetcode.cn/problems/minimum-operations-to-make-the-array-increasing/)
- **求当前最优值**，好像有很多循环..
```cpp
long long left = abs(accumulate(nums.begin(), nums.end(), 0l) - goal); // 0L代表返回的和是长整型
return left / limit + (left % limit != 0); // 记住这个求剩余数的公式！！
```
如[1785. 构成特定和需要添加的最少元素](https://leetcode.cn/problems/minimum-elements-to-add-to-form-a-given-sum/description/)
- **每次在原数组上操作再排序，求最优值**
```cpp
int maximumScore(int a, int b, int c) {
    vector<int> all = {a, b, c};
    int res = 0;
    while (true)
    {
        sort(all.begin(), all.end());
        if (all[1] == 0)
        {
            break;
        }
        res += 1;
        all[1] --;
        all[2] --;
    }
    return res;
}
```
如[1753. 移除石子的最大得分](https://leetcode.cn/problems/maximum-score-from-removing-stones/description/)

### 3.3 字符串匹配 
- **双指针指向两个数组**  
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
如[1764. 通过连接另一个数组的子数组得到一个数组](https://leetcode.cn/problems/form-array-by-concatenating-subarrays-of-another-array/description/) 
如[1813. 句子相似性 III](https://leetcode.cn/problems/sentence-similarity-iii/)  
利用双指针找出其中小的字符串通过**一次**插入是否能组成大的字符串  
此题首先要对字符串进行分割得到两个由空格分割后的字符串数组，再在数组中进行比对查找  
```cpp
// 字符串分割：
vector<string> split(const string& s, char target)
{
    vector<string> res;
    int pos = 0;
    string value = "";
    while(pos < s.size())
    {
        while (pos < s.size() && s[pos] != target)
        {
            value += s[pos];
            pos ++;
        }
        res.push_back(value);
        value = "";
        pos ++;
    }
    return res;
}

// 双指针查找：
bool areSentencesSimilar(string sentence1, string sentence2) {
    vector<string> s1 = split(sentence1, ' ');
    vector<string> s2 = split(sentence2, ' ');

    cout << s1.size() << " " << s2.size() << endl;

    int i = 0, j = 0;
    // 因为只能插入一次，不能前后都插入
    while (i < s1.size() && i < s2.size() && s1[i] == s2[i]) // i从前往后对比
    {
        i ++;
    }
    while(j < s1.size() - i && j < s2.size() - i && s1[s1.size() - j - 1] == s2[s2.size() - j - 1]) // j从后往前对比
    {
        j ++;
    }
    cout << i << " " << j << endl;

    return i + j == min(s1.size(), s2.size()); // 当遍历完的i和j相加等于最小的字符串长度后结果为true
}
```

- **双指针指向单个数组**  
使用while循环来限定i < j的判定条件，注意每层i++或j--的循环都要加上i小于等于j的判断条件否则会越界。如下面题目是找一个字符串左右两端相等的前缀和后缀和：
```cpp
int minimumLength(string s) {
    int n = s.size();
    int i = 0, j = n - 1;
    while (i < j && s[i] == s[j])
    {
        char val = s[i];
        while (i <= j && s[i] == val)
        {
            i ++;
        }
        while (i <= j && s[j] == val)
        {
            j --;
        }
    }
    return j - i + 1;
}
```
如[1750. 删除字符串两端相同字符后的最短长度](https://leetcode.cn/problems/minimum-length-of-string-after-deleting-similar-ends/)

### 3.4 计数
计算数组中只出现一次的数，可以两次循环遍历计算count数量是否为1
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i ++)
        {
            int cur = nums[i];
            int count = 0;
            for (int j = 0; j < n; j ++)
            {
                if (nums[j] == cur)
                    count ++;
            }
            if (count == 1)
            {
                return cur;
            }
        }
        return -1;
    }
};
```
### 3.5 二分查找
一般二分查找的下界为1，上界为数组nums中的最大值。当二分查找到y（满足要求的答案）时，那么所有大于等于y的是满足要求的，而小于y的都是不满足要求的，这个y就是我们二分查找夹出来的答案。当小于等于判断条件时我们调整上届，当越过判断条件时我们调整下届。
```cpp
int left = 1, right = *max_element(nums.begin(), nums.end());
int res = 0;
while (left <= right)
{
    int op = 0;
    int mid =left + (right - left) / 2;
    for (auto &num : nums)
    {
        op += (num - 1) / mid;
    }
    if (op <= maxOperations)
    {
        res = mid;
        right = mid - 1;
    }
    else
    {
        left = mid + 1;
    }
}
return res;
```
如[1760. 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/description/)

### 3.6 求最大差值利润值
暴力循环i从0开始j从i+1开始遍历整个数组往往会超时。所以一次遍历使用一个min变量记录遍历过程中出现的最小值，再用另一个变量记录最大的差值即可。
```cpp
int minPrice = INT_MAX, maxProfit = 0;
for (int &price : prices)
{
    minPrice = min(minPrice, price);
    maxProfit = max(maxProfit, price - minPrice);
}
```

### 3.7 数学方法
- **等差数列求同构子字符串**  
一个字符串所有字符相同则为同构子字符串，且子字符串是字符串中的一个连续序列。双层for循环会超时，用等差数列求和方法一次遍历即可。  
如果题目提到答案可能很大，要注意数据范围问题，使用```long long```定义数据。prev为第一个子字符，遍历时cnt计算每个字符出现次数，如果碰到不相同的字符则用等差数列求和加到res中并重置prev和cnt：
```cpp
int countHomogenous(string s) {
    long long res = 0;
    long long mod = 1e9+7;
    int cnt = 0;
    char prev = s[0];
    for (auto &c:s)
    {
        if (c == prev)
        {
            cnt ++;
        }
        else
        {
            res += (long long)(cnt+1)*cnt / 2;
            prev = c;
            cnt = 1;
        }
    }
    res += (long long)(cnt+1)*cnt / 2;
    return res % mod;
}
```

### 3.8 排序做法
如[2037. 使每位学生都有座位的最少移动次数](https://leetcode.cn/problems/minimum-number-of-moves-to-seat-everyone/description/)  
可以先对数组进行**排序**后再遍历，找对位差  

如[268. 丢失的数字](https://leetcode.cn/problems/missing-number/)  
与上题类似，同样可以先排序在找未连续出现的差值  
数组**find**查找：```find(nums.begin(), nums.end(), i) != nums.end()```

### 3.9 排序+双指针
如[349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)  
可以给两个数组排序之后，利用两个指针分别指向两个排序后数组的头，依次遍历，遇到相同元素就放入答案中，并使两个指针同时++；否则就令指向较小元素的指针++
```cpp
while (i < nums1.size() && j < nums2.size())
{
    if (nums1[i] == nums2[j])
    {
        if (!res.size() || res.back() != nums1[i]) // 通过最后一位元素与新的num不相等来去重，前提是数组里得有元素
        {
            res.emplace_back(nums1[i]);
        }
        i ++;
        j ++;
    }
    else if (nums1[i] > nums2[j])
    {
        j ++;
    }
    else
    {
        i ++;
    }
}
```

### 3.10 滑动窗口
如[1658. 将 x 减到 0 的最小操作数](https://leetcode.cn/problems/minimum-operations-to-reduce-x-to-zero/description/)  
移除的前缀和加上后缀和的和恰好等于x；可以使用left表示前缀的边界，如果left=-1则代表空前缀，如果right=n则代表空后缀。当left右移时，为了控制和为x，right也应右移，类似于滑动窗口。  
初始设left=-1，right=0代表选择了空前缀，整个数组作为后缀，使用lsum和rsum记录前缀和后缀和；那么当lsum+rsum = x的时候即为一组答案，结果取min
```cpp
int right = 0;
int lsum = 0, rsum = sum;
int ans = n + 1;

for (int left = -1; left < n; left ++)
{
    if (left != -1)
    {
        lsum += nums[left];
    }

    while (right < n && (lsum + rsum > x))
    {
        rsum -= nums[right++];
    }

    if (lsum + rsum == x)
    {
        ans = min(ans, n - right + left + 1);
    }
}
```

### 3.11 单调栈
如[496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/description/)  
求下一个更大元素这种题型时可以使用单调栈；用map来存储一个数组每个数对应的右边最大的那个数，用单调栈来存储最大的那个数。  
倒序遍历，当出现数字比栈中栈顶元素大时，就遍历栈弹出元素直到栈为空或者遍历到比当前数字大的元素；如果栈为空则对应哈希map中的值为-1否则就为栈顶元素。最后再遍历另一个数组，通过哈希map一次遍历就能得到所有更大元素。
```cpp
unordered_map<int, int> hash;
stack<int> stk;
for (int i = nums2.size() - 1; i >= 0; i --)
{
    while (!stk.empty() && nums2[i] > stk.top())
    {
        stk.pop();
    }
    hash[nums2[i]] = stk.empty() ? -1 : stk.top();
    stk.push(nums2[i]);
}
```


---
## 4. 数学
### 4.1 进制转换
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

### 4.2 贪心
我们可以用max(a,b,c)来求出排序后的c，然后用abc的和减去最大值求出排序后的a+b来模拟排序，排序后再判断不同情况  
先排序，排序后分为两种情况：
1. a+b <= c，此时可以拿c去跟a或b中每一个石子两两配对，答案为a+b
2. a+b > c，此时先拿c去跟a或b中最大的匹配，当c为0时，a和b再开始两两配对。所以答案为c+(a+b-c)/2  
如[1753. 移除石子的最大得分](https://leetcode.cn/problems/maximum-score-from-removing-stones/description/) 

### 4.3 找规律
- **放置盒子**  
如[1739. 放置盒子](https://leetcode.cn/problems/building-boxes/description/)  
摆放盒子，如果一个盒子上面有盒子，则这个盒子的四周应被完全环绕，所以此时靠墙是用盒最少的办法，求最后摆放在地上的所有盒子数量。
当我们要放置n个盒子时，我们要完整的放满i-1层，剩余的盒子再用第i层的接触地面的j个盒子来填充。此时的最小和n应该满足：(1) + (1+2) + (1+2+3) + ... + (1+2+...+i-1) 最后加上由j个盒子所带来的收益：j*(j+1)/2；上述哪个式子利用求和公式得为：2\*1/2 + 3\*2/2 + ... + i*(i-1)/2  
cur，i，j都初始化为1    
在代码中我们使用cur去维护第i层最多可以增加多少个放置的盒子，如果当前n>cur，就拿总数量n减去cur，然后递增i到下一层，否则这个i就是我们要找的那一层，更新i后更新cur=cur+i。  
在填满i-1层后我们去找j，维护变量cur为在第i层中再添加一个接触地面的盒子时会增加多少个可以放置的盒子，如果上面那一步剩下的n大于cur，则将n减去cur再递增j和cur都加1，否则j就是我们要找的接触地面的数量。  
最终我们完整的放满了前i-1层，并且在第i层放了j个接触地面的盒子，因此最终接触地面盒子的答案为：1+2+...+(i-1)+j = i*(i-1)/2 + j
```cpp
int minimumBoxes(int n) {
    int cur = 1, i = 1, j = 1;
    while (n > cur)
    {
        n -= cur;
        i ++;
        cur += i;
    }
    cur = 1;
    while (n > cur)
    {
        n -= cur;
        j ++;
        cur ++;
    }
    return (i - 1)*i/2 + j;
}
```

### 4.4 模拟
- **取各位数字之和为奇数或偶数**  
如[2180. 统计各位数字之和为偶数的整数个数](https://leetcode.cn/problems/count-integers-with-even-digit-sum/)  
对一个整数取余10即为它个位上的数字，再对它除以10下次再取余则能得到它十位上的数字
```cpp
while (x)
{
    sum += x % 10;
    x /= 10;
}
```

### 4.5 逆置
- **统计原数字与其逆置后数字相加和**
如[1814. 统计一个数组中好对子的数目](https://leetcode.cn/problems/count-nice-pairs-in-an-array/description/)  
通过给出的公式我们可以将其转换变为f(i) = f(j)的形式，f(i)代表数组i位置元素与其逆置后元素的和
```cpp
int countNicePairs(vector<int>& nums) {
    const int MOD = 1e9 + 7;
    unordered_map<int, int> hash; // 存取元素与其逆置和的数量
    long long res = 0; // 注意数据类型
    for (auto &num : nums)
    {
        int temp = num, j = 0;
        while (temp > 0) // 逆置元素方法
        {
            j = j*10 + temp % 10;
            temp = temp/10;
        }
        res += hash[num - j]; 
        hash[num - j] ++; // 存取为下个元素做比对
    }

    return res % MOD;
}
```

### 4.6 最大公约数
- **求最大公因子**  
我们需要**枚举前缀串**来判断是否相加后的前缀串与字符串相同，那么如何获得前缀串呢？
```cpp
bool check(string x, string str)
{
    int len = str.size() / x.size();
    string res = "";
    for (int i = 0; i < len; i ++)
    {
        res += x; // 得到的前缀串累加和
    }
    return res == str;
}

string gcdOfStrings(string str1, string str2) {
    int n1 = str1.size(), n2 = str2.size();
    for (int i = min(n1, n2); i > 0; i --) // 此处可以用C++自带_gcd()函数优化
    {
        if (n1 % i == 0 && n2 % i == 0)
        {
            string x = str1.substr(0, i); // 得到的前缀串
            if (check(x, str1) && check(x, str2))
                return x;
        }
    }
    return "";
}
```
如[1071. 字符串的最大公因子](https://leetcode.cn/problems/greatest-common-divisor-of-strings/description/)


---
## 5. 位运算
### 5.1 模拟全字母
由于字符集仅有26个，我们可以使用一个长度为26的二进制数字来表示字符集合，该二进制数字使用32位带符号整型变量（int类型）即可。出现第几个字符就将其左移对应位数，再进行或运算。最后判断是否为$2^{26}-1$（即所有0-25位上的数字都为1，其余为0）返回true。
```cpp
state |= 1 << (c - 'a');
```
如[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)

### 5.2 计数 
当求一个数组里的元素只出现一次或两次，求只出现一次的数字的时候，我们可以使用异或运算。因为两个相同的元素异或的值为0。
```cpp
int res = 0;
for (int &num : nums) res ^= num;
return res;
```
如[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/description/)

### 5.3 查找三个数组中出现的共同值
如[2032. 至少在两个数组中出现的值](https://leetcode.cn/problems/two-out-of-three/description/)  
因为该题一共只有三个数组，可以用二进制位运算代表三位，如果只出现一次的话那么他只存在于第一位中，此时他的二进制最低位的值-1为0，所以做与运算即可
```cpp
unordered_map<int, int> mp; // 用哈希表存二进制三位数出现次数
for (auto &n : nums1)
{
    mp[n] = 1; // 首数组，初始化所有值的二进制末位为1，001
}
for (auto &n : nums2)
{
    mp[n] |= 2; // 二进制第二位，如果出现与首数组相同的数据则为011
}
for (auto &n : nums3)
{
    mp[n] |= 4; // 二进制第三位，如果出现与首数组相同的数据则为111
}
vector<int> res;
for (auto &[k, v] : mp)
{
    if (v & (v-1)) // 当且仅当为001的时候，它只出现一次，1-1=0
    {
        res.emplace_back(k);
    }
}
return res;
```

### 5.4 率先出现两次字符
如[2351. 第一个出现两次的字母](https://leetcode.cn/problems/first-letter-to-appear-twice/)  
找一个字符串中最先出现两次的字母，可以用哈希表或者位运算  
<<是位运算符号，代表把1的二进制表示左移30位，左移一位（即在原来的数后面加一个0）相当于乘以2，左移30位应该是相当于乘以2的30次方
```cpp
if (mask & (1 << (c - 'a'))) // 当再次出现时与为1
    return c;
mask |= 1 << (c - 'a'); // 第一次出现时元素位置二进制位变为1
```


---
## 6. 图
### 6.1 广度优先搜索 
思路：  
若要判断从顶点source到顶点destination的连通性，需要我们从起始顶点source开始按层依次遍历每一层的顶点。遍历过程中我们使用队列存储最近访问过的顶点，同时记录每个顶点的访问状态。每次从队列中取出顶点vertex时，将其未访问过的邻接顶点入队列。  
算法步骤：  
初始时将顶点source设为已访问并将其入队列。每次将队列中的节点vertex出队列，并将与vertex相邻且未访问的顶点next入队列，并将next状态设为已访问。当队列为空或访问到顶点destination时遍历结束，返回顶点destination的访问状态即可。
```cpp
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<vector<int>> adj(n); //对于vector构建出来的二维数组没有进行空间的申请，比如有些返回类型为vector<vector<>>类型的函数，对于这个返回值vector表示的二维数组要先申请大小，否则使用下标访问就会报这类错误。
        for (int i = 0; i < edges.size(); i ++)
        {
            int x = edges[i][0], y = edges[i][1];
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }

        vector<bool> visted(n, false);
        visted[source] = true;
        queue<int> qu;
        qu.emplace(source);
        while (!qu.empty())
        {
            int next = qu.front();
            qu.pop();
            if (next == destination)
            {
                break;
            }
            for (auto n : adj[next])
            {
                if (!visted[n])
                {
                    visted[n] = true;
                    qu.emplace(n);
                }
            }
        }
        return visted[destination];
    }
};
```
时间复杂度：O(n+m)，n：图中顶点数目，m：图中边的数目，每个定点或者每条边只需访问一次  
空间复杂度：O(n+m)，主要取决于邻接点列表、记录访问状态的数组和**队列**

### 6.2深度优先搜索
思路：  
从顶点source开始依次遍历每一条可能的路径，判断可以到达顶点destination，同时还需要记录每个顶点的访问状态防止重复访问。  
算法步骤：  
首先从顶点source开始遍历并进行递归搜索。搜索时每次访问一个顶点vertex时，如果vertex等于destination直接返回，否则将该顶点设为已放问，并递归访问与vertex相邻且未访问的顶点next。如果通过next的路径可以访问到destination，此时直接返回true。当访问完所有的邻接节点仍然没有访问到destination时，返回false。
```cpp
class Solution {
public:
    bool dfs(int source, int destination, vector<vector<int>> &adj, vector<bool> &visted)
    {
        if (source == destination)
        {
            return true;
        }

        visted[source] = true; // 别忘了初始化！
        for (auto &next : adj[source])
        {
            if (!visted[next] && dfs(next, destination, adj, visted)) // 递归=dfs判断
            {
                return true;
            }
        }
        return false;
    }

    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<vector<int>> adj(n);
        for (auto &&edge : edges)
        {
            int x = edge[0], y = edge[1];
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }
        vector<bool> visted(n, false);
        return dfs(source, destination, adj, visted);
    }
};
```
时间复杂度：O(n+m)，n：图中顶点数目，m：图中边的数目，每个定点或者每条边只需访问一次  
空间复杂度：O(n+m)，主要取决于邻接点列表、记录访问状态的数组和**递归调用栈**

### 6.3 并查集
思路：  
我们将图中的每个强连通分量视为一个集合，强连通分量中任意两点均可达。如果两个点source和destination处在同一个强连通分量中，则两点一定可连通，因此连通性问题可以使用并查集解决。  
算法步骤：  
并查集初始化时，n个顶点分别属于n个不同的集合，每个集合只包含一个顶点。初始化之后遍历每条边，由于图中的每条边均为双向边，因此将同一条边连接的两个顶点所在的集合做合并。在遍历完所有的边之后，判断顶点source和顶点destination是否在同一个集合中，如果两个顶点在同一集合则两个顶点连通。
```cpp
class Union {
public:
    Union(int n)
    {
        parent.resize(n); // 初始化设置容器大小，还可以使用parent = vecotr<int>(n);
        rank.resize(n);
        for (int i = 0; i < n; i ++)
        {
            parent[i] = i; // 初始化每个顶点的parent为自己
        }
    }

    void v(int a, int b)
    {
        int roota = find(a);
        int rootb = find(b);
        if (roota != rootb)
        {
            if (rank[roota] > rank[rootb]) // 判断哪个root在更前面即等级更大；这里的rank判断可加可不加（只是让parent节点更严谨），不影响结果
            {
                parent[rootb] = roota;
            }
            else if (rank[roota] < rank[rootb])
            {
                parent[roota] = rootb;
            }
            else
            {
                parent[rootb] = roota; // 设置最开始的root
                rank[roota] ++; // root级别+1
            }
        }
    }

    int find(int x)
    {
        if (parent[x] != x)
        {
            parent[x] = find(parent[x]); // 递归找root
        }
        return parent[x];
    }

    bool connect(int a, int b)
    {
        return (find(a) == find(b));
    }

private:
    vector<int> parent;
    vector<int> rank; // 可加可不加
};

class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        // 此处就不需要定义邻边容器了
        Union uni(n); // 初始化
        for (auto &edge : edges)
        {
            uni.v(edge[0], edge[1]); // 相邻两边做合并
        }
        
        return uni.connect(source, destination);
    }
};
```