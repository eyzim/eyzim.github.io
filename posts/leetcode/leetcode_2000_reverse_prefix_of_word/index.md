# LeetCode 2000. Reverse Prefix of Word 解題紀錄


## [題目](https://leetcode.com/problems/reverse-prefix-of-word/)

{{< admonition type=quote title="Problem">}}

Given a **0-indexed** string word and a character `ch`, **reverse** the segment of `word` that starts at index `0` and ends at the index of the **first occurrence** of `ch` (**inclusive**). If the character `ch` does not exist in `word`, do nothing.

For example, if `word = "abcdefd"` and `ch = "d"`, then you should **reverse** the segment that starts at 0 and ends at `3` (**inclusive**). The resulting string will be `"dcbaefd"`.
Return the resulting string.

**Example 1:**

```
Input: word = "abcdefd", ch = "d"
Output: "dcbaefd"
```

Explanation: The first occurrence of "d" is at index 3.
Reverse the part of word from 0 to 3 (inclusive), the resulting string is "dcbaefd".

**Example 2:**

```
Input: word = "xyxzxe", ch = "z"
Output: "zxyxxe"
```

Explanation: The first and only occurrence of "z" is at index 3.
Reverse the part of word from 0 to 3 (inclusive), the resulting string is "zxyxxe".

**Example 3:**

```
Input: word = "abcd", ch = "z"
Output: "abcd"
```

Explanation: "z" does not exist in word.
You should not do any reverse operation, the resulting string is "abcd".

**Constraints:**

-   1 $\leq$ `word.length` $\leq$ 250
-   `word` consists of lowercase English letters.
-   `ch` is a lowercase English letter.

{{< /admonition >}}

## 想法

1. Find the position of `ch`, and then swap all the characters from 0 to the position of `ch`
2. STL trick in two lines: find the position of `ch`, then reverse.

## 解法

### 解法一

{{< image
src="/images/leetcode_2000/swap.png"  height="400"
src_s="/images/leetcode_2000/swap.png"
src_l="/images/leetcode_2000/swap.png"
alt="交換文字片段"
caption="交換文字片段"
linked="false">}}

```cpp
string reversePrefix(string word, char ch) {

	// get the position of 'ch'
	int ch_pos = word.find(ch);

	// if 'ch' is not in the string, return it
	if(ch_pos == -1)
	{
		return word;
	}

	// if 'ch' exists in the string, swap those characters from index 0 to ch_pos
	// mind that we only have to swap ch_pos/2 times
	for(auto i=0; i<=ch_pos/2; i++)
	{
		int temp = word[ch_pos-i];
		word[ch_pos-i] = word[i];
		word[i] = temp;
	}

	return word;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

### 解法二

```cpp
string reversePrefix(string word, char ch) {

	reverse(word.begin(), word.begin()+word.find(ch)+1);

	return word;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

