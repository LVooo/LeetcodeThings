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

- **模拟**  
用字符串模拟数字串。  
int类型到字符串的转换：```to_string()```  
字符串类型转int：```stoi()```  
字符或字符串类型到int类型的转换：```(c - 'a') (c - '0')```or通过ascii码转换
```cpp
char a = '0';
int ia = (int) a; // 输出48，因为ascii码的数字0从48开始
int x = ia - 48; // 输出0
```
如[1945. 字符串转化后的各位数字之和](https://leetcode.cn/problems/sum-of-digits-of-string-after-convert/description/) 


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
### 3.1 模拟全字母
用vector数组来表示26位字母位数，每出现一次在对应位置为true（或计数+1）
```cpp
vector<int> exist(26);
for (auto c : sentence) {
    exist[c - 'a'] = true;
}
```
如[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)

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

---
## 4. 数学
4.1 **进制转换**<br>
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

- **贪心**  
我们可以用max(a,b,c)来求出排序后的c，然后用abc的和减去最大值求出排序后的a+b来模拟排序，排序后再判断不同情况  
先排序，排序后分为两种情况：
1. a+b <= c，此时可以拿c去跟a或b中每一个石子两两配对，答案为a+b
2. a+b > c，此时先拿c去跟a或b中最大的匹配，当c为0时，a和b再开始两两配对。所以答案为c+(a+b-c)/2  
如[1753. 移除石子的最大得分](https://leetcode.cn/problems/maximum-score-from-removing-stones/description/)  


---
## 5. 位运算
- **模拟全字母**  
由于字符集仅有26个，我们可以使用一个长度为26的二进制数字来表示字符集合，该二进制数字使用32位带符号整型变量（int类型）即可。出现第几个字符就将其左移对应位数，再进行或运算。最后判断是否为$2^{26}-1$（即所有0-25位上的数字都为1，其余为0）返回true。
```cpp
state |= 1 << (c - 'a');
```
如[1832. 判断句子是否为全字母句](https://leetcode.cn/problems/check-if-the-sentence-is-pangram/description/)

- **计数**  
当求一个数组里的元素只出现一次或两次，求只出现一次的数字的时候，我们可以使用异或运算。因为两个相同的元素异或的值为0。
```cpp
int res = 0;
for (int &num : nums) res ^= num;
return res;
```
如[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/description/)
- 


---
## 6. 图
- **广度优先搜索**  
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

- **深度优先搜索**  
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

- **并查集**  
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