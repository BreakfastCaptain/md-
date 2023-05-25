# C++ TOP101

## 链表

### 反转链表

**1 直接构造**

`nullptr`是C++11引入的空指针常量。它用于表示空指针，即指向没有有效对象或函数的指针。

在过去，C++中常用的空指针表示方式是使用字面量 `NULL` 或者预处理器宏 `0` 来表示空指针。然而，这种表示方式存在一些问题，容易导致类型不匹配或者产生二义性。

为了解决这些问题，C++11引入了 `nullptr` 关键字，它是一个独立的类型，可以隐式地转换为任意指针类型，而不会引发类型不匹配的问题。使用 `nullptr` 可以提高代码的可读性和安全性。

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        if (!pHead) return nullptr;
        vector<ListNode*> v;
        while (pHead) {
            v.push_back(pHead);
            pHead = pHead->next;
        }
        reverse(v.begin(), v.end()); // 反转vector，也可以逆向遍历
        ListNode *head = v[0];
        ListNode *cur = head;
        for (int i=1; i<v.size(); ++i) { // 构造链表
            cur->next = v[i]; // 当前节点的下一个指针指向下一个节点
            cur = cur->next; // 当前节点后移
        }
        cur->next = nullptr; // 切记最后一个节点的下一个指针指向nullptr
        return head;
    }
};
```

**2 迭代**

`ListNode *pre = nullptr` 和 `ListNode* pre = nullptr` 是等价的，它们都将指针 `pre` 初始化为空指针。

在C++中，声明指针变量时，可以将星号 `*` 放置在类型名之后，也可以放置在变量名之前，两种写法是等效的。

所以，`ListNode *pre = nullptr` 和 `ListNode* pre = nullptr` 声明了一个名为 `pre` 的指针变量，类型为 `ListNode*`，并将其初始化为 `nullptr`，即空指针。

```cpp
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        ListNode *pre = nullptr;
        ListNode *cur = pHead;
        ListNode *nex = nullptr; // 这里可以指向nullptr，循环里面要重新指向
        while (cur) {
            // 中介nex，进行反向操作
            nex = cur->next;
            cur->next = pre;
            // 进行下一组操作
            pre = cur;
            cur = nex;
        }
        return pre;
    }
};
```

### 链表制定区间反转

输入：

```
{1,2,3,4,5},2,4
```

返回值：

```
{1,4,3,2,5}
```

**首先还是制造一个哨兵伪节点来避免头节点的分类讨论**

然后找到反转节点前一个位置的节点用pre指过去

再者，就是运用头插法来反转链表了，pre后面是cur，这里需要不断地把cur后面的节点移动到，pre后面来，这个时候需要一个临时指针来操作后面这个节点temp，这个操作需要进行n-m+1次

每次操作后都是一条完整的链表，进行上述操作完成后就实现了m-n区间的链表反转了

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        // write code here
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* pre = dummy, * cur = head;
        for (int i = 1; i < m; i++) {
            pre = pre->next;
        }
        cur = pre->next;
        for (int i = 0; i < n - m; i++) {
            ListNode* temp = cur->next;
            cur->next = cur->next->next;
            temp->next = pre->next;
            pre->next = temp;
        }
        return dummy->next;
    }
};
```



## 二分查找/排序



## 二叉树



## 堆/栈/队列



## 哈希



## 递归/回溯



## 动态规划



## 字符串

### 字符串变形

请返回变形后的字符串。题目保证给定的字符串均由大小写字母和空格构成。

输入：

```
"This is a sample",16
```

返回值：

```
"SAMPLE A IS tHIS"
```

```cpp
class Solution {
public:
    string trans(string s, int n) {
        if(n==0) 
            return s;
        string res;		// 需要声明string类型，直接建立一个新的而不是在原来的上面变更
        for(int i = 0; i < n; i++){
            //大小写转换
            if (s[i] <= 'Z' && s[i] >= 'A')   
                res += s[i] - 'A' + 'a';
            else if(s[i] >= 'a' && s[i] <= 'z') 
                res += s[i] - 'a' + 'A';
            else 
                //空格直接复制
                res+=s[i];  
        } 
        //翻转整个字符串
        reverse(res.begin(), res.end());  
        for (int i = 0; i < n; i++){
            int j = i;
            //以空格为界，二次翻转
            while(j < n && res[j] != ' ')  
                j++;
            reverse(res.begin() + i, res.begin() + j); 	// j此时处于空格的位置，反转时需要带上，因为前面全部反转把空格也反转了
            i = j;
        }
        return res;
    }
};
```

### 最长公共前缀

整体思路是：不用一次判断一个子字符串，一次判断一个字符是否和其他的符合即可

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int n = strs.size();
        //空字符串数组
        if(n == 0) 
            return "";
        //遍历第一个字符串的长度
        for(int i = 0; i < strs[0].length(); i++){ 
            char temp = strs[0][i]; 
            //遍历后续的字符串
            for(int j = 1; j < n; j++) 
                //比较每个字符串该位置是否和第一个相同
                if(i == strs[j].length() || strs[j][i] != temp) 
                    //不相同则结束
                    return strs[0].substr(0, i); 
        }
        //后续字符串有整个字一个字符串的前缀
        return strs[0]; 
    }
};
```

### 大数加法

C++没有很长的数字处理的方法

```cpp
class Solution {
public:
    string solve(string s, string t) {
        // write code here
        int len1=s.size();
        int len2=t.size();
        // 首先要保证两个数的位数要保持一致
        while(len1<len2){
            s="0"+s;
            len1++;
        }
        while(len1>len2){
            t="0"+t;
            len2++;
        }
        
        string ans;
        int carry=0;  // 定义一个变量作为进位位
        for(int i=len1-1;i>=0;i--){
            int tmp=(s[i]-'0'+t[i]-'0'+carry);
            ans+=char(tmp%10+'0');		// 注意这里易错，是把字符对应的编码给char转换而不是数字直接可以转换
            carry=tmp/10;  // 不断更新进位位
        }
        // 最后还需要特判进位位
        if(carry){
            ans+=(carry+'0');
        }
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```



## 双指针

### 合并区间

输入：

```
[[10,30],[20,60],[80,100],[150,180]]
```

返回值：

```
[[10,60],[80,100],[150,180]]
```

组数n，值大小val

要求：空间复杂度O*(*n*)，时间复杂度 O*(*n**l**o**g**n*)

```cpp
class Solution {
public:
    //重载比较，需要识记
    static bool cmp(Interval &a, Interval &b) { 
        return a.start < b.start;
    }
    
    vector<Interval> merge(vector<Interval> &intervals) {
        vector<Interval> res;
        //去除特殊情况，注意必须去除，不然会报错
        if(intervals.size() == 0) 
            return res;
        //按照区间首排序
        sort(intervals.begin(), intervals.end(), cmp); 
        //放入第一个区间
        res.push_back(intervals[0]); 
        //遍历后续区间，查看是否与末尾有重叠
        for(int i = 1; i < intervals.size(); i++){ 
            //区间有重叠，更新结尾
            if(intervals[i].start <= res.back().end) 		// 注意end
                // res.back() 可以获取到容器中最后一个 Interval 对象的引用
                res.back().end = max(res.back().end, intervals[i].end);
            //区间没有重叠，直接加入
            else 
                res.push_back(intervals[i]);
        }
        return res;
    }
};
```

`res.back().end` 不需要加括号是因为 `end` 是一个成员变量（或称为数据成员），而不是一个函数或方法。在 C++ 中，成员变量可以通过成员访问操作符 `.` 直接访问，不需要使用括号。

当调用 `res.back()` 时，会返回 `res` 中最后一个元素的引用，然后通过 `.end` 访问该元素的 `end` 成员变量，获取到区间的结束位置。这样就可以进行比较和更新操作，根据需要对区间进行合并。

### **最长无重复子数组************

给定一个长度为n的数组arr，返回arr的最长无重复元素子数组的长度，无重复指的是所有数字都不相同。

子数组是连续的，比如[1,3,5,7,9]的子数组有[1,3]，[3,5,7]等等，但是[1,3,7]不是子数组

**疑惑的点**

C++怎么使用set，如果不使用set的话递增过程中判断没有重复元素怎么判断

```cpp
class Solution {
public:
    int maxLength(vector<int>& arr) {
        map<int,int> last;
        int ans = 0, now = 0;
        for (int i = 0; i < (int)arr.size(); ++i) last[arr[i]] = -1;	// 所有的值赋值-1的映射，last里面没有重复的值
        for (int i = 0; i < (int)arr.size(); ++i) {
            if (last[arr[i]] != -1) now = max(now, last[arr[i]] + 1);	// 不为-1说明已经有了
            ans = max(ans, i - now + 1);
            last[arr[i]] = i;
        }
        return ans;
    }
};
```

### 盛水最多的容器

给定一个数组height，长度为n，每个数代表坐标轴中的一个点的高度，height[i]是在第i点的高度，请问，从中选2个高度与x轴组成的容器最多能容纳多少水；

两边可以不相等，遵循短板

输入：

```
[1,7,3,2,4,5,8,2,7]
```

返回值：

```
49
```

直接遍历13/15：时间复杂度太大

**双指针：**

我们发现我们直接朴素的暴力两重循环会超时，那我们想一下是不是有什么优化可以做一下，我们发现我们可以是不是得到这么一个公式

> 总存水量 = 两个板子之间的最小值 * 两个板子之间的距离

那么我们是不是可以，用两个指针，一个指向开头，一个指向结尾，然后我们每次移动一个指针，这时候我们要考虑一个问题，我们要移动哪一个指针，我们看到这个公式，我们可以思考一下，我们每次移动指针，两个板子之间的距离都是会变小，那么我们要是尽可能的想让我们总存水量变大，那么我们**就要让我们两个板子之间的最小值变大**，我们移动大的那个指针只会让我们的答案不变或者变小，所以我们只有移动小的那个指针才可以达到变化的一个作用

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int len = height.size(), l = 0, r = len - 1, maxx = 0;
//         获取数组长度，左右指针分别指向开头和结尾，把最大值初始为0
        while(l < r) {
//             执行循环
            maxx = max(maxx, min(height[l], height[r]) * (r - l));
//             每次对最大值进行一个更新
            height[l] < height[r] ? l += 1 : r -= 1;
//             每次我们把数组值小的那边的指针移动
        }
        return maxx;
    }
};
```



## 贪心算法



## 模拟

# 函数

## reverse()

修改了原始容器中的元素顺序，直接修改了元素里面的顺序。

就是直接对原始容器进行操作改变的**，没有返回值。**

**可以理解为左右都闭**，**这个end位置的可以取到**

```cpp
reverse(res.begin(), res.end());  
```

## substr()

`substr()` 是一个字符串的成员函数，用于提取字符串的子串。它接受两个参数：起始位置和子串长度（可选）。**返回值是一个新的字符串，表示从原始字符串中提取的子串。**

下面是 `substr()` 的用法示例：

```cpp
#include <iostream>
#include <string>

int main() {
    std::string str = "Hello, World!";

    // 提取从索引位置 7 开始的子串
    std::string sub1 = str.substr(7);
    std::cout << "Substring 1: " << sub1 << std::endl;

    // 提取从索引位置 0 开始，长度为 5 的子串
    std::string sub2 = str.substr(0, 5);
    std::cout << "Substring 2: " << sub2 << std::endl;

    return 0;
}
```

输出结果为：

```
Substring 1: World!
Substring 2: Hello
```

需要注意的是，如果只提供了起始位置参数而没有提供长度参数，`substr()` 将提取从起始位置到字符串的末尾的所有字符。如果提供了长度参数，则提取指定长度的子串。

## 字符串和数字转换

```cpp
#include <string>
// 转换为字符串
    int num = 12345;
    std::string str = std::to_string(num);
// 转换为整数
std::string str = "12345";
    int num = std::stoi(str);
```

## 字符和数字转换

1. 字符转换为编码：可以使用类型转换或整数运算将字符转换为其对应的编码值。

```cpp
char c = 'A';
int code = static_cast<int>(c);
```

2. 编码转换为字符：可以使用类型转换或整数运算将编码值转换为对应的字符。

```cpp
int code = 65;
char c = static_cast<char>(code);
```

## sort()

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    int A[] = {5, 2, 9, 1, 7, 3};
    int size = sizeof(A) / sizeof(A[0]);

    // 对数组进行排序
    sort(A, A + size);

    // 输出排序后的数组
    for (int i = 0; i < size; i++) {
        cout << A[i] << " ";
    }
    cout << endl;

    return 0;
}
```

**二维数组排序**

```cpp
 //重载比较
    static bool cmp(Interval &a, Interval &b) { 
        return a.start < b.start;
    }
    
    vector<Interval> merge(vector<Interval> &intervals) {
        sort(intervals.begin(), intervals.end(), cmp); // 起始+排序规则
```

# STL

