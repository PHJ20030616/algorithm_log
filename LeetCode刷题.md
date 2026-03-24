# [1. 两数之和](https://leetcode.cn/problems/two-sum/)

**解法一：暴力枚举**

**解法二：使用哈希表**

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> hash;
        for(int i=0;i<nums.size();i++){
            auto it=hash.find(target-nums[i]);
            if(it!=hash.end()){
                return {it->second,i};
            }else{
                hash[nums[i]]=i;
            }
        }
        return {};
    }
};
```

**注意：unordered_map中的find(x)是指key=x而不是value=x，若查找成功则返回迭代器，查找失败则返回hash.end()**

# [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>>hash;
        for(auto s:strs){
            string sorted_s=s;
            ranges::sort(sorted_s);
            hash[sorted_s].push_back(s);
        }
        vector<vector<string>> res;
        for(auto [key,val]:hash){
            res.push_back(val);
        }
        return res;
    }
};
```

**核心思想：**

利用哈希表，key为排序后的str（因为各个字母移位词排序后必定相同），val为 vector<string>类型，用于存储各个字母移位词

# [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> st(nums.begin(),nums.end());
        int res=0;
        for(int x:st){
            if(st.contains(x-1)){
                continue;
            }
            int y=x+1;
            while(st.contains(y)){
                y++;
            }
            res=max(res,y-x);
        }
        return res;

    }
};
```

**核心思想：**

1. 将数组中所有元素存入unordered_set中
2. 遍历数组int x:nums，同时在unordered_set中查找是否存在x-1
   - 若unordered_set中存在x-1，说明x并不是起始位置，直接continue
   - 若unordered_set中不存在x-1，说明x是起始位置，此时while循环，查出最长连续循环的长度



