# LeetCode 190. Reverse Bits 解題紀錄



## [題目](https://leetcode.com/problems/reverse-bits/)


{{< admonition type=quote title="Problem">}}
Reverse bits of a given 32 bits unsigned integer.


**Note**:

- Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using `2's complement notation`. Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.
 

**Example 1:**
```
Input: n = 00000010100101000001111010011100
Output:    964176192 (00111001011110000010100101000000)
```

Explanation: 

- The input binary string 00000010100101000001111010011100 
represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.

**Example 2:**

```
Input: n = 11111111111111111111111111111101 
Output:   3221225471 (10111111111111111111111111111111)
```

Explanation: 

- The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.



**Constraints:**

The input must be a binary string of length 32
 

**Follow up:** If this function is called many times, how would you optimize it?
{{< /admonition >}}



## 想法

使用的是 uint32_t 這個 type


## 解法

### 解法一

宣告一個 bitset，逐一做 bit swap


{{< image src="/images/leetcode_190/xor_truth_table.png"  height="200" 
          src_s="/images/leetcode_190/xor_truth_table.png" 
          src_l="/images/leetcode_190/xor_truth_table.png" 
caption="XOR truth map" linked="false">}}

    
```cpp

    class Solution {
    public:
        uint32_t reverseBits(uint32_t n) {
            
            bitset<32> bit(n);
            
            for(auto i=0; i<16; i++)
            {
                bit[i] = bit[31-i] ^ bit[i];
                bit[31-i] = bit[31-i] ^ bit[i];
                bit[i] = bit[31-i] ^ bit[i];
            }
            
            return (uint32_t) bit.to_ulong();
        }
    };
```
    
- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(1)$.
    

### 解法二

使用 count 1s 的方法，檢查是否為 1。
    
{{< image src="/images/leetcode_190/phase2_steps.png"  height="200" src_s="/images/leetcode_190/phase2_steps.png" src_l="/images/leetcode_190/phase2_steps.png" 
caption="moving steps" linked="false">}}
    
```cpp
    class Solution {
    public:
        uint32_t reverseBits(uint32_t n) {
            
            uint32_t ans = 0;
            
            for(auto i=0; i<32; i++)
            {
                ans = (ans<<1) | ((n>>i)&1);
            }
            
            return ans;
        }
    };
```
    
- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(1)$.


## Reference
1. http://blogs.plymouth.ac.uk/embedded-systems/glossary-2/logical-xor-glossary-entry/
