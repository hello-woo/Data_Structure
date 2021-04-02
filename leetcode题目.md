# leetcode题目

## 解题细节小知识点

```cpp
C\C++ 浮点数向下\向上取整函数 floor() ceil() -----头文件#include<math.h>
将字符串转换为数字：atoi(str.c_str())
将字符串转换为数字:stoi（str)----//在INT整型范围之类；
```

#### 各种数据结构的定义

```cpp
//单链表(single-linked list)
struct listnode{
    int val;
    listnode* next;
    listnode(int x):val(x),next(nullptr){}
};
// 二叉树节点定义，Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
/**二叉树节点定义
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
```

#### 判断是否是回文串

```cpp
//朴素的判断;
bool isPalindrome(string s){
    int left=0,right=s.size()-1;
    while(left<right){
        if(s[left++]!=s[right--]){
            return false;
        }
    }
    return true;
}
//动态规划预处理;dp[i][j]=dp[i+1][j-1]^s[i]==s[j];
//dp[i][j]为字符串从i到j是否是回文串;
bool isPalindrome(string s){
    int n=s.size();
    vector<vector<bool>>dp(n,vector<int>(n,true));
    for(int i=n-1;i>=0;i--){
        for(int j=i+1;j<n;j++){
            dp[i][j]=(s[i]==s[j])&&dp[i+1][j-1];
        }
    }
}

```

#### sort排序的使用

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

int a[10]={5,1,8,4,2,5,7,4,9,10};

int main(){
    int i;
    sort(a,a+10);
    for(int i=0;i<10;i++){
        cout<<a[i]<<" ";
    }
    return 0;
}
//输出：1 2 4 4 5 5 7 8 9 10
```

自定义使用cmp

```cpp
#include<iostream>
#include<algorithm>

using namespace std;

int a[10]={5,1,8,4,2,5,7,4,9,10};

bool cmp(const int &a,const int&b){
    return a>b;
}

int main(){
    int i;
    sort(a,a+10,cmp);
    for(int i=0;i<10;i++){
        cout<<a[i]<<" ";
    }
    return 0;
}
```

仿函数：

```
#include<iostream>
#include<algorithm>

using namespace std;

int a[10]={5,1,8,4,2,5,7,4,9,10};
//
struct Big{
    bool operator()(int a,int b){
        return a>b;
    }  
};

int main(){
    int i;
    sort(a,a+10,Big());//Big()匿名对象,
    for(int i=0;i<10;i++){
        cout<<a[i]<<" ";
    }
    return 0;
}

```

less和greater

```
#include<iostream>
#include<algorithm>

using namespace std;

int a[10]={5,1,8,4,2,5,7,4,9,10};

int main(){
    int i;
    sort(a,a+10,greater<int>());//从大到小排序
    for(int i=0;i<10;i++){
        cout<<a[i]<<" ";
    }
    return 0;
}

```

对结构体排序

- 大小比较函数
- 重载小于号
- 仿函数

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

struct node{
    int a,b;
    bool operator<(node B){
        if(a!=B.a) return a<B.a;
        return b<B.b;
    }
};
node a[10]={ {2,3},{10,10},{5,5},{4,4},{10,4},{7,1},{10,10},{4,6},{10,7},{2,8}};

int main(){
    sort(a,a+10);
    for(int i=0;i<10;i++){
        cout<<a[i].a<<" "<<a[i].b<<endl;
    }
    return 0;
}

```

#### pair的使用

`pair<int,int>`和`make_pair(a,b)`,利用vector排序得到的是第一个数从小到大排序，如果第一个相等，第二个从小到大排序。

```cpp
#include<iostream>
#include<bits/stdc++.h>

using namespace std;

pair<int ,int >p;
vector<pair<int,int>>v;
int main(){
    v.push_back(make_pair(1,1));
    v.push_back(make_pair(1,2));
    v.push_back(make_pair(2,1));
    v.push_back(make_pair(2,0));
    sort(v.begin(),v.end());
    for(int i=0;i<v.size();i++){
        cout<<v[i].first<<" "<<v[i].second<<endl;
    }
    return 0;
}

```

重载cmp

```cpp
#include<iostream>
#include<algorithm>
using namespace std;

pair<int,int>pa[100];


//自定义排序：第一个数按照从大到小排序，如果第一个数相等，第二个数按照从小到大排序；
bool cmp(pair<int,int>a,pair<int,int>b){
    if(a.first!=b.first) return a.first>b.first;
    return a.second<b.second;
}

int main(){
    int a,b;
    for(int i=0;i<5;i++){
        cin>>a>>b;
        pa[i]=make_pair(a,b);
    }
    sort(pa,pa+5,cmp);
    cout<<"sorted:"<<endl;
    for(int i=0;i<5;i++){
        cout<<pa[i].first<<" "<<pa[i].second<<endl;
    }
    return 0;
}
//输出结果为：
1 2
4 2
3 3
2 1
1 3
sorted:
4 2
3 3
2 1
1 2
1 3
```



## 每日一题打卡

#### 03.14 设计哈希映射(706)

```cpp
class MyHashMap {
public:
    /** Initialize your data structure here. */
    vector<list<pair<int,int>>>data;
    const static int base=769;
    static int hash(int key){
        return key%base;
    }
    MyHashMap():data(base) {

    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int h=hash(key);
        for(auto it=data[h].begin();it!=data[h].end();it++){
            if((*it).first==key){
                (*it).second=value;
                return ;
            }
        }
        //pair<int,int>;make_pair<key,value>
        data[h].push_back(make_pair(key,value));
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int h=hash(key);
        for(auto iter=data[h].begin();iter!=data[h].end();iter++){
            if((*iter).first==key){
                return (*iter).second;
            }
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int h=hash(key);
        for(auto it=data[h].begin();it!=data[h].end();it++){
            if((*it).first==key){
                data[h].erase(it);
                return ;
            }
        }
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```

> 总结：利用除留取余法构造哈希函数，利用拉链法解决冲突问题；
>
> `pair<int,int>`构造数对，

#### 03.15 螺旋矩阵（54）

> 总结：
>
> 1、设置四个标志位top、bottom、left、right ，模拟旋转即可

#### 03.16螺旋矩阵2

> 和昨天类似，没什么，设置四个位模拟；

#### 03.17 不同的子序列

![image-20210317230417647](E:\Study_Notes\图片\0317)





#### 415 字符串相加（10进制或36位加法）-字节高频

给定两个字符串`string num1`和`string num2`,求两个的加法，其实即为大整数加法

```cpp
//10进制加法
 string addStrings(string num1, string num2) {
        int i = num1.size()-1;
        int j = num2.size()-1;
        int carry = 0;
        string res;
        while(i>=0||j>=0||carry){
            int x=(i>=0)?num1[i]-'0':0;
            int y=(j>=0)?num2[j]-'0':0;
            int temp=x+y+carry;
            carry = temp / 10;
            res+=temp%10+'0';
            i--;
            j--;
        }
        reverse(res.begin(),res.end());
        return res;
 }
//十进制中字符变为数字减‘0’，数字变为字符加'0';
//36进制加法 设置两个函数来实现数字和字符之间的转换
//给定0-9，a-z表示36为进制，求加法，不能转换为数字之后相加在转换为字符；
 int getInt(char ch){
        if('0'<=ch&&ch<='9') return ch-'0';
        else return ch-'a'+10;
    }
    char getChar(int i){
        if(i<=9) return i+'0';
        else return i-10+'a';
    }
    string addThirtySixStrings(string num1, string num2) {
        int i=num1.size()-1;
        int j=num2.size()-1;
        int carry=0;
        string res;
        while(i>=0||j>=0||carry){
            int x=(i>=0)?getInt(num1[i]):0;
            int y=(j>=0)?getInt(num2[j]):0;
            int temp=x+y+carry;
            carry=temp/36;
            res+=getChar(temp%36);
            i--;
            j--;
        }
        reverse(res.begin(),res.end());
        return res;
    }
```

#### 43 .字符串相乘

```cpp
class Solution {
public:
    //大数加法，从后向前开始加
    string addstring(string num1,string num2){
        int i=num1.size()-1;
        int j=num2.size()-1;
        int carry=0;
        string res="";
        while(i>=0||j>=0||carry){
            int x=(i>=0)?num1[i]-'0':0;
            int y=(j>=0)?num2[j]-'0':0;
            int d=x+y+carry;
            carry=d/10;
            res+=d%10+'0';
            i--;
            j--;
        }
        reverse(res.begin(),res.end());
        return res;
    }
    //123*456，可以等价为123*6+123*5*10+123*4*100; 
    string multiply(string num1, string num2) {
        if(num1=="0"||num2=="0") return "0";
        int len1=num1.size();
        int len2=num2.size();
        string ans="";
        for(int j=len2-1;j>=0;j--){
            string t="";
            int carry=0;
            for(int i=len1-1;i>=0;i--){
                int d=(num1[i]-'0')*(num2[j]-'0')+carry;
                carry=d/10;
                t+=d%10+'0';
            }
            if(carry) t+=carry+'0';
            //得到123*6的结果；
            reverse(t.begin(),t.end());
            int coffe=len2-j-1;
            //在后面添加对应进位上的‘0’;
            if(coffe>0) t+=string(coffe,'0');
            ans=addstring(ans,t);
        }
        return ans;
    }
};
```

#### 字符串相减

```c++
#include<iostream>
#include<bits/stdc++.h>

using namespace std;

string subStrings(string num1, string num2) {
    int i=num1.size()-1;
    int j=num2.size()-1;
    string res="";
    int carry=0;
    while(i>=0||j>=0||carry){
        int x=(i>=0)?num1[i]-'0':0;
        int y=(j>=0)?num2[j]-'0':0;
        int d=x-y-carry;
        carry=(d<0);
        res+=d+10*carry+'0';
        i--;
        j--;
    }
    for(int i=res.size()-1;i>=0;i--){
        if(res[i]=='0') res.pop_back();
        else break;
    }
    reverse(res.begin(),res.end());
    return res;
}

int main()  
{  
    int len1,len2;  
    string a,b;
    cin>>a>>b;
    string res;
    if(a.size()>b.size()){
        cout<<"结果为："<<subStrings(a,b)<<endl;
    }else if(a.size()<b.size()){
        cout<<"结果为：-"<<subStrings(b,a)<<endl;
    }else{
        for(int i=a.size()-1;i>=0;i--){
            if(a[i]==b[i]) continue;
            if(a[i]>b[i]) {
                cout<<"结果为："<<subStrings(a,b)<<endl;
                break;
            }
            if(a[i]<b[i]){
                cout<<"结果为:-"<<subStrings(b,a)<<endl;
                break;
            }
        }
    }
    return 0;   
}

```



#### 338.比特位计数

解题思路：

> 1. 奇数：二进制表示中，奇数一定比前面那个偶数多一个 1，因为多的就是最低位的 1。
> 2. 偶数：二进制表示中，偶数中 1 的个数一定和除以 2 之后的那个数一样多。因为最低位是 0，除以 2 就是右移一位，也就是把那个 0 抹掉而已，所以 1 的个数是不变的。
>

<img src="E:\Study_Notes\图片\leetcode\lc338" alt="image-20201116224547437" style="zoom:150%;" />

```c++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int>ans(num+1);
        ans[0]=0;
        for(int i=1;i<=num;i++){
            if(i%2){
                ans[i]=ans[i-1]+1;
            }else{
                ans[i]=ans[i/2];
            }
        }
        return ans;
    }
};
```

#### 14.最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 `""`。

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
       if(strs.size()==0){
       		return "";
       }
        string ans=strs[0];
        for(int i=1;i<strs.size();i++){
            string t=ans;
            for(int j=0;j<strs[i].size()&&j<t.size();j++){
                if(t[j]==strs[i][j]){
                    ans+=t[j];
                }else{
                    break;
                }
            }
            if(ans==""){
                break;
            }
        }
        return ans;
    }
};
```

#### oj477. 元音字母

给定一个只包含大写字母 ′′A′′−′′Z′′″A″−″Z″ 的字符串，找到相邻两个元音字母之间间隔最大的距离。

 注：元音字母为 AEIOUAEIOU。

```c
int find_maxstring(string s){
    int ans=0,last=-1;
    //s[i]表示读到字符串末尾
	for(int i=0;s[i];i++){
        if(s[i]=='A'||...){
            if(last==-1){
                last=i;
            }else{
            ans=max(ans,i-last);
            last=i;
        }
      }
    }
    return ans;
}
```



#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

![](E:\Study_Notes\图片\leetcode\lc26.png)

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()==0){
            return 0;
        }
        int p=1;
        for(int i=1;i<nums.size();i++){
            if(nums[i]!=nums[i-1]){
                nums[p++]=nums[i];
            }
        }
        return p;
    }
};
```

#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)



```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int start=0;
        int end=0;
        int len=0;
        int result=0;
        int n=s.size();
        while(end<n){
            char temp=s[end];
            for(int index=start;index<end;index++){
                if(temp==s[index]){
                    start=index+1;
                    len=end-start;
                }
            }
            end++;
            len++;
            result=max(result,len);
        }
        return result;

    }
};
```



## 链表

#### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

![image-20201124194542820](E:\Study_Notes\图片\leetcode\lc2)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head=new ListNode(-1);//存放结果的链表
       ListNode *h=head; //移动指针
       int sum=0;//每个位的加和结果
       bool carry=false;//进位标志
       while(l1!=NULL||l2!=NULL){
           sum=0;
           if(l1!=NULL){
               sum+=l1->val;
               l1=l1->next;
           }
           if(l2!=NULL){
               sum+=l2->val;
               l2=l2->next;
           }
           if(carry){
               sum+=1;
           }
           h->next=new ListNode(sum%10);
           h=h->next;
           carry=sum>=10?true:false;
        }
        if(carry){
            h->next=new ListNode(1);
        }
        return head->next;
    }
};
```

#### **160.两个链表的交点**

[leetcode160](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)
**解题思路：**

> 设 A 的长度为 a + c，B 的长度为 b + c，其中 c 为尾部公共部分长度，可知 a + c + b = b + c + a。同样的距离，同样的速度，则一定可以找到公共节点。
> 当访问 A 链表的指针访问到链表尾部时，令它从链表 B 的头部开始访问链表 B；同样地，当访问 B 链表的指针访问到链表尾部时，令它从链表
> A 的头部开始访问链表 A。这样就能控制访问 A 和 B 两个链表的指针能同时访问到交点。
>
> 如果不存在交点，那么 a + b = b + a，以下实现代码中 l1 和 l2 会同时为 null，从而退出循环。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *l1=headA,*l2=headB;
        while(l1!=l2){
            l1=(l1==NULL)?headB:l1->next;
            l2=(l2==NULL)?headA:l2->next;
        }
        return l1;
    }
};
```
#### **206反转链表**

[leetcode206](https://leetcode-cn.com/problems/reverse-linked-list/)
[动画演示](https://leetcode-cn.com/problems/reverse-linked-list/solution/dong-hua-yan-shi-206-fan-zhuan-lian-biao-by-user74/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        //迭代
        // ListNode *pre=NULL;
        // ListNode *cur=head;
        // while(cur!=NULL){
        //     ListNode *tmp=cur->next;
        //     cur->next=pre;
        //     pre=cur;
        //     cur=tmp;
        // }
        // return pre;
        //递归
        if(head==NULL||head->next==NULL) return head;
        ListNode *next=head->next;
        ListNode *newhead=reverseList(next);
        next->next=head;
        head->next=NULL;
        return newhead;

    }
};
```
#### 21.合并两个有序链表

[leetcode21](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        //递归问题
        // if(l1==NULL) return l2;
        // if(l2==NULL) return l1;
        // if(l1->val<l2->val){
        //     l1->next=mergeTwoLists(l1->next,l2);
        //     return l1;
        // }else{
        //     l2->next=mergeTwoLists(l1,l2->next);
        //     return l2;
        // }
        //暴力解法
        ListNode *head=new ListNode(-1);
        ListNode *h=head;
        while(l1!=NULL&&l2!=NULL){
            if(l2->val<l1->val){
                h->next=l2;
                l2=l2 ->next;
            }else{
                h->next=l1;
                l1=l1->next;
            }
            h=h->next;
        }
        h->next=(l1==NULL)?l2:l1;
        return head->next;
    }
};
```
## 二分查找：

#### 解题思路：

```c++
//常规:朴素的二分查找（在一维数组中查找一个数，有返回索引，无则返回-1）
int binary_search(int arr[],int n,int val){
	int head=0,tail=n-1;
	while(head<=tail){
		int mid=(head+tail)/2;
		if(arr[mid]==val){
			return mid;
		}else if(arr[mid]>val){
			tail=mid-1;
		}else{
			head=mid+1;
		}
	}
	return -1;
}
//变体1：可以总结为00001111问题，查找满足条件的第一个1
int binary_search(int n){
    while(head!=tail){
        int mid=(head+tail)/2;
        if(arr[mid]==0){
            head=mid+1;//mid指向的值必不为答案，搜索区间加一
        }else{
            tail=mid;//mid指向的值可能为答案
        }
    }
return head/tail
}
//变体2：可以总结为11110000问题，查找满足条件的最后一个1
int binary_search(int n){
    while(head!=tail){
        int mid=(head+tail+1)/2;//注意不同点，加1为了防止左指针死循环
        if(arr[mid]==0){
            tail=mid-1;//mid指向的值必不为答案，搜索区间减1
        }else{
            head=mid;//mid指向的值可能为答案
        }
    }
return head/tail
}
//变体3:二分答案，带入函数求答案与给定值比较
```



#### 35.搜索插入位置（000111)

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

```c++
//初级版
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int head=0,mid=0,tail=nums.size()-1;
        while(head<=tail){
            int mid=(head+tail)/2;
            if(nums[mid]<target){
                head=mid+1;
            }else if(nums[mid]==target){
                return mid;
            }else{
                tail=mid-1;
            }
        }
        return head;
    }
};
//进阶版
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
       int head =0,tail=nums.size()-1;
       while(head<=tail){
           int mid=(head+tail)/2;
           if(nums[mid]<target){
               head=mid+1;
           }else{
               tail=mid-1;
           }
       }
       return head;
    }
};
//
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0, right = n - 1, ans = n;
        while (left <= right) {
            int mid = ((right - left) >> 1) + left;
            if (target <= nums[mid]) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
};
//至尊版,利用上述模板
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        if(target>nums[nums.size()-1]){
            return nums.size();
        }
        int head=0,tail=nums.size()-1;
        while(head!=tail){
            int mid=(head+tail)/2;
            if(nums[mid]>=target){
                tail=mid;
            }else{
                head=mid+1;
            }
        }
    return head;
    }
};

```



#### 查找最后一个小于等于给定值的下标（111110000）

> 输入：n个数，k个待查找的元素，下面一行为k个元素，输出最后一个小于等于给定值的下标，没有输出-1；
>
> 5 4
> 4 4 6 6 9
> 5 6 7 3
>
> 输出： 1 3 3 -1；



```cpp
#include<iostream>
using namespace std;

int num[500005],n,k;
int main(){
    //采用scanf读入元素比较快：
    scanf("%d%d",&n,&k);
    for(int i=0;i<n;i++){
        scanf("%d",&num[i]);
    }
    for(int i=0;i<k;i++){
        int t,l=0,r=n-1;
        scanf("%d",&t);
        if(i!=0){
            cout<<" ";
        }
        if(num[0]>t){
            cout<<-1;
            continue;
        }
        while(l!=r){
            int mid=(l+r+1)>>1;
            if(num[mid]<=t){
                l=mid;
            }else{
                r=mid-1;
            }
        }
        cout<<l;
    }
    return 0;
}

```



#### [69. x 的平方根（1100）](https://leetcode-cn.com/problems/sqrtx/)

> 实现 `int sqrt(int x)` 函数。计算并返回 *x* 的平方根，其中 *x* 是非负整数。
>
> 由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

```c++
class Solution {
public:
    int mySqrt(int x) {
        int left = 0, right = x , ans = x;
        while (left <= right) {
            int mid = ((right - left) >> 1) + left;
            if (x <(long long)mid*mid) {
                right = mid - 1;
            } else {
                ans=mid;
                left = mid + 1;
            }
        }
        return ans;
        }
  }
//利用上述模板;111110000问题，找小于等于x(<=为真即为1,)的最后一个一
class Solution {
public:
    int mySqrt(int x) {
        long long  l=0,r=x;
        while(l!=r){
            long long mid=l+(r-l+1)/2;
            if(mid*mid<=x){
                l=mid;
            }else{
                r=mid-1;
            }
        }
        return l;
    }
}
```

#### oj390.原木切割(二分答案整数切割)

![](E:\Study_Notes\图片\leetcode\原木切割.png)

```c++
/*二分答案，解题思路：
长度范围为1~max(长度),输入一个长度会计算得到分成几段，当长度小于答案时，分成段数一定大于给定的段数，即可以等价为1111100000问题，求满足条件的最后一个1.
*/
#include<iostream>
using namespace std;

int num[100005];

int func(int n){
    int sum=0;
    for(int i=0;i<n;i++){
        sum+=num[i]/n;
    }
    return sum;
}

int main(){
    int n ,m;
    cin>>n>>m;
    int l=1,lr=0;
    for(int i=0;i<n;i++){
        cin>>num[i];
        lr=max(num[i],lr);

    }
    while(l!=lr){
        int mid=(l+lr+1)/2;
        int s=func(mid);
        if(s>=m){
            l=mid;
        }else{
            lr=mid-1;
        }
    }
    cout<<l<<endl;
    return 0;
}

```

#### oj393.绳子切割（小数分割）

![](E:\Study_Notes\图片\面视算法\393.png)

```c++
#include<iostream>
#include<cstdio.h>

int n,m;
double num[10005],tr;


int main(){
    cin>>n>>m;
    for(int i=0;i<n;i++){
        cin>>num[i];
        tr=max(tr,num[i]);
    }
    double l=0,r=tr;
    while(r-l>0.00001){
        double mid=(l+r)/2;
        itn s= func(mid);
        if(s>=m){
            l=mid;
        }else{
            r=mid;
        }
    }
    //保留两位数字，舍弃小数位方法：(*100在/100,或者-0.005在四舍五入)
    printf("%.2f\n",((int)l*100)/100.0);
	return 0;
}
```



#### oj389.暴躁的程序员（二分答案）

![](E:\Study_Notes\图片\面视算法\390.png)

```c++
#include<iostream>
#include<algorithm>
int num[100005];

int func(int d){
    int s=1;last=num[0];
    for(int i=1;i<m;i++){
        if(num[i]-last>=d){
            s++;
            last=num[i];
        }
    }
    return s;
}
int main(){
    int m,n;
    int l=1,r=0;
    cin>>m>>n;
    for(int i=0;i<m;i++){
        cin>>num[i];
        r=max(r,num[i]);
    }
    sort(num,num+n);
    while(l!=r){
        int mid=(l+r+1)/2;
        int s=func(mid);
        if(s>=n){
            l=mid;
        }else{
            r=mid-1;
        }
    }
    cout<<l<<endl;
	return 0;
}
```

#### oj82.伐木

![](E:\Study_Notes\图片\面视算法\oj82.png)

```c
#include<iostream>
using namespace std;

int n,m,num[1000005],tr;

long long func(int h){
    long long s=0;
    for(int i=0;i<n;i++){
        if(h<num[i]){
            s+=num[i]-h;
        }
    }
    return s;
}
int bin_search(){
    int l=0,r=tr;
    while(l!=r){
        int mid=((long long )l+r+1)/2;
        long long s=func(mid);
        if(s>=m){
            l=mid;   
        }else{
            r=mid-1;
        }
    }
    return r;
}
int main(){
    cin>>n>>m;
    for(int i=0;i<n;i++){
        cin>>num[i];
        tr=max(tr,num[i]);
    }
    cout<<bin_search()<<endl;
    return 0;
}
```

#### oj391数列分段

![](E:\Study_Notes\图片\面视算法\数列分段.png)

```c++
#include<iostream>
using namespace std;

long long n,m,num[100005],tl,tr;

long long func(long long x){
    long long cnt =0;now=0;
    for(int i=0;i<n;i++){
        if(now+num[i]==x){
            cnt++;
            now=0;
        }else if(now+num[i]>x){
            cnt++;
            now=num[i];
        }else{
            now+=num[i];
        }
    }
    if(now){
        cnt++;//判断最后一段是否统计
    }
	return cnt;
}

long long bin_search(){
    long long l=tl,r=tr;
    while(l!=r){ 
    long long mid=(l+r)/2;
    long long s=func(mid);
    if(s<=m){
        r=mid;
    }else{
        l=mid+1;
    }
    }
    return l;
}
int main(){
    cin>>n>>m;
    for(int i=0;i<n;i++){
        cin>num[i];
        tl=max(tl,num[i]);
        tr+=num[i];
    }
    cout<<bin_search()<<endl;
    return 0;
}
```

#### oj394.跳石头

```c
#include<iostream>
using namespace std;

int ll,n,m,num[50005],tl;

int func(int d){
    //cnt最终移走石头
    int cnt=0,last=0;
    for(int i=1;i<=n+1;i++){
        if(num[i]-last<d){
            cnt++;
        }else{
            last=num[i];
        }
    }
    return cnt;
}

int bin_search(){
    int l=tl,r=ll;
    while(l!=r){
        int mid=(l+r+1)/2;
        int s=func(mid);
        if(s<=m){
            l=mid;
        }else{
            r=mid-1;
        }
    }
    return l;
}

int main(){
    cin>>ll>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>num[i];
        if(i==1){
            tl=num[1]-num[0];
        }else{
            tl=min(tl,num[i]-num[i-1]);
        }
    }
    num[n+1]=ll;
    cout<<bin_search()<<endl;
	return 0;
}
```

744.大于给定元素的最小元素
[leetcode744](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

**解题思路**：此题为找到大于目标字符的第一个字符，及0000011111问题查找第一个1，利用模板即可。另外注意特殊情况，当目标值比最后一个字符都大的时候，返回第一个字符串。
```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int l=0,r=letters.size()-1;
        //考虑特殊情况
        if(letters[letters.size()-1]<=target){
            return letters[0];
        }
        while(l!=r){
            int mid=(l+r)/2;
            if(letters[mid]>target){
                r=mid;
            }else{
                l=mid+1;
            }
        }
        return letters[l];
    }
};
```

#### 540.查找有序数组中的单个元素

[leetcode540](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)
**要求**：时间复杂度为(OlogN）,因此不能遍历数组用异或操作，
**解题思路**：因为数组大小肯定为奇数，因此可以只索引偶数节点。

>  - 奇数长度的数组首尾元素索引都为偶数，因此我们可以将 lo 和 hi 设置为数组首尾。 我们需要确保 mid 是偶数，如果为奇数，则将其减1。 
>  - 然后，我们检查 mid 的元素是否与其后面的索引相同。 如果相同，则我们知道 mid 不是单个元素。且单个元素在 mid之后。则我们将 lo 设置为 mid + 2。 如果不是，则我们知道单个元素位于 mid，或者在 mid 之前。我们将 hi 设置为mid。 
>  - 一旦 lo == hi，则当前搜索空间为 1 个元素，那么该元素为单个元素，我们将返回它。
>  作者：LeetCode
>  链接：https://leetcode-cn.com/problems/single-element-in-a-sorted-array/solution/you-xu-shu-zu-zhong-de-dan-yi-yuan-su-by-leetcode/


```cpp
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        // int ans=nums[0];
        // for(int i=1;i<nums.size();i++){
        //     ans^=nums[i];
        // }
        // return ans;
        int l=0,r=nums.size()-1;
        while(l!=r){
            int mid=(l+r)/2;
            if(mid%2){
                mid--;//保证待查找区间大小是基数，索引一直是偶数
            }
            if(nums[mid]==nums[mid+1]){
                l=mid+2;
            }else{
                r=mid;
            }
        }
        return nums[l];
    }
```

#### 278.第一个错误的版本

[leetcode278](https://leetcode-cn.com/problems/first-bad-version/)
这题比较简单，0000111问题找第一个满足条件的1
```cpp
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l=0,r=n;
        while(l!=r){
        //注意写成l+(r-l)/2;否则会溢出
            int mid=l+(r-l)/2;
            if(isBadVersion(mid)){
                r=mid;
            }else{
                l=mid+1;
            }
        }
        return l;
    }
};
```

#### 153.旋转数组中的最小数字

[leetcode153](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
没什么好说的，0001111问题查找第一个1
```cpp
class Solution {
public:
//前面一堆0，后面一堆1，找带一个1，其中为1的条件是，右边大于等于中间的值
    int findMin(vector<int>& nums) {
        // sort(nums.begin(),nums.end());
        // return *nums.begin();
        int l=0,r=nums.size()-1;
        while(l!=r){
            int mid=(l+r)/2;
            if(nums[mid]<=nums[r]){
                r=mid;
            }else{
                l=mid+1;
            }
        }
        return nums[l];
    }
};
```

#### 34.查找区间
[leetcode34](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/?utm_source=LCUS&utm_medium=ip_redirect&utm_campaign=transfer2china)
**题目描述**：给定一个有序数组 nums 和一个目标 target，要求找到 target 在 nums 中的第一个位置和最后一个位置。
**解题思路**：可以用二分查找找出第一个位置和最后一个位置，但是寻找的方法有所不同，需要实现两个二分查找。即000111问题找第一个1和1111000问题找最后一个1，最后两者的并集即为答案。
但是如果我们将寻找 target 最后一个位置，转换成寻找 target+1 第一个位置，再往前移动一个位置。这样我们只需要实现一个二分查找代码即可。

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.size()==0) return {-1,-1};
       int l=0,r=nums.size()-1;
       while(l!=r){
           int mid=(l+r)/2;
           if(nums[mid]>=target){
               r=mid;
           }else{
               l=mid+1;
           }
       }
       if(nums[l]!=target) return {-1,-1};
       int start=l;

       l=0,r=nums.size()-1;
       while(l!=r){
           int mid=(l+r+1)/2;
           if(nums[mid]<=target){
               l=mid;
           }else{
               r=mid-1;
           }
       }
       if(nums[l]!=target) return {-1,-1};
       int end=r;
       return {start,end};
    }
};
```
更改后的代码

```cpp
class Solution {
public:
    int findFirst(vector<int>nums,int target){
        int l=0,h=nums.size();//注意h的初始值
        while(l!=h){
            int mid=l+(h-l)/2;
            if(nums[mid]>=target){
                h=mid;
            }else{
                l=mid+1;
            }
        }
        return l;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        int first=findFirst(nums,target);
        int last=findFirst(nums,target+1)-1;
        if(first==nums.size()||nums[first]!=target){
            return {-1,-1};
        }
        return {first,max(first,last)};
    }
};
```
在寻找第一个位置的二分查找代码中，需要注意 h 的取值为 nums.size()，而不是 nums.size() - 1。
先看以下示例：

> nums = [2,2], target = 2

如果 h 的取值为 nums.size() - 1，那么 last = findFirst(nums, target + 1) - 1 = 1 - 1 = 0。这是因为 findLeft 只会返回 [0, nums.length - 1] 范围的值，对于 findFirst([2,2], 3) ，我们希望返回 3 插入 nums 中的位置，也就是数组最后一个位置再往后一个位置，即 nums.size()。所以我们需要将 h 取值为 nums.size()，从而使得 findFirst返回的区间更大，能够覆盖 target 大于 nums 最后一个元素的情况。

## 动态规划

动态规划五部曲：

> 1、确定dp数组以及下标含义
>
> 2、确定递推公式
>
> 3、dp数组如何初始化
>
> 4、确定遍历顺序
>
> 5、举例推导dp数组
>

### 练习题

#### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

> 给定一个包含非负整数的 `m x n 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**解题思路**：

![image-20201121193229346](E:\Study_Notes\图片\面视算法\64最小路径和动态规划)

```c++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.size()==0||grid[0].size()==0) return 0;
        int m=grid.size();
        int n=grid[0].size();
        int dp[m][n];
        dp[0][0]=grid[0][0];
        for(int j=1;j<n;j++){
            dp[0][j]=dp[0][j-1]+grid[0][j];
        }
        for(int i=1;i<m;i++){
            dp[i][0]=dp[i-1][0]+grid[i][0];
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=min(dp[i-1][j],dp[i][j-1])+grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```



#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

> 给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```c++
//解法一:暴力枚举
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max=INT_MIN;
        for(int i=0;i<nums.size();i++){
            int sum=0;
            for(int j=i;j<nums.size();j++){
                sum+=nums[j];
                if(max<sum){
                    max=sum;
                }
            }
        }
        return max;
    }
};
//动态规划
/*思路分析：
dp[i]表示nums中以nums[i]结尾的最大自序和
dp[i]=max(dp[i-1]+nums[i],nums[i]);
dp[i]是当前数字或与前面的最大自序和的和；
可以开辟一维数组或者int 一个整型变量
*/
class Solution(){
public:
    int maxSubArray(vector<int>&nums){
        int ans=INT_MIN,now=0;
        for(int i=0;i<nums.size();i++){
            now=max(nums[i]+ans,nums[i]);
            ans=max(now,ans);
        }
        return ans;
    }
};
//贪心算法
/*思路分析：
从左向右迭代，一个个数字相加，如果sum<0，重新开始找子序串
*/
class Solution
{
public:
    int maxSubArray(vector<int> &nums)
    {
       int ans=INT_MIN;
       int sum=0;
       for(int i=0;i<nums.size();i++){
           sum+=nums[i];
           ans=max(ans,sum);
           if(sum<0){
               sum=0;
           }
       }
      return ans;
    }
};

```

双指针：

```c++
#include<iostream>
#include<cstdio.h>
using namespace std;
int n,int t,num[1000005];
int  main(){
    scanf("%d%d",&n,&t);
    for(int i=0;i<n;i++){
        scanf("%d",num[i]);
    }
    int l=0,r=n-1;
    while(l<r){
        if(num[l]+num[r]==t){
            cout<<l<<" "<<r<<endl;
            return 0;
        }
        if(num[l]+num[r]<t){
            l++;
        }else{
            r--;
        }
    }
    cout<<-1<<endl;
    return 0;
}
```

#### 切割回文oj46

![image-20210116193637809](E:\Study_Notes\图片\切割回文)

##### 状态定义：

$dp[i]$ 代表取字符串的前 $i$ 位，最少分成多少段回文串

##### 状态转移

$dp[i] = min(dp[j]) + 1\ |条件：\ s[j + 1, i]\ is\ palindrome$

1. 根据状态转移，算法时间复杂度$O(n^2)$
2. 所以，我们需要对转移阶段进行优化
3. 动态规划优化章节的时候，重点解决

##### 代码实现

```cpp
#include<iostream>
using namespace std;

#define max_n 50000

int dp[max_n+5];

bool is_huiwen(string s,int l,int r){
    while(l<r){
        if(s[l++]-s[r--]) return false;
    }
    return true;
}

int main(){
    string s ;
    cin>>s;
    //dp[i]:表示以第i个字符结尾的切割刀数;
    dp[0]=0;
    for(int i=1;i<=s.size();i++){
        dp[i]=dp[i-1]+1;
        for(int j=0;j<i;j++){
            if(is_huiwen(s,j,i-1)){
                dp[i]=min(dp[i],dp[j]+1);
            }
        }
    }
    cout<<dp[s.size()]-1<<endl;
    return 0;
}
```



#### 最长上升子序列

##### 状态定义

$dp[i]$，代表以 i 位位置上的值做为结尾的最长上升子序列的长度

##### 状态转移

$dp[i] = max(dp[j]) + 1 | val_j < val_i$

##### 优化方式

1. 维护一个单调数组 len，len[i] 代表长度为 i 的序列的结尾最小值
2. $dp[i]$ 在转移的时候，在 len 数组中查找第一个 $len[k]>=val_i$ 的位置，$dp[i] = k$
3. 更新 $len[k] = val_i$
4. 需要明确，len 数组为什么是单调的
5. 证明过程：假设，更新前是单调的，更新以后，一定是单调的
6. 在 len 数组中查找位置 k，实际上就是二分算法搞定

**代码实现：**

```cpp
#include<cstring>
#include<iostream>
using namespace std;

#define max_n 1000000
int arr[max_n+5];
int len[max_n+5];
int dp[max_n+5];

int binary_search(int *arr,int n,int x){
    int head=0,tail=n,mid;
    while(head<tail){
        mid=(head+tail)>>1;
        if(arr[mid]>=x){
            tail=mid;
        }else{
            head=mid+1;
        }
    }
        return head;
    }

int main(){
    int n,a;
    scanf("%d",&n);
    memset(len,0x3f,sizeof(len));
    len[0]=0;
    int ans=0;
    for(int i=1;i<=n;i++){
            cin>>arr[i];
    }
    for(int i=1;i<=n;i++){
        //在len数组中找到第一个大于等于arr[i]的位置：
        dp[i]=binary_search(len ,ans+1,arr[i]);
        len[dp[i]]=arr[i];
        ans=max(ans,dp[i]);
    }
    cout<<ans<<endl;
    return 0;
}

```



#### 01背包问题(oj47)

![](E:\Study_Notes\图片\01背包.png)

1. 抽象问题，背包问题抽象为寻找组合（x1,x2,x3...xn，其中xi取0或1，表示第i个物品取或者不取），vi代表第i个物品的价值，wi代表第i个物品的重量，总物品数为n，背包容量为c，求满足背包容量的最大价值。
2. 建模，问题即求max(x1v1 + x2v2 + x3v3 + ... + xnvn)。
3. 约束条件，x1w1 + x2w2 + x3w3 + ... + xnwn < c
4. 定义函数KS(i,j)：代表当前背包剩余容量为j时，前i个物品最佳组合所对应的价值；

那这里的递推关系式是怎样的呢？对于第i个物品，有两种可能：

1. 背包剩余容量不足以容纳该物品，此时背包的价值与前i-1个物品的价值是一样的，KS(i,j) = KS(i-1,j)

2. 背包剩余容量可以装下该商品，此时需要进行判断，因为装了该商品不一定能使最终组合达到最大价值，如果不装该商品，则价值为：KS(i-1,j)，如果装了该商品，则价值为KS(i-1,j-wi) + vi，从两者中选择较大的那个
   $$
   dp[i][j]=max(dp[i-1][j],dp[i-1][j-w[i]]+v[i])
   $$
   

```cpp

#include<iostream>
using namespace std;

#define max_n 100
#define max_v 10000

int v[max_n+5],w[max_n+5];
int dp[max_n+5][max_v+5];

int main(){
    int V, n;
    cin>>V>>n;
    for(int i=1;i<=n;i++) cin>>v[i]>>w[i];
    for(int i=1;i<=n;i++){
        for(int j=1;j<=V;j++){
            dp[i][j]=dp[i-1][j];
            if(j>=v[i]){
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-v[i]]+w[i]);
            }
        }
    }
    cout<<dp[n][V]<<endl;
    return 0;
}
//优化2：滚动数组dp[i%2]=dp[(i-1)%2]....
//优化3:优化为一维数组
#include<iostream>
using namespace std;

#define max_n 100
#define max_v 10000

int dp[max_v+5];

int main(){
    int V, n,v,w;
    cin>>V>>n;
    for(int i=1;i<=n;i++){
        cin>>v>>w;
        //逆序
        for(int j=V;j>=v;j--){
            dp[j]=max(dp[j],dp[j-v]+w);
        }
    }
    cout<<dp[V]<<endl;
    return 0;
}

```

#### 完全背包问题(oj48)

![](E:\Study_Notes\图片\完全背包问题.png)

##### **状态定义**

$dp[i][j]$ 前 i 个物品，背包最大承重为 j 的情况下，最大价值

##### **状态转移**

$$dp[i][j] = max\left\{\begin{aligned}&dp[i-1][j]&没选第 i 件\\&dp[i][j-v[i]]+w[i] &选了若干个第 i 件\end{aligned}\right.$$

程序实现的时候，参考01背包的程序实现，将逆向刷表，改成正向刷表

##### 代码实现

```cpp
#include<iostream>
using namespace std;

#define max_n 10000
int dp[max_n+5];
int main(){
    int V,n,v,w;
    cin>>n>>V;
    for(int i=1;i<=n;i++){
        cin>>v>>w;
        for(int j=v;j<=V;j++){
            dp[j]=max(dp[j],dp[j-v]+w);
        }
    }
    cout<<dp[V]<<endl;
    return 0;
}
```



#### 多重背包问题(oj49)

![](E:\Study_Notes\图片\多重背包.png)

##### 问题模型转换

1. 多重背包，每类物品多了一个数量限制
2. 01背包，每种物品只有一个
3. 将多重背包中的数量限制，当做多个单一物品来处理
4. 至此就将多重背包，转成了0/1背包问题

##### 状态定义

$dp[i][j]$ 前 i 个物品，背包最大承重为 j 的情况下，最大价值

##### 状态转移

$$dp[i][j] = max\left\{\begin{aligned}&dp[i-1][j]&没选第 i 件\\&dp[i-1][j-v[i]]+w[i] &选了第 i 件\end{aligned}\right.$$

##### 代码实现

```cpp
#include<iostream>
using namespace std;

#define max_n 100000
int dp[max_n+5];

int main(){
    int V,n,v,w,s;
    cin>>V>>n;
    for(int i=1;i<=n;i++){
        cin>>v>>w>>s;
        //增加一层循环
        for(int k=0;k<s;k++){
            for(int j=V;j>=v;j--){
                dp[j]=max(dp[j],dp[j-v]+w);
            }
        }
    }
    cout<<dp[V]<<endl;
    return 0;
}
```

##### 转移过程优化：

**二进制过程拆分法**

```cpp
#include<iostream>
using namespace std;

#define max_n 100000
int dp[max_n+5];

int main(){
    int V,n,v,w,s;
    cin>>V>>n;
    for(int i=1;i<=n;i++){
        cin>>v>>w>>s;
        //增加一层循环
        for(int k=1;s;k*=2){
            if(k>s)  k=s;
            s-=k;
            for(int j=V;j>=k*v;j--){
                dp[j]=max(dp[j],dp[j-k*v]+k*w);
            }
        }
    }
    cout<<dp[V]<<endl;
    return 0;
}
```

1. 本质上，对于某一类物品，我们具体要选择多少件，才是最优答案
2. 普通的单一拆分法，实际上只是想枚举某个物品选择 1--s 件的所有情况
3. 二进制拆分法可以达到相同的效果，拆分出来的物品数量会更少
4. 拿14举例，普通拆分法 14 份，二进制拆分法 4 份物品

**时间复杂度：**$O(nm\sum_{i=1}^{i=n}{logs_i})$

**最优时间复杂度：**$O(nm)$，借助单调队列，后续再讲

**01背包时间复杂度：**$O(nm)$

**完全背包时间复杂度：**$O(nm)$



#### 扔鸡蛋问题(oj50)

![](E:\Study_Notes\图片\扔鸡蛋问题.png)

##### 解题思路：

定义状态变量：$$dp[n][m]$$表示n个鸡蛋，m层楼的最少测试次数；

中间定义一个变量k(枚举所有的楼层)；假设第一个鸡蛋第K层楼碎了，则需要向下查找，查找次数为$$dp[n-1][k-1]$$次；假如没有碎，则应该向上查找 ，查找次数为$$dp[n][m-k]$$;最坏的情况下，$$dp[n][m]$$为两者的最大值；而最少测多少次为枚举所有楼层K之后这里面最小的一个值；

则有以下状态转移方程：

$$dp[n][m] = min(max\left\{\begin{aligned}&dp[n-1][k-1]+1&鸡蛋碎了\\&dp[n][m-k]+1 &鸡蛋没碎\end{aligned})\right.$$

##### 代码实现

```cpp
#define MAX_N 32
#define MAX_M 1000000
int dp[MAX_N + 5][MAX_M + 5];

int main() {
    int n, m;
    cin >> n >> m;//n个鸡蛋，m层;
    //初始化边界条件，1个鸡蛋i层需要测i次,自底向上查询;
    for (int i = 0; i <= m; i++) dp[1][i] = i;
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = j;
            //循环遍历楼层，选出最优解
            for (int k = 1; k <= j; k++) {
                dp[i][j] = min(dp[i][j], max(dp[i - 1][k - 1], dp[i][j - k]) + 1);
            }
        }
    }
    cout << dp[n][m] << endl;
    return 0;
}

```

时间复杂度：$$O(n*m^2)$$

##### 优化1：

优化K的选取：最优的K值即为$$dp[n-1][k-1]$$和$$dp[n][m-k]$$的交点，而随着K的增大，前者慢慢增大，后者慢慢减小，所以K即为满足$$dp[n-1][k-1]\lt dp[n][m-k]$$的最后一个K值；总体复杂度为O(n*m);

```cpp
#include<iostream>
using namespace std;
#define MAX_N 32
#define MAX_M 1000000
int dp[MAX_N+5][MAX_M+5];

int main(){
    int n,m;
    cin>>n>>m;
    for(int i=0;i<=m;i++) dp[1][i]=i;//一个鸡蛋测m次；
    //两个鸡蛋两层楼开始,
    for(int i=2;i<=n;i++){
        int k=2;
        dp[i][1]=1;
        for(int j=2;j<=m;j++){
            while(k<=j&&dp[i-1][k-1]<dp[i][j-k]) ++k;
            dp[i][j]=max(dp[i-1][k-1],dp[i][j-k])+1;
        }
    }
    cout<<dp[n][m]<<endl;
    return 0;
}
```

##### 优化2:

状态定义的优化：

1. 原状态定义所需存储空间与 m 相关，m 值域大，所以存不下
2. **当发现某个自变量与因变量之间存在相关性的时候，两者即可对调**
3. $dp[n][m]=k$ 重定义为$dp[n][k]=m$，代表 n 个鸡蛋扔 k 次，最多测多少层楼
4. k 的值域小，当 n=2 时，$k \le \sqrt{2m}$ 

$$dp[n][k]=dp[n-1][k-1]+dp[n][k-1]+1$$

```CPP
#include<iostream>
using namespace std;
#define MAX_N 100
#define MAX_K 500000
long long  dp[MAX_N+5][MAX_K+5];

int sovle(int n,int m){
    if(n==1) return m;
    //初始化dp[1][k]，一枚鸡蛋测K次最多测K层楼
    for(int i=1;i<=MAX_K;i++) dp[1][i]=i;
    for(int i=2;i<=n;i++){
        for(int k=1;k<=MAX_K;k++){
            dp[i][k]=dp[i-1][k-1]+dp[i][k-1]+1;
        }
    }
    int k=1;
    //满足dp[n][k]大于m的第一个K即为求得的答案；见下面分析
    while(dp[n][k]<m) k++;
    return k;
}

int main(){
    int n,m;
    cin>>n>>m;
    cout<<solve(n,m)<<endl;
    return 0;
}
```

```
当n=2，m=10时,对应的答案是14;
n=2
k=1,dp=1
k=2,dp=3
k=3,dp=6
k=4,dp=10
k=5,dp=15
k=6,dp=21
k=7,dp=28
k=8,dp=36
k=9,dp=45
k=10,dp=55
k=11,dp=66
k=12,dp=78
k=13,dp=91
k=14,dp=105
k=15,dp=120
```



### 如何求解递推问题

> 1. 确定递推状态：一个数学符号加上一个符号状态的语义表示
>
>    > 学习递推状态的技巧；
>    > $$
>    > f(x)=y
>    > $$
>    > y:问题中的求解量，也是我们所谓的因变量；
>    >
>    > x:问题中直接影响求解量的部分，也就是我们所谓的自变量；
>    >
>    > 本质：寻找问题中的自变量和因变量；
>
> 2. 确定递推公式
>
>    > 本质：分析问题中的容斥关系；
>    > $$
>    > f(n)=f(n-1)+f(n-2)
>    > $$ { }
>    > 
>
> 3. 程序实现；（递归+记忆化）（递推/循环实现）；

#### 练习题 1：爬楼梯

> 每次只能爬两层或者三层:

$$
f(n)=f(n-2)+f(n-3)
$$



#### 练习题2：墙壁涂色（oj42)

> 大小为wallsize的环形墙壁涂色，三种颜色红、黄、蓝，问一共有多少种涂色方案；

解题思路：

解题1：拆成非环的一维序列；

因变量：方法总数；

自变量：墙壁数量

解题技巧：头颜色，尾颜色
$$
f(n,i,j):n块墙壁，头部涂颜色i，尾部涂颜色j,所代表的方法总数
$$
所有

解答2：
$$
f(1,3)和f(2,3)的值相等\n
f(n,j):代表n块墙壁，第一块0种颜色（隐变量），最后一块涂第j种颜色;
$$
解答3：

f(n)：代表首尾颜色不同的方案总数；

例如墙壁为1......234；可以分为两种情况，1和3颜色一样，1和3颜色不同；当1和3颜色一样，则4的颜色有两种情况，第1块和第2块颜色不同的方案总数为f(n-2)；当1和3颜色不一样时，4的颜色只有一种方案，则方案总数为f(n-1)；则有：
$$
f(n)=2*f(n-2)+f(n-1);
$$
代码实现（大整数加法）：

```cpp
#include<iostream>
#include<vector>
using namespace std;

class BigInt:public vector<int>{
public:
    BigInt(){
        push_back(0);
    }
    BigInt(int x){
        push_back(x);
        process_digit();
    }
    BigInt operator*(int x){
        BigInt ret(*this);
        ret*=x;
        return ret;
    }
    void operator*=(int x){
        for(int i=0;i<size();i++) at(i)*=x;
        process_digit();
        return;
    }
    void operator+=(const BigInt &num){
        for(int i=0;i<num.size();i++){
            if(i==size()) push_back(num[i]);
            else at(i)+=num[i];
        }
        process_digit();
        return ;
    }
    BigInt operator+(const BigInt &num){
        BigInt ret(*this);
        ret+=num;
        return ret;
    }
    void process_digit(){
        for(int i=0;i<size();i++){
            if(at(i)<10) continue;
            if((i+1)==size()) push_back(0);
            at(i+1)+=at(i)/10;
            at(i)%=10;
        }
        return ;
    }
};


ostream &operator<<(ostream &out,const BigInt &num){
    for(int i=num.size()-1;i>=0;i--){
        out<<num[i];
    }
    return out;
}

int main(){
    int n,k;
    BigInt f[3]={0};
    cin>>n>>k;
    f[1]=k;
    f[2]=k*(k-1);
    f[0]=k*(k-1)*(k-2);
    for(int i=4;i<=n;i++){
        //滚动数组;
        f[i%3]=f[(i-1)%3]*(k-2)+f[(i-2)%3]*(k-1);
    }
    cout<<f[n%3]<<endl;
    return 0;
}

```





#### 练习题3：钱币问题

> 现在给你1、2、5、10、20、50、100、200元的钱币若干张，问要想凑足N元钱，一共有多少种不同的方法？注意（1，1，2）和（1，2，1）属于同一种方法；

解题思路;

> $$
> f(目标金额，使用钱币的种类)=方法种数
> $$
>
> 定义
> $$
> f(n,m)
> $$
> 为使用前n种钱币组成金钱总数为m的种数，则可以分为不利用第n种钱币的数量f(n-1,m)加上使用第n种钱币的数量f(n,m-val(n));m中减去第n种金币的面额利用n种钱币组成的数量；

#### 数字三角形：

$$

$$

### 如何求解动态规划问题

##### 动规5部曲

> 1. 确定动规的状态；
> 2. 确定状态转移方程；
> 3. 正确性证明：状态初始化及遍历顺序
> 4. 程序实现;

### 动态规划优化的分类

> 1. 状态转移过程的优化，不改变状态定义，使用一些特殊的数据结构或者算法专门优化转移过程。
> 2. 程序实现的优化、例如：01背包问题。状态定义没有变、转移过程没有变。
> 3. 状态定义的优化：大量训练，源头优化；
> 4. 状态定义->源头，转移过程->过程，程序实现->结果；

程序优化：01优化、钱币优化、滚动数组；





## 双指针

#### 167.有序数组的两数之和

> [leetcode 167](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)
> **解题思路**：
> 这道题和力扣第一题无序的两数之和不同，因为是**有序**的，所以可以考虑利用双指针，
> 双指针思想：左指针指向最小的数值，从头向尾遍历，右指针指向最大的数值，从尾向头遍历。
>
>  - 如果target==sum,则返回结果
>  - 如果target>sum;则要将sum变大，移动左指针；
>  - 如果target<sum,则要将sum变小，移动右指针；
> 代码如下：

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
       vector<int>ans;
       int l=0,r=numbers.size()-1;
       while(l<r){
           if(numbers[l]+numbers[r]==target){
               ans.push_back(l+1);
               ans.push_back(r+1);
           }
           if(numbers[l]+numbers[r]<target){
               l++;
           }else{
               r--;
           }
       }
       return ans;
    }
};
```

#### 633.两数平方和

[leetcode633:两数平方和](https://leetcode.com/problems/sum-of-square-numbers/description/)
**解题思路**：方法和上面一样，确定左右边界即可左边界为0，有边界为根号target;注意这题要利用long long 类型，否则会溢出。

```cpp
class Solution {
public:
    bool judgeSquareSum(int c) {
        if(c==0) return true;
        long long  l=0,r=sqrt(c);
        while(l<=r){
            long long sum=l*l+r*r;
            if(sum==c){
                return true;
            }else if(sum>c){
                r--;
            }else{
                l++;
            }
        }
        return false;
    }
};
```

#### 345.反转字符串中的元音字符

[leetcode345](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)
**解题思路**：这题先要遍历一边字符串，把所有的元音字符存在一个容器中，可以是哈希表或是vector中，然后利用左右指针交换元音字符，注意元音字符包括大写的。

```cpp
class Solution {
public:
    string reverseVowels(string s) {
        vector<int>str;
        string ans=s;
        for(int i=0;i<s.size();i++){
            if(s[i]=='a'||s[i]=='e'||s[i]=='i'||s[i]=='o'||s[i]=='u'||s[i]=='A'||s[i]=='E'||s[i]=='I'||s[i]=='O'||s[i]=='U'){
                str.push_back(i);
            }
        }
        int l=0,r=str.size()-1;
        while(l<=r){
           ans[str[l]]=s[str[r]];
           ans[str[r]]=s[str[l]];
           l++;
           r--;
        }
        return ans;
    }

};
```
**优化方法**：上面需要利用额外的空间，可以一边遍历一遍交换，当头尾指针都遇到元音时交换两个字符。可以将全部元音字符存到一个字符串中
注意：npos可以表示string的结束位子，是string::type_size 类型的，也就是find（）返回的类型。find函数在找不到指定值得情况下会返回string::npos。

```cpp
class Solution {
public:
    string reverseVowels(string s) {
        string v = "aeiouAEIOU";
        int i = -1,j = s.size();
        while(i<j){
            while(i<j && v.find(s[++i])==v.npos);
            while(i<j && v.find(s[--j])==v.npos);
            swap(s[i],s[j]);
        }
        return s;
    }
};
```

#### 680.回文字符串
[leetcode 680](https://leetcode-cn.com/problems/valid-palindrome-ii/)
**解题思路**：本题的关键是处理删除一个字符。在使用双指针遍历字符串时，如果出现两个指针指向的字符不相等的情况，我们就试着删除一个字符，再判断删除完之后的字符串是否是回文字符串。在试着删除字符时，我们既可以删除左指针指向的字符，也可以删除右指针指向的字符。

```cpp
class Solution {
public:
//构造函数判断删除后的字符是否是回文字符串
    bool isPalindrome(string s,int i,int j){
        while(i<j){
            if(s[i++]!=s[j--]){
                return false;
            }
        }
        return true;
    }
    bool validPalindrome(string s) {
        for(int i=0,j=s.size()-1;i<j;i++,j--){
            if(s[i]!=s[j]){
                //删除左指针或者右指针后在判断
                return isPalindrome(s,i,j-1)||isPalindrome(s,i+1,j);
            }
        }
        return true;
       
    }
};
```

#### 141.判断链表是否存在环
[leetcode141](https://leetcode-cn.com/problems/linked-list-cycle/)
**解题思路**：设置双指针，一个快指针，一个慢指针，慢指针每次移动一个节点，快指针每次移动两个节点，如果存在环，那么这两个指针一定会相遇。
其中利用哈希表unordered_set也可以unordered_set.count()返回值为0或1，存在x,返回1，反之，返回0

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head==NULL) return false;
        //双指针，都从头节点开始，一个快指针一次走两步，一个慢指针走一步，如果存在环，则一定会重合。
        ListNode *p1=head,*p2=head;
        while(p2!=NULL&&p2->next!=NULL){
            p1=p1->next;
            p2=p2->next->next;
             if(p1==p2){
                return true;
            }
        }
        return false;  
    }
};
//利用哈希表存
unordered_set<ListNode*>us;
        while(head){
            if(us.count(head)){
                return true;
            }
            us.insert(head);
            head=head->next;
        }
        return false;
```

#### oj600杨氏矩阵

![](E:\Study_Notes\图片\面视算法\杨氏矩阵.png)

```c
/*
解题思路：
从左下角或者右上角开始查找，
*/
//n行m列，待查找的数为t;
int x=n,y=1;
while(1){
    if(num[x][y]==t){
        cout<<x<<" "<<y<<endl;
        return 0;
    }
    if(num[x][y]>t){
        x--;
    }else{
        y++;
    }
}
```

## 递归、排列、回溯

```c++
//oj235
int n,num[15];
void p(deep){
	for(int i=1;i<=deep;i++){
		if(i!=1){
			cout<<" ";
		}
        cout<<num[i];
	}
    cout<endl;
}
void func(int s,int deep){
    for(int i=s;i<=n;i++){
        num[deep]=i;
        p(deep);
        func(i+1;deep+1);
    }
}
int cnt=0;
void func1(int s){
    for(int i=s;i<=n;i++){
        num[cnt]=i;
        p();
        cnt++;
        func1(i+1);
        cnt--;
    }
}
int main(){
    cin>>n;
    func(1,1);
    func1(1);
    return 0;
}
```



```c++
//236.cpp
#include<iostream>
using namespace std;
int n,m ,num[15],cnt;
void p(){
    for(int i=0;i<cnt;i++){
        if(i){
            cout<<" ";
        }
        cout<<num[i];
    }
    cout<<endl;
}
void func(int s,int left){
    if(left==0){
        p();
        return ;
    }
    for(int i=s;i<=n;i++){
        num[cnt]=i;
        cnt++;
        func(i+1,left-1);
        cnt--;
    }
}

int main(){
    cin>>n>>m;
    func(1,m);
    return 0;
}

```

```c++
//oj237.cpp
#include<iostream>
using namespace std;
int n,num[15],mark[15],cnt;

void p(){
    for(int i=0;i<n;i++){
        if(i){
            cout<<" ";
        }
        cout<<num[i];
    }
    cout<<endl;
}

void func(int left){
    if(left==0){
        p();
        return ;
    }
    for(int i=1;i<=n;i++){
        if(mark[i]==0){
            mark[i]=1;
            num[cnt]=i;
            cnt++;
            func(left-1);
            cnt--;
            mark[i]=0;
        }
    }
}

int main(){
    cin>>n;
    func(n);
    return 0;
}

```

#### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

解题思路：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;  
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    TreeNode*func(vector<int>&nums,int left,int right){
        if(left>right){
            return NULL;
        }

        int mid=(left+right)/2;

        TreeNode *root=new TreeNode(nums[mid]);
        root->left=func(nums,left,mid-1);
        root->right=func(nums,mid+1,right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        if(nums.size()==0) return NULL;
        return func(nums,0,nums.size()-1);
        
    }
};
```

#### 正确的单词

![image-20210320225144128](E:\Study_Notes\图片\面视算法\正确的单词)

**解题思路：**

```cpp
//a 2个 b 2个c 3个 d 一个 a,b,c,d依次选几个，组合问题;


```

### 回溯

**回溯法解决的问题**

回溯法，一般可以解决如下几种问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 棋盘问题：N皇后，解数独等等

因为回溯法解决的都是在集合中递归查找子集，**「集合的大小就构成了树的宽度，递归的深度，都构成的树的深度」**。

**回溯模板**

**「for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历」**

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

组合问题：需要startIndex，每次i从startIndex开始;

排列问题：需要used[i]标记数组，每次i从零开始

#### 组合问题

##### 一个集合里面的组合问题

###### 216无重复数字无重复选取组合总和III

startIndex（int）为下一层for循环搜索的起始位置。

![image-20210321164130660](E:\Study_Notes\图片\面视算法\组合问题)

```cpp
class Solution {
private:
    vector<vector<int>> result; // 存放结果集 
    vector<int> path; // 符合条件的结果
    // targetSum：目标和，也就是题目中的n。 
    // k：题目中要求k个数的集合。 
    // sum：已经收集的元素的总和，也就是path里元素的总和。 
    // startIndex：下一层for循环搜索的起始位置。
    void backtracking(int targetSum, int k, int sum, int startIndex) {
        if (path.size() == k) {
            if (sum == targetSum) result.push_back(path);
            return; // 如果path.size() == k 但sum != targetSum 直接返回
        }
        for (int i = startIndex; i <= 9; i++) {
            sum += i; // 处理
            path.push_back(i); // 处理
            backtracking(targetSum, k, sum, i + 1); // 注意i+1调整startIndex
            sum -= i; // 回溯
            path.pop_back(); // 回溯
        }
    }

public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear(); // 可以不加
        path.clear();   // 可以不加
        backtracking(n, k, 0, 1);
        return result;
    }
};
```

注意：如果for循环是`for(int i=1;i<=9;i++)`结果里面将会出现重复的结果得到：

> [[1,1,5],[1,2,4],[1,3,3],[1,4,2],[1,5,1],[2,1,4],[2,2,3],[2,3,2],[2,4,1],[3,1,3],[3,2,2],[3,3,1],[4,1,2],[4,2,1],[5,1,1]]
>
> 这是因为for循环每次都从1开始，表明每个值都可以重复选取;只要满足和为n的K个数均可；

而`FOR循环下成如下代码时：

```cpp
for(int i=index;i<=9;i++){
    sum+=i;
    path.push_back(i);
    dfs(k,n,sum,index+1);
    path.pop_back();
    sum-=i;
}
```

得到的结果是：

> [[1,2,4],[1,3,3],[2,2,3]]
>
> 以上结果多出来了两个，这是为什么呢？？？？？，这是



###### 39 无重复数字可重复寻选取组合总和

![image-20210321172802985](E:\Study_Notes\图片\面视算法\组合总和)

这里选择的元素可以重复，说明for循环中从startIndex开始，而且递归调用也从i开始

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex) {
        if (sum > target) {
            return;
        }
        if (sum == target) {
            result.push_back(path);
            return;
        }

        for (int i = startIndex; i < candidates.size(); i++) {
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum, i); // 不用i+1了，表示可以重复读取当前的数
            sum -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        result.clear();
        path.clear();
        backtracking(candidates, target, 0, 0);
        return result;
    }
};
```

###### 40 有重复数字不能重复选取组合总和

![image-20210321173641409](E:\Study_Notes\图片\面视算法\组合总和II)

这道题目和[39.组合总和](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247485343&idx=1&sn=2c7e259454411002d2c6e0e39cc0b939&scene=21#wechat_redirect)如下区别：

1. 本题candidates 中的每个数字在每个组合中只能使用一次。
2. 本题数组candidates的元素是有重复的，而[39.组合总和](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247485343&idx=1&sn=2c7e259454411002d2c6e0e39cc0b939&scene=21#wechat_redirect)是无重复元素的数组candidates

最后本题和[39.组合总和](https://mp.weixin.qq.com/s?__biz=MzUxNjY5NTYxNA==&mid=2247485343&idx=1&sn=2c7e259454411002d2c6e0e39cc0b939&scene=21#wechat_redirect)要求一样，解集不能包含重复的组合。

**「本题的难点在于区别2中：集合（数组candidates）有重复元素，但还不能有重复的组合」**

难点在于如何去重：在同一树枝上去重还是同一树层上去重

![图片](E:\Study_Notes\图片\面视算法\去重)



前面我们提到：要去重的是“同一树层上的使用过”，如果判断同一树层上元素（相同的元素）是否使用过了呢。

**「如果`candidates[i] == candidates[i - 1]` 并且 `used[i - 1] == false`，就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]」**

![图片](E:\Study_Notes\图片\面视算法\树层还是树枝上去重)

图中将used的变化用橘黄色标注上，可以看出在candidates[i] == candidates[i - 1]相同的情况下：

- used[i - 1] == true，说明同一树支candidates[i - 1]使用过
- used[i - 1] == false，说明同一树层candidates[i - 1]使用过

所以这里需要把数组排序，将相同得元素放在一起，并用used标记数组标记同一树枝上面的元素是否已经被使用过;

**代码实现：**

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex, vector<bool>& used) {
        if (sum == target) {
            result.push_back(path);
            return;
        }
        //下面用到了剪枝；
        for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) {
            // used[i - 1] == true，说明同一树支candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if (i > 0 && candidates[i] == candidates[i - 1] && used[i - 1] == false) {
                continue;
            }
            sum += candidates[i];
            path.push_back(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used); // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            used[i] = false;
            sum -= candidates[i];
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(), false);
        path.clear();
        result.clear();
        // 首先把给candidates排序，让其相同的元素都挨在一起。
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0, used);
        return result;
    }
};

```



##### 两个集合的组合问题

![image-20210321161458624](E:\Study_Notes\图片\面视算法\电话号码)

**解题思路**

这题是在两个集合中组合得到不同的集合；这个index和一个集合中的startIndex含义不一样，

**「因为本题每一个数字代表的是不同集合，也就是求不同集合之间的组合，而上面都是求同一个集合中的组合！」**

```cpp
class Solution {
public:
    string path;
    vector<string>res;
    map<int,string>lettermap={{2,"abc"},{3,"def"},{4,"ghi"},{5,"jkl"},{6,"mno"},{7,"pqrs"},{8,"tuv"},{9,"wxyz"}};
    void dfs(string digits,int index){
        if(index==digits.size()){
            res.push_back(path);
            return ;
        }
        int x=digits[index]-'0';
        for(int i=0;i<lettermap[x].size();i++){
            path.push_back(lettermap[x][i]);
            dfs(digits,index+1);
            path.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        if(digits.size()==0) return res;
        dfs(digits,0);
        return res;

    }
};
```

#### 切割问题

##### 131 切割回文串

![image-20210321174926792](E:\Study_Notes\图片\面视算法\131)

本题涉及到两个关键问题：

1. 切割问题，有不同的切割方式
2. 判断回文

- 组合问题：选取一个a之后，在bcdef中再去选取第二个，选取b之后在cdef中在选取第三个.....。
- 切割问题：切割一个a之后，在bcdef中再去切割第二段，切割b之后在cdef中在切割第三段.....。

所以切割问题也可以用回溯来解决;

![图片](E:\Study_Notes\图片\面视算法\切割问题)

**「那么在代码里什么是切割线呢？」**

在处理组合问题的时候，递归参数需要传入startIndex，表示下一轮递归遍历的起始位置，这个startIndex就是切割线。

**「来看看在递归循环，中如何截取子串呢？」**

在`for (int i = startIndex; i < s.size(); i++)`循环中，我们 定义了起始位置startIndex，那么 [startIndex, i] 就是要截取的子串。

首先判断这个子串是不是回文，如果是回文，就加入在`vector<string> path`中，path用来记录切割过的回文子串。

**「注意切割过的位置，不能重复切割，所以，backtracking(s, i + 1); 传入下一层的起始位置为i + 1」**。

**代码实现：**

```cpp
class Solution {
private:
    vector<vector<string>> result;
    vector<string> path; // 放已经回文的子串
    void backtracking (const string& s, int startIndex) {
        // 如果起始位置已经大于s的大小，说明已经找到了一组分割方案了
        if (startIndex >= s.size()) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < s.size(); i++) {
            if (isPalindrome(s, startIndex, i)) {   // 是回文子串
                // 获取[startIndex,i]在s中的子串
                string str = s.substr(startIndex, i - startIndex + 1);
                path.push_back(str);
            } else {                                // 不是回文，跳过
                continue;
            }
            backtracking(s, i + 1); // 寻找i+1为起始位置的子串
            path.pop_back(); // 回溯过程，弹出本次已经填在的子串
        }
    }
    bool isPalindrome(const string& s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
public:
    vector<vector<string>> partition(string s) {
        result.clear();
        path.clear();
        backtracking(s, 0);
        return result;
    }
};
```



## DFS(深搜)

#### 算法思想

#### 62 不同路径

> 题目表述：从点【0，0】到【m,n】的路径总共有多少，
>
> 采用深度优先遍历，并且采用一个$vis$[]数组标记走过的路，记忆化搜索用二维数组$mem$记录从【0，0】到【m,n】的路径数

```cpp
class Solution {
public:
    vector<vector<int>>mem;
    vector<vector<int>>vis;
    int cnt=0;
    int dir[2][2]={0,1,1,0};
    //非记忆化搜索
    // void dfs(int x,int y,int m,int n,vector<vector<int>>&mem){
    //     if(x==m-1&&y==n-1){
    //         cnt++;
    //         return ;
    //     }
    //     mem[x][y]=1;
    //     for(int i=0;i<2;i++){
    //         int xx=x+dir[i][0];
    //         int yy=y+dir[i][1];
    //         if(xx>=0&&xx<m&&yy>=0&&yy<n&&mem[xx][yy]!=1){
    //             dfs(xx,yy,m,n,mem);
    //         }
    //     }
    //     mem[x][y]=0;
    // }
    //记忆化搜索;
    int memodfs(int x,int y,int m,int n,vector<vector<int>>&mem){
        if(x==m-1&&y==n-1){
            return 1;
        }
        vis[x][y]=1;
        int sum=0;
        for(int i=0;i<2;i++){
            int xx=x+dir[i][0];
            int yy=y+dir[i][1];
            if(xx>=0&&xx<m&&yy>=0&&yy<n&&vis[xx][yy]!=1){
                if(mem[xx][yy]!=0){
                    sum+=mem[xx][yy];
                }else{
                    sum+=memodfs(xx,yy,m,n,mem);
                }
            }
        }
        vis[x][y]=0;
        mem[x][y]=sum;
        return sum;
    }
    int uniquePaths(int m, int n) {
        mem.resize(m,vector<int>(n,0));
        vis.resize(m,vector<int>(n,0));
        //dfs(0,0,m,n,mem);
         return memodfs(0,0,m,n,mem);
    }
};
```

#### 朋友圈和省份数量

采用邻接矩阵存储的图

```cpp
class Solution {
public:
    void dfs(int i,vector<int>&visited,vector<vector<int>>&isConnected){
        visited[i]=1;
        for(int k=0;k<isConnected.size();k++){
            //如果当前顶点i未被访问，说明是新的连通域，则遍历新的联通域，且cnt+=1;
            if(isConnected[i][k]&&!visited[k]){
                dfs(k,visited,isConnected);
            }
        }
    }
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n=isConnected.size();
        vector<int>visited(n,0);
        int cnt = 0;
        for(int i=0;i<n;i++){
            if(!visited[i]){
                dfs(i,visited,isConnected);
                cnt++;
            }
        }
        return cnt;
    }
};
```



#### 01迷宫分块

![image-20210320223908261](E:\Study_Notes\图片\面视算法\01迷宫分块)



**解题思路**

```cpp
//0可以与1上下左右相邻的联通，1可以与0上下左右相邻联通，及上下左右不一样即可联通：遍历每个点，
#include<iostream>
using namespace std;
//存地图，check标记数组，记录是否遍历了当前点;
char mmap[2005][2005],check[2005][2005];
int n,m,ans;
int dir[4][2]={0,1,1,0,0,-1,-1,0};

void dfs(int x,int y){
    for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(xx==0||yy==0||xx==n+1||yy==m+1||check[xx][yy]==1){
            continue;
        }
        //判断新点和原始节点是否是同一个节点，如果直接改原始矩阵地图，不能判断是否一样，因此从新开了一个标记数组;如果新点和原始点不一样，继续深搜，找到一起联通区域;
        if(mmap[x][y]!=mmap[xx][yy]){
            check[xx][yy]=1;
            dfs(xx,yy);
        }
    }
}

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>&mmap[i][1];//一次读入一行数据放入mmap二维数组之中。
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(check[i][j]==0){
                ans++;
                check[i][j]=1;
                dfs(i,j);
            }
        }
    }
    cout<<ans<<endl;
    return 0;
}

```



#### 深搜走地图

```c++
//深搜走地图，递归问题，解决连通性问题
//#为障碍物，S为起点，T为终点，判断是否起点到终点是否有路径
#include<iostream>
using namespace std;
//sx,sy起点坐标
int n,m,sx,sy;
char mmap[105][105];
//方向数组
int dir[4][2]={0,1,1,0,0,-1,-1,0};

int func(int x,int y){
    //循环遍历四个方向
    for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(mmap[xx][yy]=='T'){
            return 1;
        }
        if(mmap[xx][yy]=='.'){
            mmap[xx][yy]='#';//走过的点变为#，更改地图，避免重复搜索
            if(func(xx,yy)==1){
                return 1;
            }
        }
    }
    return 0;
}

int main(){
    cin>>n>>m;
    for(int  i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
            //字符起点
            if(mmap[i][j]=='S'){
                sx=i,sy=j;
            }
        }
    }
    //递归返回值
    if(func(sx,sy)==1){
        cout<<"YES"<<endl;
    }else
   		cout<<"NO"<<endl;
    return 0;
}
```

```c++
//oj535.cpp--求最短路径
#include<iostream>
using namespace std;

int n,m,ans=1,sx,sy;
int dir[4][2]={0,1,1,0,0,-1,-1,0};
char mmap[105][150];

void func(int x,int y){
    for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(mmap[xx][yy]=='.'){
            ans++;
            mmap[xx][yy]=0;
            func(xx,yy);
        }
    }
}

int main(){
    cin>>m>>n;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
            if(mmap[i][j]=='@'){
                sx=i,sy=j;
            }
        }
    }
    func(sx,sy);
    cout<<ans<<endl;
    return 0;
}

```

```c++
//oj536.cpp--求最大连通域
#include<iostream>
using namespace std;

int n,m,ans,temp;
int dir[4][2]={0,1,1,0,0,-1,-1,0};
char mmap[105][150];

void func(int x,int y){
    for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(mmap[xx][yy]=='1'){
            temp++;
            mmap[xx][yy]=0;
            func(xx,yy);
        }
    }
}

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
            }
        }
    for(int i=1;i<=n;i++){  
        for(int j=1;j<=m;j++){  
            if(mmap[i][j]=='1'){
                temp=1;
                mmap[i][j]=0;
                func(i,j);
                ans=max(ans,temp);
            }
        }

    }
    cout<<ans<<endl;
    return 0;

}

```

```c++
#include<iostream>
using namespace std;

int n,m,mark[3003][3005],ans,sx,sy;
int dir[4][2]={0,1,1,0,0,-1,-1,0};
char mmap[3005][3005];

void func(int x,int y){
      for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(xx<0||yy<0||xx>n+1||yy>n+1||mark[xx][yy]==1){
            continue;
        }
          if(mmap[x][y]!=mmap[xx][yy]){
              ans++;
              mark[xx][yy]=1;
              func(xx,yy);
          }
}

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        cin>>&mmap[i][j];
    }
    cin>>sx>>sy;
    ans=1;
    mark[sx][sy]=1;
    func(sx,sy);
    cout<<ans<<endl;
}
```

```c++
//oj404
#include<iostream>
using namespace std;

int n,m,mark[3003][3005],ans,sx,sy;
int dir[4][2]={0,1,1,0,0,-1,-1,0};
char mmap[3005][3005];

void func(int x,int y){
    for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(xx<1||yy<1||xx>n||yy>m||mark[xx][yy]==1){
            continue;
        }
        if(mmap[x][y]!=mmap[xx][yy]){
            ans++;
            mark[xx][yy]=1;
            func(xx,yy);
        }
    }
}
    int main(){
        cin>>n>>m;
        for(int i=1;i<=n;i++){
            cin>>&mmap[i][1];
        }
        cin>>sx>>sy;
        ans=1;
        mark[sx][sy]=1;
        func(sx,sy);
        cout<<ans<<endl;
        return 0;

    }

```

```c++
//oj405.cpp
#include<iostream>
#include<queue>
using namespace std;
struct node{
    int x;
    int y;

};
queue<node>que;
int n,m,k,ans[3005][3005],temp;
int dir[4][2]={0,1,1,0,0,-1,-1,0};
char mmap[3005][3005];

void check(){
    while(!que.empty()){
        node t=que.front();
        que.pop();
        ans[t.x][t.y]=temp;
    }
}

void func(int x,int y){
    que.push((node){x,y});
    for(int i=0;i<4;i++){
        int xx=x+dir[i][0];
        int yy=y+dir[i][1];
        if(xx<1||yy<1||xx>n||yy>m||ans[xx][yy]!=0){
            continue;
        }
        if(mmap[x][y]!=mmap[xx][yy]){
            ans[xx][yy]=1;
            temp++;
            func(xx,yy);
        }
    }
}
int main(){
    cin>>n>>m>>k;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
        }
    }
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(ans[i][j]==0){
                temp=1;
                ans[i][j]=1;
                func(i,j);
                check();
            }
        }
    }

    for(int i=0;i<k;i++){
        int a,b;
        cin>>a>>b;
        cout<<ans[a][b]<<endl;
    }
    return 0;

}
```

```c++
//oj536.cpp
#include<iostream>
using namespace std;

int n,arr[25][25],check[25];

void func(int now){
    if(now!=1){
        cout<<"-";
    }
    cout<<now;
    check[now]=1;
    for(int i=1;i<=n;i++){
        if(check[i]==0&&arr[now][i]==1){
            func(i);
        }
    }
}

int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            cin>>arr[i][j];

        }

    }
    for(int i=1;i<=n;i++){
        if(check[i]==0){
            func(i);

        }

    }
    cout<<endl;
    return 0;

}

```

#### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

![image-20201121224421957](E:\Study_Notes\图片\leetcode\lc200)

```c++
class Solution {
public:
    int ans=0;
    int dir[4][2]={0,1,1,0,0,-1,-1,0};

    void dfs(vector<vector<char>>& grid,int x,int y){
        for(int i=0;i<4;i++){
            int xx=x+dir[i][0];
            int yy=y+dir[i][1];
            if(xx<0||yy<0||xx>grid.size()-1||yy>grid[0].size()-1||grid[xx][yy]=='0'){
                continue;
            }
            if(grid[xx][yy]=='1'){
                grid[xx][yy]=0;
                dfs(grid,xx,yy);
            }
        }
        return ;
    }
    int numIslands(vector<vector<char>>& grid) {
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]=='1'){
                    grid[i][j]=0;
                    ans++;
                    dfs(grid,i,j);
                }
            }
        }
        return ans;
    }
};
//
class Solution {
public:
    int dir[4][2]={0,1,1,0,0,-1,-1,0};
    void dfs(vector<vector<char>> &grid,int x,int y)
    {
        if(x<0||x==grid.size()||y<0||y==grid[0].size()||grid[x][y]=='0') return;
        grid[x][y]='0';
        for(int i=0;i<4;i++)
        {
           int xx=x+dir[i][0];
           int yy=y+dir[i][1];
           dfs(grid,xx,yy);
        }
        return;
    }
    int numIslands(vector<vector<char>>& grid) {
        int ans=0;
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0;j<grid[0].size();j++)
            {
                if(grid[i][j]=='1')
                {
                    ans++;
                    dfs(grid,i,j);
                } 
            }
        }
        return ans;
    }

};
```

#### [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

解题思路：

> 对于一个陆地格子的每条边，它被算作岛屿的周长当且仅当这条边为**网格的边界或者相邻的另一个格子为水域**。 因此，我们可以遍历每个陆地格子，看其四个方向是否为边界或者水域，如果是，将这条边的贡献（即 1）加入答案 ans 中即可。
>

```c++
class Solution {
public:
	//方向数组
    int dir[4][2]={0,1,1,0,0,-1,-1,0};

    int dfs(vector<vector<int>>& grid,int x,int y){
       if(x<0||y<0||x==grid.size()||y==grid[0].size()||grid[x][y]==0){
           return 1;
       }
       int res=0;
       if(grid[x][y]==2) {
           return 0;
       }   
        //标记走过的地方
       grid[x][y]=2;  
       for(int i=0;i<4;i++){
           int xx=x+dir[i][0];
           int yy=y+dir[i][1];
           res+=dfs(grid,xx,yy);
       }
       return res;
    }

    int islandPerimeter(vector<vector<int>>& grid) {
        int ans=0;
         for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==1){
                   // grid[i][j]=2;
                    ans+=dfs(grid,i,j);
                }
            }
        }
        return ans;

    }
};
```

#### [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

> 给定一个包含了一些 0 和 1 的非空二维数组 grid 。
>
> 一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。
>
> 找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

```c++
class Solution {
    
public:
    int dir[4][2]={0,1,1,0,0,-1,-1,0};
    int temp=0;
    void dfs(vector<vector<int>> &grid,int x,int y)
    {
       for(int i=0;i<4;i++){
           int xx=x+dir[i][0];
           int yy=y+dir[i][1];
           if(xx<0||yy<0||xx==grid.size()||yy==grid[0].size()||grid[xx][yy]==0){
               continue;
           }
           if(grid[xx][yy]==1){
               grid[xx][yy]=0;
               temp++;
               dfs(grid,xx,yy);
           }
       }
       return ;
    }

    int maxAreaOfIsland(vector<vector<int>>& grid) {
         int ans=0;
        for(int i=0;i<grid.size();i++)
        {
            for(int j=0;j<grid[0].size();j++)
            {
                if(grid[i][j]==1)
                {
                    grid[i][j]=0;
                    temp=1;
                    dfs(grid,i,j);
                    ans=max(ans,temp);

                } 
            }
        }
        return ans;


    }
};
```



## BFS(广搜)

#### 广搜走地图

```c++
//广度优先搜索，解决地图问题，并且可以直接求出最小步数。
#include<iostream>
#include<queue>
using namespace std;
struct node{
	int x,y,step;
};
int n,m,sx,sy;
int dir[4][2]={0,1,1,0,0,-1,-1,0};
char mmap[105][105];

int main(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
            if(mmap[i][j]=='S'){
                sx=i,sy=j;
            }
        }
    }
    queue<node>que;
    que.push((node){sx,sy,0});
    while(!que.empty()){
        node temp=que.front();
        que.pop();
        for(int i=0;i<4;i++){
            int x=temp.x+dir[i][0];
            int y=temp.y+dir[i][1];
            if(mmap[x][y]=='T'){
                cout<<temp.step+1<<endl;
                return 0;
            }
            if(mmap[x][y]=='.'){
                mmap[x][y]=0;
                que.push((node){x,y,temp.step+1});
            }
        }
    }
    cout<<"NO"<<endl;
	return 0;
}
```



```c++
//oj402.cpp
#include<iostream>
using namespace std;
#include<iostream>
#include<queue>
struct node{
    int now,step;
};

int n,a,b,num[205],check[205];

int main(){
    cin>>n>>a>>b;
    for(int i=1;i<=n;i++){
        cin>>num[i];
    }
    queue<node>que;
    que.push((node){a,0});
    check[a]=1;
    while(!que.empty()){
        node temp=que.front();
        que.pop();
        int up=temp.now+num[temp.now];
        int down=temp.now-num[temp.now];
        if(up==b||down==b){
            cout<<temp.step+1<<endl;
            return 0;
        }
        if(up<=n&&check[up]==0){
            que.push((node){up,temp.step+1});
            check[up]=1;
        }
        if(down>=1&&check[down]==0){
            que.push((node){down,temp.step+1});
            check[down]=1;
        }
    }
    cout<<-1<<endl;
    return 0;

}

```

![image-20201206155712117](E:\Study_Notes\图片\面视算法\小明回家)

```c++
//存储状态、起始状态、终止状态、状态转移、去重
#include<iostream>
#include<queue>
using namespace std;

struct node{
    int x,y,step,f;
    
};
int n,m,check[2005][2005];
char mmap[2005][2005];
int dir[4][2]={0,1,1,0,0,-1,-1,0};

int main(){
    cin>>n>>m;
    queue<node>que;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
            if(mmap[i][j]=='S'){
                que.push((node){i,j,0,0});
                check[i][j]=1;
            }
        }
    }
    while(!que.empty()){
        node temp=que.front();
        que.pop();
        for(int i=0;i<4;i++){
            int x=temp.x+dir[i][0];
            int y=temp.y+dir[i][1];
            if((check[x][y]&1)&&temp.f==0){
                continue;
            }
            if((check[x][y]&2)&&temp.f==1){
                continue;
            }
            if(mmap[x][y]=='T'&&temp.f==1){
                cout<<temp.step+1<<endl;
                return 0;
            }
            if(mmap[x][y]=='.'||mmap[x][y]=='S'||mmap[x][y]=='T'){
                que.push((node){x,y,temp.step+1,temp.f});
                check[x][y]+=temp.f+1;
            }
            if(mmap[x][y]=='P'){
                que.push((node){x,y,temp.step+1,1});
                check[x][y]=3;
            }
        }
    }
    cout<<-1<<endl;
    return 0;
}

```



![image-20201206160913021](E:\Study_Notes\图片\面视算法\关系网络)

```c++
//oj528.cpp
#include<iostream>
#include<queue>
using namespace std;

struct node{
    int now,step;
};

int n,x,y,check[105],arr[105][105];

int main(){
    cin>>n>>x>>y;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            cin>>arr[i][j];
        }
    }
    queue<node>que;
    que.push((node){x,0});
    check[x]=1;
    while(!que.empty()){
        node temp=que.front();
        que.pop();
        for(int i=1;i<=n;i++){
            if(arr[temp.now][i]==1&&check[i]==0){
                if(i==y){
                    cout<<temp.step<<endl;
                    return 0;
                }
                que.push((node){i,temp.step+1});
                check[i]=1;
            }
        }
    }
    cout<<0<<endl;
    return 0;
}
```



```c++
//层序遍历、广搜、
#include<iostream>
#include<queue>
#include<string>
using namespace std;

struct node{
    int x,y;
};

int n,m,cnt;
int dir[4][2]={-1,0,1,0,0,-1,0,1};
char mmap[55][55];

int main(){
    queue<node>que;
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            cin>>mmap[i][j];
            if(mmap[i][j]=='*'){
                que.push((node){i,j});
                mmap[i][j]='.';
            }
        }
    }
    cin>>cnt;
    while(cnt--){
        string str;
        cin>>str;
        int dir_num,check[55][55]={0},qsize=que.size();
        if(str=="NORTH"){
            dir_num=0;
        }else if(str=="SOUTH"){
            dir_num=1;
        }else if(str=="WEST"){
            dir_num=2;
        }else{
            dir_num=3;
        }
        while(qsize--){
            node temp=que.front();
            que.pop();
            for(int i=1;1;i++){
                int x=temp.x+dir[dir_num][0]*i;
                int y=temp.y+dir[dir_num][1]*i;
                if(mmap[x][y]!='.'){
                    break;
                }
                if(check[x][y]==0){
                    que.push((node){x,y});
                    check[x][y]=1;
                }
            }
        }
    }
    while(!que.empty()){
        node temp=que.front();
        que.pop();
        mmap[temp.x][temp.y]='*';
    }
    for(int i=1;i<=n;i++){
        cout<<&mmap[i][1]<<endl;
    }
    return 0;
}
```

## 算法下

$ans[j][k]$表示从点$j$到点$k$的最短路径

邻接矩阵 时间复杂度$O(n^3)$  

Floyd算法：求多源最短路径，可以判断任意两点是否连接；

```cpp
for(int i=1;i<=n;i++){
    for(int j=1;j<=n;j++){
        for(int k=1;k<=n;k++){
            ans[j][k]=min(ans[j][k],ans[j][i]+ans[i][k])
        }
    }
}
```

求任意点到任一点之间的最短路径；

![image-20210319150658616](E:\Study_Notes\图片\最短路径)

```cpp
#include<iostream>
#include<cstring>
using namespace std;
int n,m,s,arr[105][105];


int main(){
    memset(arr,0x3f,sizeof(arr));
    cin>>n>>m>>s;//n行m列的邻接矩阵，起点为s:
    for(int i=1;i<=n;i++){
        arr[i][i]=0;
    }
    for(int i=0;i<m;i++){
        int s1,e,v;//两个点以及对应的权值
        cin>>s1>>e>>v;
        arr[s1][e]=min(v,arr[s1][e]);
        arr[e][s1]=min(arr[s1][e],v);
    }
    //求点j到k的最短路径;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            for(int k=1;k<=n;k++){
                arr[j][k]=min(arr[j][k],arr[j][i]+arr[i][k]);
            }
        }
    }

    for(int i=1;i<=n;i++){
        if(arr[s][i]!=0x3f3f3f3f){
                cout<<arr[s][i]<<" ";
            }else{
                cout<<-1<<" ";
            }
        cout<<endl;
    }
    return 0;
}



```

邻接表：

![image-20210319151228007](E:\Study_Notes\图片\邻接表)

实现细节：

链表、vector

权值：（终点和权值，e,v;pair<e,v>)

重边：存进去，遍历有重复不考虑；

```cpp
#include<iostream>
#include<vector>
#include<utility>

using namespace std;
int n,m;
int main(){
    cin>>n>>m;//n个点，m条边；
    //初始化动态二维数组：
    vector<vector<pair<int,int>> >edg(n+1,vector<pair<int,int>>{});
    for(int i=0;i<m;i++){
        int s,e,v;
        cin>>s>>e>>v;
        edg[s].push_back(make_pair(e,v));
    }
    for(int i=1;i<=n;i++){
        cout<<i<<" :";
        for(int j=0;j<edg[i].size();j++){
            cout<<"{"<<edg[i][j].first<<","<<edg[i][j].second<<"}";
        }
        cout<<endl;
    }
    return 0;
}

```

链式前向星：

边：起点、终点、权值(s,e,v)；

```cpp
struct edge{
	int e,v,next;//
};
edge[100005];
int head[100005];//点，初始化为-1;
head[i];以i点为起点的最后一条边的编号；
edge[i].next----->和编号为j的边角
```

## RMP区间最值

解决问题：不带修改区间最大最小问题；

特点：重叠不会对区间最大值产生影响；例如知道1到8的最值，和2到10的最值，求1到10的最值；

> 从n开始向后数m个数，停下的数是n+m-1;
>
> 从n开始往前数m个数，停下的数是n-m+1；

原理：ST表（稀疏表）



## 容器的使用

### deque

```cpp
deque.push_back(elem);   //在容器尾部添加一个数据
deque.push_front(elem);  //在容器头部插入一个数据
deque.pop_back();                 //删除容器最后一个数据
deque.pop_front();            //删除容器第一个数据
deque.at(idx);  //返回索引idx所指的数据，如果idx越界，抛出out_of_range。
deque[idx];  //返回索引idx所指的数据，如果idx越界，不抛出异常，直接出错。
deque.front();   //返回第一个数据。
deque.back();  //返回最后一个数据
deque.begin();  //返回容器中第一个元素的迭代器。
deque.end();  //返回容器中最后一个元素之后的迭代器。
deque.rbegin();  //返回容器中倒数第一个元素的迭代器。
deque.rend();   //返回容器中倒数最后一个元素之后的迭代器。
```



### queue

==随机迭代器==，不支持[]访问；

```c++
queue<int>que;
que.push(5);
que.pop();//出队
que.front();//队首
que.size();
que.empty();
//自定义类型;struct node{int x;int y;};
queue<node>qu;

push(x) 将x压入队列的末端
pop() 弹出队列的第一个元素(队顶元素)，注意此函数并不返回任何值
front() 返回第一个元素(队顶元素)
back() 返回最后被压入的元素(队尾元素)
empty() 当队列为空时，返回true
size() 返回队列的长度
```

### stack

```c++
stack<int> sta;
sta.push(s);
sta.pop();
sta.top();//栈顶元素
sta.size();
sta.empty();
//queue和stack的底层实现是双端队列deque
```

### vector

随机迭代访问其，不支持[]访问;

```c++
vector<int>v;
v.push_back(5);
v.size();
v.insert(1,6);//插入元素，时间复杂度为o(n);
vector<vetor<int> >
//底层实现为一片连续的存储空间，数组
```

### priority_queue

随机迭代访问器，不支持[]访问，一般求最小（最大）的前K或者第K个数可以用；

```c++
//优先队列 ---堆,排序重载运算符小于号
priority_queue<int>que;
que.push(4);
que.pop();
que.top();
que.size();
que.empty();
//排序要重载小于号

//升序队列，小顶堆
priority_queue <int,vector<int>,greater<int> > q;
//降序队列，大顶堆
priority_queue <int,vector<int>,less<int> >q;
//greater和less是std实现的两个仿函数（就是使一个类的使用看上去像一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了）
```

```cpp
//求最小的K个数，
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        priority_queue<int,vector<int>,less<int>>prique;
        vector<int>res;
        if(arr.size()==0||k==0) return res;
        //维护一个容量大小为K的大顶堆，如果后面的数字比堆顶元素小，堆顶元素弹出，新元素入堆；
        for(int i=0;i<k;i++){
            prique.push(arr[i]);
        }
        for(int i=k;i<arr.size();i++){
            if(arr[i]<prique.top()){
                prique.pop();
                prique.push(arr[i]);
            }
        }
        for(int i=0;i<k;i++){
            res.push_back(prique.top());
            prique.pop();
        }
        return res;
    }
};
```

### map

```c++
//键值对无重复数字
map<string,int>m;
m["123"]=456;//m[key]=value;
insert,
find,
erase;
count;
//底层实现:红黑树，查找效率为O(logN);
mutimap;//可以有重复得元素
set;//集合，底层是红黑树，去重排序，multiset,,--->unordered_set;
pair<int,int> ;//.first() ,.second();
```

### unordered_map

map、set底层实现是**==红黑树==**；unordered_map和unordered_set底层实现是**==哈希表==**；

map自动排序，unordered_map没有自动排序；

```cpp
/*unordered_map和map类似，都是存储的key-value的值，可以通过key快速索引到value。
不同的是unordered_map不会根据key的大小进行排序.
unordered_map内部元素是无序的，而map中的元素是按照二叉搜索树存储，进行中序遍历会得到有序遍历.

```

```cpp
//查找元素是否存在unordered_map<int,int>map是否存在x;
map.find(x)!=map.end() or map.count(x)!=0
//插入数据
map.insert(Map::value_type(1,"Raoul"));
//遍历
unordered_map<key,T>::iterator iter;
(*iter).fisrt;//the key value;
(*iter).second;//the mapped value;
for(unordered_map<key,T>::iterator iter=mp.begin();iter!=mp.end();iter++){
    cout<<"the key value is"<<iter->first;
    cout<<"the mapped value is "<<iter->second;
}
```



## 单调队列、单调栈

### 单调队列

解决区间内最小或最大值问题；

1. 本质问题是：固定查询结尾的 RMQ 问题，例如 $RMQ(x, 7)$
2. 问题性质：维护滑动窗口最值问题
3. 入队：将队尾违反单调性的元素淘汰出局，再将当前元素入队
4. 出队：如果队首元素超出了滑动窗口的范围，队首出队
5. 队首元素：滑动窗口内的最值
6. 均摊时间复杂度：$O(1)$





## 常用解题函数及技巧

### 库函数

#### lower_bound&&upper_bound

```cpp
lower_bound( )和upper_bound( )都是利用二分查找的方法在一个排好序的数组中进行查找的。

在从小到大的排序数组中，
lower_bound( begin,end,num)：从数组的begin位置到end-1位置二分查找第一个大于或等于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标。
upper_bound( begin,end,num)：从数组的begin位置到end-1位置二分查找第一个大于num的数字，找到返回该数字的地址，不存在则返回end。通过返回的地址减去起始地址begin,得到找到数字在数组中的下标

lower_bound(起始地址，结束地址，要查找的数值) 返回的是数值 第一个 出现的位置。
upper_bound(起始地址，结束地址，要查找的数值) 返回的是数值 最后一个 出现的位置。

#include<iostream>
#include<algorithm>
using namespace std;

int main(){
    int num[8]={1,2,2,2,3,4,5,6};
    int pos1=upper_bound(num,num+7,2)-num;
    int pos2=lower_bound(num,num+7,2)-num;
    cout<<pos1<<" "<<pos2;
    return 0;
}
//
pos1=4;
pos2=1;


#include<iostream>
#include<algorithm>
#include<vector>
using namespace std;
int main(){
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(2);
    v.push_back(2);
    v.push_back(4);
    v.push_back(5);
    auto it=lower_bound(v.begin(),v.end(),2);
    v.insert(it,3);
    for(int i=0;i<v.size();i++){
        cout<<v[i]<<endl;
    }
    return 0;
}
//输出结果为:
1
3
2
2
2
4
5
```

### C++11新特性之std::advance函数

将某个迭代器前进到指定的位置上，例如：

```cpp
// advance example
#include <iostream>     // std::cout
#include <iterator>     // std::advance
#include <list>         // std::list

int main () {
  std::list<int> mylist;
  for (int i=0; i<10; i++) mylist.push_back (i*10);
  std::list<int>::iterator it = mylist.begin();
  std::advance (it,5);
  std::cout << "The sixth element in mylist is: " << *it << '\n';
  return 0;
}
//0 10 20 30 40 50 
//The sixth element in mylist is: 50
```

### 输入输出

##### 字符串与数字的转换

```cpp
string to_string(int value)//数字向字符串转换
0的ASCII码值为48，
//字符---->数字 :减去'0';
字符6变为数字6: int res= '6'-'0';//res=6;
//数字---->字符 :加上'0'
数字6变为字符6:char res=6+'0';

atoi() 和stoi()函数：
int atoi(const char *str)
stoi函数：　将 n 进制的字符串转化为十进制
    
 1 #include <iostream>
 2 #include <string>
 3 
 4 using namespace std;
 5 
 6 int main()
 7 {
 8     string str = "1010";
 9     int a = stoi(str, 0, 2);
10     cout << a << endl;
11         return 0;
12 }  
上式输出:10：
相同点：
①都是C++的字符处理函数，把数字字符串转换成int输出
②头文件都是#include<cstring>
不同点：
①atoi()的参数是 const char* ,因此对于一个字符串str我们必须调用 c_str()的方法把这个string转换成 const char*类型的,而stoi()的参数是const string*,不需要转化为 const char*；
```



```cpp
//cin自动以空格结束
//例如：输入i am ok;输出iao;
int main(){
    string s;
    while(cin>>s){
        cout<<s[0];
    }
    cout<<endl;
    return 0
}
int main(){
    string s;
    getline(cin,s);
    string res="";
    res=s[0];
    for(int i=1;i<s.size();i++){
        //这里空格只能用单引号加上空格表示；
        if(s[i-1]==' '){
            res+=s[i];
        }
    }
    cout<<res<<endl;
}



```

### unordered_set

```cpp
/*1.头文件：#include<unordered_set>

2.功能：可以用来去重，但是不排序。

　　　　速度比set要快很多（底层是散列）

3.使用前提：编译器要是c++11标准的
4.包括一个函数 v.count(i)  意思是：v容器中是否有一个元素跟i的值相同 若有，返回1；若无，则返回0

unordered_set::insert()是C++ STL中的内置函数，用于在unordered_set容器中插入新的{element}。仅当每个元素与容器中已经存在的任何其他元素不相等时才插入每个元素(unordered_set中的元素具有唯一值)
```

```cpp
/*字符串可以当作栈来使用；
string res;
res.back()是末尾的元素；
res.pop_back();删除末尾的元素；

```



