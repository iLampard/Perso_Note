# String algorithms


### 字符串反转

#### Reverse String, [LeetCode344](https://leetcode.com/problems/reverse-string/)

```c
string reverseString(string s) {
    if(s.empty())
        return "";
    int end = s.size() - 1;
    int start = 0;
    while(start <= end){
        swap(s[start++], s[end--]);
    }
    
    return s;
}
```

#### Reverse Words in a String, [LeetCode151](https://leetcode.com/problems/reverse-words-in-a-string/)
该问题中的字符串前后均可能含有空格，单词之间可能有多个空格间隔。

```c
// storeIndex表示当前存储到的位置，n为字符串的长度
// 先把整个字符串翻转一遍，再循环遍历，遇到空格直接跳过，如果是非空格字符，此时看storeIndex是否为0，为0的话表示第一个单词，不用增加空格，如果不为0，说明不是第一个单词，需要在单词中间加一个空格
// 然后要找到下一个单词的结束位置用一个while循环来找下一个为空格的位置，在此过程中继续覆盖原字符串，找到结束位置了，下面就来翻转这个单词，然后更新i为结尾位置，最后遍历结束

void reverseWords(string &s) {
    int storeIndex = 0;
    int n = s.size();
    reverse(s.begin(), s.end());
    int j ;
    for(int i = 0; i < n; i++){
        if(s[i] != ' '){
            if(storeIndex) s[storeIndex++] = ' ';
            j = i;
            while(j < n && s[j] != ' ')  s[storeIndex++] = s[j++];
            reverse(s.begin() + storeIndex - (j - i),
                    s.begin() + storeIndex);
            i = j;
        }
        
    }
    
    s.resize(storeIndex);
}

```


#### Reverse Words in a String II, [LeetCode186](https://leetcode.com/problems/reverse-words-in-a-string-ii/)
该问题中的字符串前后均不含有空格，单词之间仅有单个空格间隔。
```c
// 每个单词翻转一遍，再把整个字符串翻转一遍
void reverseWords(string &s) {
    int left = 0;
    for (int i = 0; i <= s.size(); ++i) {
        if (i == s.size() || s[i] == ' ') {
            reverse(s, left, i - 1);
            left = i + 1;
        }
    }
    reverse(s, 0, s.size() - 1);
}

void reverse(string &s, int left, int right) {
    while (left < right) {
        char t = s[left];
        s[left] = s[right];
        s[right] = t;
        ++left; --right;
    }
}
```
