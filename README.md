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
```
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
方法一：栈，通过栈的的方式，每次弹出栈中的顶层元素，实现反转。
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
方法二，递归方式，对于每次递归，在归的过程之对于最后的元素放入数组中。
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
方法一：迭代
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
方法二：递归
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




