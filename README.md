# Leetcode
This repository inspired me to develop a good habit of writing questions


# 剑指 offer 记录

## 剑指offer 09
## 用栈实现队列
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead, 分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )<br>
输入输出实例：
```
    输入：
        ["CQueue","appendTail","deleteHead","deleteHead"]
        [[],[3],[],[]]
    输出：[null,null,3,-1]
```
对于数列中各个项对应的是各个方法的使用的输入，第一个列表对于方法名，第二个列表对应放发需要输入的参数，最后是输出值。

 ``` c++
 class CQueue {
public:
    CQueue() {
    }
    
    void appendTail(int value) {
        s1.push(value);
        //s2.push(value);
    }
    
    int deleteHead() {
        int head;
        if(s1.size() <= 0)
            return -1;
        else{
            //将用于存储的栈内顺序进行倒置，用于删除栈底元素
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();   
            }
            //栈底弹出
            head = s2.top();
            s2.pop();
            //将栈进行还原
            while(!s2.empty()){
                s1.push(s2.top());
                s2.pop();
            }
            return head;
        }

    }
private:
    //建立需要进行使用的双栈，实现要求。
    stack<int>  s1;
    stack<int>  s2;
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

## 剑指offer 30
## 包含min函数的栈
定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
输入输出实例：
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```
本题目的重点是如何实现对于栈找最小元素的复杂度O(0)的实现。
解题思路：
![solve_way](https://github.com/SEU-PZH/Leetcode/blob/main/img/day1.png)
```c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> s1;
    stack<int> s2;
    MinStack() {
    }
    
    void push(int x) {
        if(s1.empty()){
            s2.push(x);
        }
        else if(s2.top() >= x){
            //辅助栈的建立，不需严格递增
            s2.push(x);
        }
        s1.push(x);
    }
    
    void pop() {
        int head;
        head = s1.top();
        //保持元素一致性
        if(head == s2.top()){
            s2.pop();
        }
        s1.pop();
    }
    
    int top() {
        return s1.top();
    }
    
    int min() {
        return s2.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```


## 剑指 Offer 06
## 从尾到头打印链表
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
```
输入：head = [1,3,2]
输出：[2,3,1]
```
### 方法一：栈，通过栈的的方式，每次弹出栈中的顶层元素，实现反转。
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 #include <iostream>
 using namespace std;
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        stack<int> s1;
        while(head != NULL){
            s1.push(head->val);
            head = head->next;
        }
        vector<int> v1;
        while(!s1.empty()){
            v1.push_back(s1.top());
            s1.pop();
        }
        return v1;
        
    }
};
```
### 方法二，递归方式，对于每次递归，在归的过程之对于最后的元素放入数组中。
```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        if(!head)
            return {};
        vector<int> a=reversePrint(head->next);
        a.push_back(head->val);
        return a;
    }
};
```

## 剑指 Offer 24
## 反转链表 
定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```
```c++
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
        stack<int> s1;
        ListNode* node = head;
        while(node!=NULL){
            s1.push(node->val);
            node = node->next;
        }
        ListNode* temp = head;
        while(temp!=NULL){
            temp->val = s1.top();
            s1.pop();
            temp = temp->next;
        }
        return head;
        
    }
};
```
### 方法一：迭代
假设链表为1→2→3→∅，我们想要把它改成 3∅←1←2←3。

在遍历链表时，将当前节点的指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用
```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```
### 方法二：递归<br>
![solve_way](https://github.com/SEU-PZH/Leetcode/blob/main/img/day2.png)
```c++
//newHead指向的永远是反转后的头节点，而head可以看成是针对于每次递归过程中，归的过程中对于链表进行反转过程。
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return newHead;
    }
};
```

## 剑指 Offer 35
## 复杂链表的复制
请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

![day2_2](https://user-images.githubusercontent.com/97490701/149089353-238efac1-cc26-41bc-90b3-a8b7deec3c33.png)
### 方法一：回溯 + 哈希表
这道题的难点，是建立一种你深拷贝的方式，对于每一个新的指向节点，都要对于其建立新的空间，方法一的方式是对于原本的节点使用MAP对应节点和新开辟节点之间的对应关系，最后进行相互指针的指引。
```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
//#include <iostream>
//using namespace std;
class Solution {
public:
    Node* copyRandomList(Node* head) {
        map<Node*, Node*> m1;
        Node* temp = head;
        while(head!=NULL){
            if(m1[head] == NULL){
                Node* copyhead = new Node(head->val);
                m1[head] = copyhead;
                head = head->next;
                //copyhead = copyhead->next;
            }
            
        }
        head = temp;
        Node* res = m1[head];
        while(head!=NULL){
            m1[head]->random = m1[head->random];
            m1[head]->next = m1[head->next];
            head = head->next;
        }
        return res;
    }
};
```
### 方法二：迭代 + 节点拆分
注意到方法一需要使用哈希表记录每一个节点对应新节点的创建情况，而我们可以使用一个小技巧来省去哈希表的空间。我们首先将该链表中每一个节点拆分为两个相连的节点，例如对于链表 A→B→C，我们可以将其拆分为A→A→B→B→C→C。对于任意一个原节点 SS，其拷贝节点 S'S′  即为其后继节点。这样，我们可以直接找到每一个拷贝节点 S'S 的随机指针应当指向的节点，即为其原节点 SS 的随机指针指向的节点 TT 的后继节点 T'T。需要注意原节点的随机指针可能为空，我们需要特别判断这种情况。

当我们完成了拷贝节点的随机指针的赋值，我们只需要将这个链表按照原节点与拷贝节点的种类进行拆分即可，只需要遍历一次。同样需要注意最后一个拷贝节点的后继节点为空，我们需要特别判断这种情况

![day2_3](https://user-images.githubusercontent.com/97490701/149090529-717ac349-f040-4d4c-9b70-c9ef3a3b9e4a.png)


```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == nullptr) {
            return nullptr;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = new Node(node->val);
            nodeNew->next = node->next;
            node->next = nodeNew;
        }
        for (Node* node = head; node != nullptr; node = node->next->next) {
            Node* nodeNew = node->next;
            nodeNew->random = (node->random != nullptr) ? node->random->next : nullptr;
        }
        Node* headNew = head->next;
        for (Node* node = head; node != nullptr; node = node->next) {
            Node* nodeNew = node->next;
            node->next = node->next->next;
            nodeNew->next = (nodeNew->next != nullptr) ? nodeNew->next->next : nullptr;
        }
        return headNew;
    }
};
```


## 剑指 offer 05
## 替换空格
请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
```
输入：s = "We are happy."
输出："We%20are%20happy."
```
```c++
class Solution {
public:
    string replaceSpace(string s) {
        for(int i=0;i<s.length();i++){
            if(s[i]==' '){
                s.erase(i,1);
                s.insert(i,"%20");
            } 
        }
        return s;
    }
};
```

 
## 剑指 offer 58 
## Ⅱ左旋字符串
字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。
示例 1：
```
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```
示例 2：
```
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
```
```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        for(int i=0;i<n;i++){
            s.push_back(s[i]);
        }
        s.erase(0,n);
        return s;
    }
};
```

## 剑指 Offer 03
## 数组中重复的数字
找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```
```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        map<int, int> m1;
        for(int i=0; i<nums.size(); i++){
            m1[nums[i]]++;
            if(m1[nums[i]]>1){
                return nums[i];
            }
        }
        return 0;
    }
};
```

## 剑指 Offer 53  
## I. 在排序数组中查找数字 I
统计一个数字在排序数组中出现的次数。
```
输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
```
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int a = 0;
        for(int i=0;i<nums.size();i++){
            if(nums[i] == target){
                a++;
            }
        }
        return a;
    }
};
```
二分查找
![day3](https://user-images.githubusercontent.com/97490701/149472948-eb27b74f-14bb-43e0-9c07-9c0b97470e2d.png)

```c++
class Solution {
public:
    int binarySearch(vector<int>& nums, int target, bool lower) {
        int left = 0, right = (int)nums.size() - 1, ans = (int)nums.size();
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
                ans = mid;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }

    int search(vector<int>& nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false) - 1;
        if (leftIdx <= rightIdx && rightIdx < nums.size() && nums[leftIdx] == target && nums[rightIdx] == target) {
            return rightIdx - leftIdx + 1;
        }
        return 0;
    }
};
```

## 剑指 Offer 53 
## II. 0～n-1中缺失的数字
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
```
输入: [0,1,3]
输出: 2
```
二分查找
```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left =0;
        int right = nums.size();
        int mid;
        while(left<=right){
            mid = (left+right)/2;
            if(nums[mid] != mid){
                right = mid;
            }
            if(nums[mid] == mid){
                left = mid;
            }
            if(right - left <= 1){
                if(nums[left] == left)
                    return left+1;
                else
                    return left;
            }
        }
        return -1;
    }
};
```


## 剑指 Offer 04
## 二维数组中的查找
在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
```
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。
```
本来想得是通过两个二分查找，相夹找到范围，后来发现后几个实例过不去，发现对于二分查找找上下界，只能够用来确定他的上界，无法确定下界。
```c++
//#include <iostream>
//using namespace std;
class Solution {
public:
    int findcol(vector<vector<int>>& matrix, int target, bool flag) {
            int down, up=0;
            down = matrix.size()-1;//5
            //cout<<up;
            int mid_row;
            while((down-up)>1){
                mid_row = (down+up)/2;
                if(matrix[mid_row][0]>target || (flag == true && matrix[mid_row][0] == target)){
                    down = mid_row;
                }
                else{
                    up = mid_row;
                }
                if(abs(matrix[mid_row][0]-target)<=(matrix[0].size()-1))
                    break;
            }
            if(flag)
                return up;
            else
                return down;
    }

    int findrow(vector<vector<int>>& matrix, int target, bool flag) {
            int down, up=0;
            down = matrix[0].size()-1;
            int mid_row;
            while((down-up)>1){
                mid_row = (down+up)/2;
                if(matrix[0][mid_row]>target || (flag == true && matrix[0][mid_row] == target)){
                    down = mid_row;
                }
                else{
                    up = mid_row;
                }
                if((matrix[0][mid_row]-target)<=(matrix[0].size()-1))
                    break;
            }
            if(flag)
                return down;
            else
                return up;
    }

    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {

        int down_fin,up_fin;
        int left_fin,right_fin;
        if(matrix.size()==0){
            return false;
        }
        up_fin = findcol(matrix, target, true);
        down_fin = findcol(matrix, target, false);
        left_fin = findrow(matrix, target, false);
        right_fin = findrow(matrix, target, true);

        //cout<<down_fin<<up_fin<<left_fin<<right_fin;
        
        //if(matrix[][])

        for(int i=0;i<=down_fin;i++){
            for(int j = 0;j<matrix[0].size(); j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        for(int i=0;i<matrix.size();i++){
            for(int j = 0;j<=right_fin; j++){
                if(matrix[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
};
```

### 方法一：暴力
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int rows = matrix.length, columns = matrix[0].length;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (matrix[i][j] == target) {
                    return true;
                }
            }
        }
        return false;
    }
}

```
### 方法二：
先找行，再找列。<br>
若 flag > target ，则 target 一定在 flag 所在 行的上方 ，即 flag 所在行可被消去。<br>
若 flag < target ，则 target 一定在 flag 所在 列的右方 ，即 flag 所在列可被消去。<br>
![day4](https://user-images.githubusercontent.com/97490701/149607792-399dcbaa-aaed-4e6d-8b27-76b8af237522.png)

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int i = matrix.size() - 1, j = 0;
        while(i >= 0 && j < matrix[0].size())
        {
            if(matrix[i][j] > target) i--;
            else if(matrix[i][j] < target) j++;
            else return true;
        }
        return false;
    }
};
```

## 剑指 Offer 11
## 旋转数组的最小数字
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。

给你一个可能存在 重复 元素值的数组 numbers ，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一次旋转，该数组的最小值为1。  

```
输入：[3,4,5,1,2]
输出：1
```
### 方法一：暴力
```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if(numbers.size()==1)
            return numbers[0];
        for(int i=1;i<numbers.size();i++){
            if(numbers[i] - numbers[i-1]<0)
                return numbers[i];
        }
        return numbers[0];
    }
};
```
### 方法二：二分查找

![day4_2](https://user-images.githubusercontent.com/97490701/149612609-cdf89b30-280c-43b1-9c5f-42c47d7558dc.png)

```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int low = 0;
        int high = numbers.size() - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (numbers[pivot] < numbers[high]) {
                high = pivot;
            }
            else if (numbers[pivot] > numbers[high]) {
                low = pivot + 1;
            }
            else {
                high -= 1;
            }
        }
        return numbers[low];
    }
};
```

## 剑指 Offer 50
## 第一个只出现一次的字符
在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
```
输入：s = "abaccdeff"
输出：'b'
```
### 方法一：使用哈希表存储频数
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        map<char, int> m1;
        if(s.length() == 0){
            return ' ';
        }
        for(int i=0; i<s.length();i++){
            m1[s[i]]++;
        }
        for(int i=0; i<s.length();i++){
            if(m1[s[i]] == 1)
                return s[i];
        }
        return ' ';
    }
};
```
### 方法二：使用哈希表存储索引
```c++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char, int> position;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (position.count(s[i])) {
                position[s[i]] = -1;
            }
            else {
                position[s[i]] = i;
            }
        }
        int first = n;
        for (auto [_, pos]: position) {
            if (pos != -1 && pos < first) {
                first = pos;
            }
        }
        return first == n ? ' ' : s[first];
    }
};
```
### 方法三：队列
思路与算法<br>

我们也可以借助队列找到第一个不重复的字符。队列具有「先进先出」的性质，因此很适合用来找出第一个满足某个条件的元素。<br>

具体地，我们使用与方法二相同的哈希映射，并且使用一个额外的队列，按照顺序存储每一个字符以及它们第一次出现的位置。当我们对字符串进行遍历时，设当前遍历到的字符为 cc，如果 cc 不在哈希映射中，我们就将 cc 与它的索引作为一个二元组放入队尾，否则我们就需要检查队列中的元素是否都满足「只出现一次」的要求，即我们不断地根据哈希映射中存储的值（是否为 -1−1）选择弹出队首的元素，直到队首元素「真的」只出现了一次或者队列为空。<br>

在遍历完成后，如果队列为空，说明没有不重复的字符，返回空格，否则队首的元素即为第一个不重复的字符以及其索引的二元组。<br>

小贴士<br>

在维护队列时，我们使用了「延迟删除」这一技巧。也就是说，即使队列中有一些字符出现了超过一次，但它只要不位于队首，那么就不会对答案造成影响，我们也就可以不用去删除它。只有当它前面的所有字符被移出队列，它成为队首时，我们才需要将它移除。<br>

```c++
class Solution {
public:
    char firstUniqChar(string s) {
        unordered_map<char, int> position;
        queue<pair<char, int>> q;
        int n = s.size();
        for (int i = 0; i < n; ++i) {
            if (!position.count(s[i])) {
                position[s[i]] = i;
                q.emplace(s[i], i);
            }
            else {
                position[s[i]] = -1;
                while (!q.empty() && position[q.front().first] == -1) {
                    q.pop();
                }
            }
        }
        return q.empty() ? ' ' : q.front().first;
    }
};
```
