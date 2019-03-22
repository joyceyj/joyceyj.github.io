---
layout: post
title: longest substring without repeating charaters
date: 2019-03-22
categories: C_language
tag: C++ leetcode
---
# Longest substring without repeating charaters无重复字符的最长子串

## 题目
> Given a string, find the length of the longest substring without repeating characters.

> Example 1:

> Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
Example 2:

> Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

> Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
             
## 题目理解

给定字符串，找到没有重复的字符的子串，返回符合的子串的最大长度

## 解题思路1

我首先想到的就是暴力求解：

	1.求子串：遍历字符串，对每个字符作为子串的头，用substr（start，length）得到子串，
	如果某个子串中出现重复字符，则从下一个字符开始作为子串的头；	
	2.判断子串是否有重复字符：遍历子串，O(n^2)，这一步可以使用stl容器map或set降低时间复杂度；
	3.比较子串长度。
	
## 代码

```C++
bool repeat(string s) {
    int len = s.size();
    if (len <= 1) return false;
    for (int i = 0; i < len; i++) {
        for (int j = i+1; j < len; j++) {
            if (s[i] == s[j]) return true;
        }
    }
    return false;
}
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int len = s.size();
        // cout << len;
        if (len < 1) return 0;
        int count = 1;
        for (int i = 0; i < len; i++) {
            for (int j = 1; j <= len-i; j++) {
                string substr = s.substr(i, j);
                // cout << "i=" << i << substr << endl;
                if (!repeat(substr)) {
                    // cout << j << endl;
                    if (count < j) count = j;
                } else {
                    break;
                }
            }
        }
        return count;
    }
};
```

## 改进：使用STL容器

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int len = s.size();
        // cout << len;
        if (len < 1) return 0;
        int count = 1;
        
        set<char> unique;
        for (int i = 0; i < len; i++) {
            unique.clear();
            unique.insert(s[i]);
            
            for (int j = i+1; j < len; j++) {
                if (unique.find(s[j]) != unique.end()) {
                    break;
                } else {
                    unique.insert(s[j]);
                    // cout << "j=" << j << " " << count << endl;
                    count = count > j-i+1 ? count : j-i+1;
                }
                
            }
            // set<char>::iterator it = unique.begin();
            // for (; it != unique.end(); it++) {
            //     cout << *it << " ";
            // }
            // cout << endl;
        }
        return count;
    }
};
```

## 解题思路2

建立hash map，用数组记录ascii码表中字符在字符串中最近一次出现的位置，在字符出现的两次位置下标相减就得到了子字符串的长度

## 代码

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
            // ASCII码表索引，记录对应字符char最近一次出现的位置下标
            int indices[256];
            memset(indices,-1,sizeof(indices));
        
            int strLength = s.size();
            int maxLength = -1; // 已记录的最大长度
            int minindex = 0;   // 上一次搜索无重复的最小下标
            for(int i=0 ; i<strLength; i++){
            		// int(s[i])表示字符在ascii表中的序号
                // cout << indices[int(s[i])] << " " << minindex << endl;
                minindex = max( indices[ int(s[i]) ]+1 , minindex);
                maxLength = max(maxLength, i - minindex);
                // cout << maxLength << endl;
                indices[int(s[i])]=i;
            }
            return maxLength+1;
    }
};
```


## 时间复杂度

||时间复杂度|leetcode|note|
|----|----|----|----|
|思路1.1|O(n^4)|986 / 987 test cases passed|当字符串长时会发生超时|
|思路1.2|O(n^2)| Runtime: 1164 ms; Memory:271.7 MB|
|思路2|O(n)|Runtime: 20 ms; Memory:14.5 MB|




