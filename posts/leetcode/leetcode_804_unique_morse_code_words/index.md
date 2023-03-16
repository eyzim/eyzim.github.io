# LeetCode 804. Unique Morse Code Words 解題紀錄



## [題目](https://leetcode.com/problems/unique-morse-code-words/)


{{< admonition type=quote title="Problem">}}

International Morse Code defines a standard encoding where each letter is mapped to a series of dots and dashes, as follows:

- 'a' maps to ".-",
- 'b' maps to "-...",
- 'c' maps to "-.-.", and so on.
For convenience, the full table for the `26` letters of the English alphabet is given below:
```
[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]
```
Given an array of strings `words` where each word can be written as a concatenation of the Morse code of each letter.

For example, `"cab"` can be written as `"-.-..--..."`, which is the concatenation of `"-.-."`, `".-"`, and `"-..."`. We will call such a concatenation the **transformation** of a word.
Return *the number of different transformations among all words we have*.

 

Example 1:

```
Input: words = ["gin","zen","gig","msg"]
Output: 2
```
Explanation: The transformation of each word is:
"gin" -> "--...-."
"zen" -> "--...-."
"gig" -> "--...--."
"msg" -> "--...--."
There are 2 different transformations: "--...-." and "--...--.".


Example 2:
```
Input: words = ["a"]
Output: 1
```

Constraints:

- 1 $\leq$ `words.length` $\leq$ 100
- 1 $\leq$ `words[i].length` $\leq$ 12
- words[i] consists of lowercase English letters.

{{< /admonition >}}


## 想法


題目已經給好 a 到 z 的對照表，新增一個陣列，對照存入後，只要依照題目給予的字串即可生成替代的摩斯密碼。

最後再存入 set 中，若無重複即存入，計算 set 中個數就可以知道有幾組未重複組合。

英文 char 如何轉成數字 [看這邊](https://eyzim.github.io/posts/cpp/cpp_int_to_string/) 或是下面的小抄

 * char to int: **`a[i] - 'a'`**

ps: 這個問題好像被我 google 了 n 遍了 :rofl:

## 解法


```cpp
class Solution {
public:
    int uniqueMorseRepresentations(vector<string>& words) {
        
		// 密碼對照表
        string mp[26] = {".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."};
        
        set<string> produce_code;
        
		// 一一讀入 string vector 中的每個字串
        for(auto word:words)
        {
            string comb = "";
            
			// 一一讀入字串的字母
            for(auto i:word)
            {
			    // 將 char 轉為數字，查表查出正確翻譯
                comb += mp[i-'a'];
            }
            
			// 存入 set 中確認有幾組未重複組合
            produce_code.insert(comb);
        }
        
        return produce_code.size();
    }
};
```


- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.
