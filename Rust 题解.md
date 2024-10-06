# Rust 题解

## 数组

### 1.二分查找

https://leetcode.cn/problems/binary-search/

```rust
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> i32 {
        let mut ans: i32 = -1;

        let mut lhs: i32 = 0;
        let mut rhs: i32 = (nums.len()-1) as i32;
        while lhs <= rhs {
            let mid: usize = (lhs + (rhs-lhs)/2) as usize;
            if nums[mid] == target {
                ans = mid as i32;
                break;
            } else if nums[mid] < target {
                lhs = mid as i32 + 1;
            } else if nums[mid] > target {
                rhs = mid as i32 - 1;
            }
        }

        ans
    }
}
```

### 2.移除元素

https://leetcode.cn/problems/remove-element/

```rust
impl Solution {
  	// slow 指针指向可放入的位置, fast 指针查找不等于 val 的元素
    pub fn remove_element(nums: &mut Vec<i32>, val: i32) -> i32 {
        let mut ans: i32 = 0;

        let mut slow: usize = 0;
        let mut fast: usize = 0;
        while fast < nums.len() {
            if nums[fast] != val {
                nums[slow] = nums[fast];
                slow += 1;
                ans += 1;
            }
            fast += 1;
        }
        ans
    }
}
```

### 3.有序数组的平方

https://leetcode.cn/problems/squares-of-a-sorted-array/

```rust
impl Solution {
  	// 按值的平方排序, 所以要根据绝对值排序
  	// 不能确定绝对值最小的值, 但是绝对值最大的值一定位于数组的两端
  	// 所以从数组两端往中间查找
    pub fn sorted_squares(nums: Vec<i32>) -> Vec<i32> {
        let mut ans: Vec<i32> = vec![0; nums.len()];
        let mut idx: usize = nums.len()-1;
        let mut lhs: usize = 0;
        let mut rhs: usize = nums.len()-1;

        while lhs <= rhs {
            if nums[lhs].abs() > nums[rhs].abs() {
                ans[idx] = nums[lhs].pow(2);
                lhs += 1;
            } else {
                ans[idx] = nums[rhs].pow(2);
                if rhs == 0 { break; } // 防止 rhs-1 溢出
                rhs -= 1;
            }
            idx -= 1;
        }
        ans
    }
}
```

### 4.长度最小的子数组

https://leetcode.cn/problems/minimum-size-subarray-sum/description/

```rust
impl Solution {
  	// 左右指针维护左开右闭的窗口
  	// 和大于等于target时缩小窗口
    pub fn min_sub_array_len(target: i32, nums: Vec<i32>) -> i32 {
        // [lhs, rhs)
        let mut lhs: usize = 0;
        let mut rhs: usize = 0;

        let mut ans: usize = 0;
        let mut sum: i32 = 0; // 当前窗口中的和
        while rhs < nums.len() { // 遍历元素
            let nr: i32 = nums[rhs];
            rhs += 1;
            sum += nr;
            while sum >= target { // 达成条件, 缩小窗口
                if sum == target {
                    ans = if ans == 0 { rhs - lhs } 
                          else { std::cmp::min(ans, rhs-lhs) };
                }
                let nl: i32 = nums[lhs];
                lhs += 1;
                sum -= nl;
            }
        }
        ans as i32
    }
}
```

### 5. 螺旋矩阵

https://leetcode.cn/problems/spiral-matrix-ii/

```rust
impl Solution {
    pub fn generate_matrix(n: i32) -> Vec<Vec<i32>> {
        let mut ans: Vec<Vec<i32>> = Vec::<Vec::<i32>>::new();
        for i in 0..n {
            ans.push(vec![0; n as usize]); // move
        }
        
        let mut i: i32 = 0;
        let mut j: i32 = 0;
        let mut num: i32 = 1;

        let mut bound_up: i32 = 0;
        let mut bound_left: i32 = 0;
        let mut bound_down: i32 = n-1;
        let mut bound_right: i32 = n-1;

        loop {
            while j <= bound_right { // up bound
                ans[i as usize][j as usize] = num;
                j += 1;
                num += 1;
            }
            i += 1;
            j -= 1;
            bound_up += 1;
            if num > n.pow(2) { break; }

            while i <= bound_down { // right bound
                ans[i as usize][j as usize] = num;
                i += 1;
                num += 1;
            }
            i -= 1;
            j -= 1;
            bound_right -= 1;
            if num > n.pow(2) { break; }

            while j >= bound_left { // down bound
                ans[i as usize][j as usize] = num;
                j -= 1;
                num += 1;
            }
            i -= 1;
            j += 1;
            bound_down -= 1;
            if num > n.pow(2) { break; }

            while i >= bound_up { // left bound
                ans[i as usize][j as usize] = num;
                i -= 1;
                num += 1;
            }
            i += 1;
            j += 1;
            bound_left += 1;
            if num > n.pow(2) { break; }
        }
        ans
    }
}
```

### 6.区间和

https://kamacoder.com/problempage.php?pid=1070

```c++
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int n = 0;
    scanf("%d", &n);
    
    vector<int> pre_sum = vector<int>(n, 0);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &pre_sum[i]);
        if (i > 0) {
            pre_sum[i] = pre_sum[i] + pre_sum[i-1];
        }
    }
    
    int lhs = 0;
    int rhs = 0;
    while (scanf("%d %d", &lhs, &rhs) != EOF) {
        if (lhs == 0)
            printf("%d\n", pre_sum[rhs]);
        else
            printf("%d\n", pre_sum[rhs] - pre_sum[lhs-1]);
    }
  
    return 0;
}
```

### 7.开发商购买土地

https://kamacoder.com/problempage.php?pid=1044

```c++
#include <iostream>
#include <vector>
#include <climits>

using namespace std;

int main() {
    int n = 0, m = 0;
    scanf("%d %d", &n, &m);
    
    vector<vector<int>> nums = vector<vector<int>>(n+1, vector<int>(m+1, 0));
    for (int i = 0; i < n; ++i) {
        int sum = 0;
        for (int j = 0; j < m; ++j) {
            scanf("%d", &nums[i][j]);
            sum += nums[i][j];
        }
        nums[i][m] = sum;
    }
    
    for (int j = 0; j < m; ++j) {
        int sum = 0;
        for (int i = 0; i < n; ++i) {
            sum += nums[i][j];
        }
        nums[n][j] = sum;
    }
    
    int ans = INT_MAX;
    // - - -
    for (int i = 0; i < n; ++i) {
        int sumA = 0; // [row_0, row_i]
        for (int j = 0; j <= i; ++j) {
            sumA += nums[j][m];
        }
        int sumB = 0; // [row_i+1, row_n-1]
        for (int j = i+1; j < n; ++j) {
            sumB += nums[j][m];
        }
        ans = min(ans, abs(sumA-sumB));
    }
    
    // | | |
    for (int i = 0; i < m; ++i) {
        int sumA = 0; // [col_0, col_i]
        for (int j = 0; j <= i; ++j) {
            sumA += nums[n][j];
        }
        int sumB = 0; // [col_i+1, col_m-1]
        for (int j = i+1; j < m; ++j) {
            sumB += nums[n][j];
        }
        ans = min(ans, abs(sumA-sumB));
    }
    
    printf("%d\n", ans);
    
    return 0;
}
```



## 链表

### 1.移除元素

https://leetcode.cn/problems/remove-linked-list-elements/

```rust
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummy = new ListNode(-1, head);
        ListNode* pred = dummy;
        ListNode* cur = dummy->next;
        while (cur) {
            if (cur->val == val) {
                pred->next = cur->next;
                cur = pred->next;
            } else {
                pred = cur;
                cur = cur->next;
            }
        }
        return dummy->next;
    }
};
```

```rust
impl Solution {
    pub fn remove_elements(head: Option<Box<ListNode>>, val: i32) -> Option<Box<ListNode>> {
        let mut dummy = Box::new(ListNode::new(0));
        dummy.next = head;

        let mut ptr = &mut dummy;
        
        // ptr.next.take() == std::mem::replace(&mut ptr.next, None)
        while let Some(node) = ptr.next.take() {
            if node.val == val {
                ptr.next = node.next;
            } else {
                ptr.next = Some(node);
                ptr = ptr.next.as_mut().unwrap();
            }
        }
        dummy.next
    }
}
```



### 2.设计链表

```c++
class MyLinkedList {
public:
    class ListNode {
    public:
        int val;
        ListNode* next;
        ListNode(int _val, ListNode* _next) : val(_val), next(_next) {}
    };
    ListNode* dummy;
    ListNode* tail;
    int length;
    MyLinkedList() 
        : dummy(new ListNode(-1, nullptr)), tail(nullptr), length(0) {
        tail = dummy;
    }
    
    int get(int index) {
        if (index >= length) {
            return -1;
        }

        return findNode(index)->val;
    }
    
    void addAtHead(int val) {
        ListNode* node = new ListNode(val, dummy->next);
        dummy->next = node;

        if (tail == dummy) { // 维护尾指针
            tail = node;
        }
        length += 1; // 维护长度
    }
    
    void addAtTail(int val) {
        ListNode* node = new ListNode(val, nullptr);
        tail->next = node;

        tail = node; // 维护尾指针
        length += 1; // 维护长度
    }
    
    void addAtIndex(int index, int val) {
        if (index > length) {
            return;
        }
        
        if (index == 0) {
            addAtHead(val);
        } else if (index == length) {
            addAtTail(val);
        } else {
            ListNode* cur = findNode(index-1);
            ListNode* node = new ListNode(val, cur->next);
            cur->next = node;
            length += 1; // 维护长度
        }
    }
    
    void deleteAtIndex(int index) {
        if (index >= length) {
            return;
        }

        ListNode* cur = findNode(index-1);
        cur->next = cur->next->next;

        if (index == length-1) {  // 维护尾指针
            tail = cur;
        }
        length -= 1; // 维护长度
    }

    ListNode* findNode(int index) {
        // index 保证合法
        int idx = -1;
        ListNode* p = dummy;
        while (idx < index) {
            idx += 1;
            p = p->next;
        }
        return p;
    }
};
```

### 3.反转链表

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (! head || ! head->next)
            return head;

        ListNode* head_n = reverseList(head->next);
        ListNode* p = head_n;
        while (p->next) {
            p = p->next;
        }
        p->next = head;
        head->next = nullptr;

        return head_n;
    }
};
```

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (! head || ! head->next)
            return head;

        ListNode* prev = head;
        ListNode* cur = head->next;
        while (cur) {
            ListNode* next = cur->next;
            cur->next = prev;

            prev = cur;
            cur = next;
        }
        head->next = nullptr;
        return prev;
    }
};
```

### 4.两两交换链表中的节点

https://leetcode.cn/problems/swap-nodes-in-pairs/description/

```c++
// 递归
// 定义 swapPairs: 两两交换链表中的节点
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (! head || ! head->next) {
            return head; // 0个或一个节点
        }
        ListNode* next = swapPairs(head->next->next);
        ListNode* ans = head->next;
        ans->next = head;
        head->next = next;

        return ans;
    }
};
```

### 5.删除链表的倒数第N个节点

https://leetcode.cn/problems/remove-nth-node-from-end-of-list/

```rust
impl Solution {
    pub fn remove_nth_from_end(head: Option<Box<ListNode>>, n: i32) -> Option<Box<ListNode>> {
        let mut dummy = Box::new(ListNode {
            val: -1,
            next: head,
        });
				
      	// 双指针解法需要一个可变借用和不可变借用
      	// 使用裸指针得到可变借用
        let mut p1 = &mut dummy as *mut Box<ListNode>;
        let mut p2 = &dummy;

        for _ in 0..n {
            p2 = p2.next.as_ref().unwrap();
        }

        while p2.next.is_some() {
            unsafe { p1 = (*p1).next.as_mut().unwrap() as *mut Box<ListNode>; }
            p2 = p2.next.as_ref().unwrap();
        }

        unsafe {
            let nxt = (*p1).next.take().unwrap();
            (*p1).next = nxt.next;
        }

        dummy.next
    }
}
```

```c++
// 快慢指针
// 快指针先走 N 步
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(-1, head);

        ListNode* slow = dummy;
        ListNode* fast = dummy;

        for (int i = 0; i < n; ++i) {
            fast = fast->next;
        }

        while (fast->next) {
            slow = slow->next;
            fast = fast->next;
        }

        slow->next = slow->next->next;

        return dummy->next;
    }
};
```

### 6.链表相交

https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* pa = headA;
        ListNode* pb = headB;

        while (pa != nullptr || pb != nullptr) {
            if (pa == pb) return pa;

            if (pa == nullptr) pa = headB;
            else pa = pa->next;

            if (pb == nullptr) pb = headA;
            else pb = pb->next;
        }

        return nullptr;
    }
};
```

### 7.环形链表

https://leetcode.cn/problems/linked-list-cycle-ii/

```c++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head == nullptr) return nullptr;

        ListNode* dummy = new ListNode(-1, head);
        ListNode* slow = dummy;
        ListNode* fast = dummy;

        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) {
                slow = dummy;
                while (slow != fast) {
                    slow = slow->next;
                    fast = fast->next;
                }
                return slow;
            }
        }
        return nullptr;
    }
};
```

## 哈希表

### 1.有效的字母异位词

https://leetcode.cn/problems/valid-anagram/

```rust
impl Solution {
    pub fn is_anagram(s: String, t: String) -> bool {
        let mut mm_s: Vec<u32> = vec![0; 26];
        let mut mm_t: Vec<u32> = vec![0; 26];

        for c in s.chars() {
            let idx: usize = c as usize - 'a' as usize;
            mm_s[idx] += 1;
        }

        for c in t.chars() {
            let idx: usize = c as usize - 'a' as usize;
            mm_t[idx] += 1;
        }

        for idx in 0_usize..26_usize {
            if mm_s[idx] != mm_t[idx] {
                return false;
            }
        }

        true
    }
}
```

### 2.两个数组的交集

https://leetcode.cn/problems/intersection-of-two-arrays/submissions/567691468/

```rust
impl Solution {
    pub fn intersection(nums1: Vec<i32>, nums2: Vec<i32>) -> Vec<i32> {
        let mut ans: Vec<i32> = Vec::new();

        let mut map_nums1: [bool; 1005] = [false; 1005];
        let mut map_ans: [bool; 1005] = [false; 1005];

        for num in nums1.iter() {
            let idx = *num as usize;
            map_nums1[idx] = true;
        }

        for num in nums2.iter() {
            let idx = *num as usize;
            if map_nums1[idx] && !map_ans[idx] {
                ans.push(*num);
                map_ans[idx] = true;
            }
        }

        ans
    }
}
```

### 3.快乐数

https://leetcode.cn/problems/happy-number/

```rust
impl Solution {
    pub fn is_happy(n: i32) -> bool {
        use std::collections::HashSet;
        let mut set: HashSet<i32> = HashSet::new();

        let mut n: i32 = n;
        let mut sum: i32 = 0;

        loop {
            while n > 0 {
                sum += (n%10).pow(2);
                n /= 10;
            }
            
            if sum == 1 { break true; }
            if set.contains(&sum) { break false; }

            set.insert(sum);
            n = sum;
            sum = 0;
        }
    }
}
```

### 4.两数之和

https://leetcode.cn/problems/two-sum/

```rust
impl Solution {
  	// 先建立映射表, 再查表
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        use std::collections::HashMap;
        let mut map: HashMap<i32, usize> = HashMap::new();
        let mut idx: usize = 0;
        let mut ans: Vec<i32> = Vec::new();

        for num in &nums {
            let tmp: i32 = target - nums[idx];
            if map.contains_key(&tmp) {
                ans.push(map.get(&tmp).copied().unwrap() as i32);
                ans.push(idx as i32);
                break;
            }
            map.insert(*num, idx);
            idx += 1;
        }

        ans
    }
}
```

### 5.四数相加

https://leetcode.cn/problems/4sum-ii/

```rust
impl Solution {
  	// 先建立映射表, 再查表
    pub fn four_sum_count(nums1: Vec<i32>, nums2: Vec<i32>, nums3: Vec<i32>, nums4: Vec<i32>) -> i32 {
        use std::collections::HashMap;
        let mut map = HashMap::<i32, u32>::new();

        for n1 in &nums1 {
            for n2 in &nums2 {
                let sum: i32 = *n1 + *n2;
                let cnt = map.entry(sum).or_insert(0_u32);
                *cnt += 1_u32;
            }
        }
        
        let mut ans: i32 = 0;
        for n3 in &nums3 {
            for n4 in &nums4 {
                let tmp = 0 - (*n3) - (*n4);
                if map.contains_key(&tmp) {
                    ans += map.get(&tmp).copied().unwrap() as i32;
                }
            }
        }

        ans
    }
}
```

### 6.赎金信

https://leetcode.cn/problems/ransom-note/

```rust
impl Solution {
  	// magazine[i]'s cnt >= ransom_note[j]
    pub fn can_construct(ransom_note: String, magazine: String) -> bool {
        let mut mm = [0; 26];
        for c in magazine.chars() {
            let idx = c as usize - 'a' as usize;
            mm[idx] += 1;
        }

        for c in ransom_note.chars() {
            let idx = c as usize - 'a' as usize;
            if mm[idx] <= 0 {
                return false;
            } else {
                mm[idx] -= 1;
            }
        }

        true
    }
}
```

### 7.三数之和

https://leetcode.cn/problems/3sum/

```rust
impl Solution {
    pub fn three_sum(nums: Vec<i32>) -> Vec<Vec<i32>> {
        let mut ans = Vec::<Vec::<i32>>::new();
        let mut nums = nums;
        nums.sort();

        for idx in 0_usize..nums.len()-2 {
            if idx > 0 && nums[idx] == nums[idx-1] { // 去重
                continue;
            }
            let mut lhs = idx + 1;
            let mut rhs = nums.len()-1;
            while lhs < rhs {
                let sum = nums[idx] + nums[lhs] + nums[rhs];
                if sum == 0 {
                    let mut tmp = vec![-1; 3];
                    tmp[0] = nums[idx];
                    tmp[1] = nums[lhs];
                    tmp[2] = nums[rhs];
                    ans.push(tmp);
                    
                    lhs += 1;
                    rhs -= 1;
                    while lhs < rhs && nums[lhs] == nums[lhs-1] { // 去重
                        lhs += 1;
                    }
                    while lhs < rhs && nums[rhs] == nums[rhs+1] { // 去重
                        rhs -= 1;
                    }
                } else if sum < 0 {
                    lhs += 1;
                    while lhs < rhs && nums[lhs] == nums[lhs-1] { // 调优
                        lhs += 1;
                    }
                } else if sum > 0 {
                    rhs -= 1;
                    while lhs < rhs && nums[rhs] == nums[rhs+1] { // 调优
                        rhs -= 1;
                    }
                }
            }
        }
        ans
    }
}
```

### 8.四数之和

https://leetcode.cn/problems/4sum/

```rust
impl Solution {
    pub fn four_sum(nums: Vec<i32>, target: i32) -> Vec<Vec<i32>> {
        let mut ans = Vec::<Vec::<i32>>::new();
        if nums.len() < 4 {
            return ans;
        }
        let mut nums = nums;
        nums.sort();

        for idx in 0_usize..nums.len()-3 {
            if idx > 0 && nums[idx] == nums[idx-1] { // 去重
                continue;
            }
            let tmp = Solution::three_sum(&nums, idx+1, target-nums[idx]);
            for mut t in tmp {
                t.push(nums[idx]);
                ans.push(t);
            }
        }
        ans
    }
    pub fn three_sum(nums: &Vec<i32>, beg: usize, target: i32) -> Vec<Vec<i32>> {
        let mut ans = Vec::<Vec::<i32>>::new();

        for idx in beg..nums.len()-2 {
            if idx > beg && nums[idx] == nums[idx-1] { // 去重
                continue;
            }
            let mut lhs = idx + 1;
            let mut rhs = nums.len()-1;
            while lhs < rhs {
                let sum = nums[idx] as i64 + nums[lhs] as i64 + nums[rhs] as i64;
                if sum == target as i64 {
                    let mut tmp = vec![-1; 3];
                    tmp[0] = nums[idx];
                    tmp[1] = nums[lhs];
                    tmp[2] = nums[rhs];
                    ans.push(tmp);
                    
                    lhs += 1;
                    rhs -= 1;
                    while lhs < rhs && nums[lhs] == nums[lhs-1] {
                        lhs += 1;
                    }
                    while lhs < rhs && nums[rhs] == nums[rhs+1] {
                        rhs -= 1;
                    }
                } else if sum < target as i64 {
                    lhs += 1;
                    while lhs < rhs && nums[lhs] == nums[lhs-1] {
                        lhs += 1;
                    }
                } else if sum > target as i64 {
                    rhs -= 1;
                    while lhs < rhs && nums[rhs] == nums[rhs+1] {
                        rhs -= 1;
                    }
                }
            }
        }
        ans
    }
}
```



## 字符串

### 1.反转字符串

https://leetcode.cn/problems/reverse-string/submissions/568302722/

```rust
impl Solution {
    pub fn reverse_string(s: &mut Vec<char>) {
        let mut lhs: usize = 0;
        let mut rhs: usize = s.len()-1;

        while lhs < rhs {
            let tmp: char = s[lhs];
            s[lhs] = s[rhs];
            s[rhs] = tmp;

            lhs += 1;
            rhs -= 1;
        }
    }
}
```

### 2.反转字符串 II

https://leetcode.cn/problems/reverse-string-ii/

```rust
impl Solution {
    pub fn sup_func(s: &mut Vec<char>, mut beg: usize, mut end: usize) {
        end -= 1;
        while beg < end {
            let tmp: char = s[beg];
            s[beg] = s[end];
            s[end] = tmp;

            beg += 1;
            end -= 1;
        }
    }
    pub fn reverse_str(s: String, k: i32) -> String {
        let k: usize = k as usize;
        let mut vec_c: Vec<char> = Vec::new();
        for c in s.chars() {
            vec_c.push(c);
        }

        let mut beg: usize = 0;
        let mut end: usize = 0;
        while end <= s.len() { // [beg, end)
            if end - beg == 2*k {
                Solution::sup_func(&mut vec_c, beg, beg + k);
                beg = end;
            } else {
                end += 1;
            }
        }
        let remain_length: usize = s.len() - beg;
        if remain_length >= k { // 剩余长度 >= k
            Solution::sup_func(&mut vec_c, beg, beg + k);
        } else if remain_length < k { // 剩余长度 < k
            Solution::sup_func(&mut vec_c, beg, s.len());
        }

        let mut ans: String = String::new();
        for c in vec_c.into_iter() {
            ans.push(c);
        }
        ans
    }
}
```

### 3.替换数字

https://kamacoder.com/problempage.php?pid=1064

```c++
#include <iostream>
#include <string>
using namespace std;

int main() {
    string str;
    cin >> str;
    
    string ans;
    
    for (int i = 0; i < str.size(); ++i) {
        if (str[i] >= '0' && str[i] <= '9') {
            ans += "number";
        } else {
            ans.push_back(str[i]);
        }
    }
    
    cout << ans << endl;
    
    return 0;
}
```

### 4.反转字符串中的单词

https://leetcode.cn/problems/reverse-words-in-a-string/

```rust
// 处理前导空格，中间空格，尾后空格
// 全部旋转
// 单独旋转每个单词（双指针）
impl Solution {
    pub fn reverse_vec(vec_c: &mut Vec<char>, mut beg: usize, mut end: usize) {
        end -= 1;
        while beg < end {
            let tmp: char = vec_c[beg];
            vec_c[beg] = vec_c[end];
            vec_c[end] = tmp;

            beg += 1;
            end -= 1;
        }
    }
    pub fn reverse_words(s: String) -> String {
        let mut vec_c = Vec::<char>::new();
        let mut is_pre_space = true;
        let mut is_mid_space = false;
        for c in s.chars() {
            if c == ' ' && is_pre_space == true { // 前导空格
                continue;
            }
            if is_pre_space == true {
                is_pre_space = false;
            }

            if c == ' ' { // 中间空格
                if is_mid_space == false {
                    is_mid_space = true;
                    vec_c.push(c);
                } else {
                    continue;
                }
            } else {
                is_mid_space = false;
                vec_c.push(c);
            }
        }
        // 尾后空格(至多一个)
        if vec_c.len() > 0 && vec_c[vec_c.len()-1] == ' ' {
            vec_c.pop();
        }
        
        let mut beg: usize = 0;
        let mut end: usize = vec_c.len();
        Solution::reverse_vec(&mut vec_c, beg, end);

        beg = 0;
        end = 0;
        while end < vec_c.len() {
            if vec_c[end] != ' ' {
                end += 1;
            } else {
                Solution::reverse_vec(&mut vec_c, beg, end);
                beg = end + 1;
                end = beg;
            }
        }
        Solution::reverse_vec(&mut vec_c, beg, end);

        let mut ans = String::new();
        for c in vec_c.into_iter() {
            ans.push(c);
        }
        ans
    }
}
```

### 5.右旋字符串

https://kamacoder.com/problempage.php?pid=1065

```c++
// 先全部反转
// 反转 [0, k)
// 反转 [k, end)

#include <iostream>
#include <string>
using namespace std;

void reverse_str(string &str, int beg, int end) {
    end -= 1;
    while (beg < end) {
        char tmp = str[beg];
        str[beg] = str[end];
        str[end] = tmp;
        
        beg += 1;
        end -= 1;
    }
}

int main() {
    int k;
    cin >> k;
    string str;
    cin >> str;
    
    reverse_str(str, 0, str.size());
    reverse_str(str, 0, k);
    reverse_str(str, k, str.size());
    
    cout << str << endl;
 
    return 0;   
}
```

### 6.KMP



### 7.重复的子字符串

https://leetcode.cn/problems/repeated-substring-pattern/description/

```rust
// abcabc -> a bc|abcabc|ab c
impl Solution {
    pub fn repeated_substring_pattern(s: String) -> bool {
        let s_double = format!("{}{}", &s[1..], &s[..s.len()]);
        match s_double.find(&s[..]) {
            Some(_) => true,
            None => false,
        }
    }
}
```



## 栈与队列

### 1.用栈实现队列

https://leetcode.cn/problems/implement-queue-using-stacks/description/

```rust
// 使用双栈，栈1用来入队，栈2用来出队
use std::collections::VecDeque;
struct MyQueue {
    deque1: VecDeque<i32>,
    deque2: VecDeque<i32>,
}
/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl MyQueue {
    
    fn new() -> Self {
        MyQueue {
            deque1: VecDeque::new(),
            deque2: VecDeque::new(),
        }
    }
    
    fn push(&mut self, x: i32) {
        if self.deque1.is_empty() {
            while ! self.deque2.is_empty() {
                let val = self.deque2.pop_back().unwrap();
                self.deque1.push_back(val);
            }
        }
        self.deque1.push_back(x);
    }
    
    fn pop(&mut self) -> i32 {
        if self.deque2.is_empty() {
            while ! self.deque1.is_empty() {
                let val = self.deque1.pop_back().unwrap();
                self.deque2.push_back(val);
            }
        }
        self.deque2.pop_back().unwrap()
    }
    
    fn peek(&mut self) -> i32 {
        if self.deque2.is_empty() {
            while ! self.deque1.is_empty() {
                let val = self.deque1.pop_back().unwrap();
                self.deque2.push_back(val);
            }
        }
        self.deque2.back().copied().unwrap()
    }
    
    fn empty(&self) -> bool {
        self.deque1.is_empty() && self.deque2.is_empty()
    }
}
```

### 2.用队列实现栈

https://leetcode.cn/problems/implement-stack-using-queues/

```rust
// 使用双队列，元素要么都在队列1，要么都在队列2
use std::collections::VecDeque;
struct MyStack {
    queue1: VecDeque<i32>,
    queue2: VecDeque<i32>,
}
/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl MyStack {
    // 
    // push_back, pop_front
    fn new() -> Self {
        MyStack {
            queue1: VecDeque::new(),
            queue2: VecDeque::new(),
        }
    }
    
    fn push(&mut self, x: i32) {
        if ! self.queue1.is_empty() {
            self.queue1.push_back(x);
        } else {
            self.queue1.push_back(x);
        }
    }
    
    fn pop(&mut self) -> i32 {
        if ! self.queue1.is_empty() { // 元素在队列1
            while self.queue1.len() > 1 {
                let tmp = self.queue1.pop_front().unwrap();
                self.queue2.push_back(tmp);
            }
            self.queue1.pop_front().unwrap()
        } else {
            while self.queue2.len() > 1 {
                let tmp = self.queue2.pop_front().unwrap();
                self.queue1.push_back(tmp);
            }
            self.queue2.pop_front().unwrap()
        }
    }
    
    fn top(&mut self) -> i32 {
        if ! self.queue1.is_empty() { // 元素在队列1
            while self.queue1.len() > 1 {
                let tmp = self.queue1.pop_front().unwrap();
                self.queue2.push_back(tmp);
            }
            let ans = self.queue1.back().copied().unwrap();
            self.queue2.push_back(self.queue1.pop_front().unwrap());
            ans
        } else {
            while self.queue2.len() > 1 {
                let tmp = self.queue2.pop_front().unwrap();
                self.queue1.push_back(tmp);
            }
            let ans = self.queue2.back().copied().unwrap();
            self.queue1.push_back(self.queue2.pop_front().unwrap());
            ans
        }
    }
    
    fn empty(&self) -> bool {
        self.queue1.is_empty() && self.queue2.is_empty()
    }
}
```

### 3.有效的括号

https://leetcode.cn/problems/valid-parentheses/

```rust
// 使用栈记录左括号，遇到右括号就用栈顶元素匹配
use std::collections::VecDeque;
impl Solution {
    pub fn _is_valid(c1: char, c2: char) -> bool {
        if c1 == '{' && c2 == '}' {
            true
        } else if c1 == '[' && c2 == ']' {
            true
        } else if c1 == '(' && c2 == ')' {
            true
        } else {
            false
        }
    }
    pub fn is_valid(s: String) -> bool {
        let mut stack = VecDeque::<char>::new();
        for c in s.chars() {
            if c == '{' || c == '[' || c == '(' {
                stack.push_back(c);
            } else {
                if stack.is_empty() {
                    return false;
                } else if (! Solution::_is_valid(stack.back().copied().unwrap(), c)) {
                    // match fail
                    return false;
                } else {
                    stack.pop_back();
                }
            }
        }
        stack.is_empty()
    }
}
```

### 4.删除字符串中的重复项

https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/

```rust
// 使用栈记录，不与栈顶字符相同就入栈，否则出栈
use std::collections::VecDeque;
impl Solution {
    pub fn remove_duplicates(s: String) -> String {
        let mut stack = VecDeque::<char>::new();

        for c in s.chars() {
            if stack.is_empty() || *stack.back().unwrap() != c {
                // 可以入栈
                stack.push_back(c);
            } else {
                stack.pop_back();
            }
        }
        
        let mut ans = String::new();
        for c in stack {
            ans.push(c);
        }
        
        ans
    }
}
```

### 5.逆波兰表达式求值

https://leetcode.cn/problems/evaluate-reverse-polish-notation/

```rust
use std::collections::VecDeque;
impl Solution {
    pub fn eval_rpn(tokens: Vec<String>) -> i32 {
        let mut stack = VecDeque::<i32>::new();

        for token in tokens {
            if token == "+" || token == "-" || token == "*" || token == "/" {
                // 遇到操作符就计算结果
                let num_r = stack.pop_back().unwrap();
                let num_l = stack.pop_back().unwrap();
                match &token[..] {
                    "+" => stack.push_back(num_l + num_r),
                    "-" => stack.push_back(num_l - num_r),
                    "*" => stack.push_back(num_l * num_r),
                    "/" => stack.push_back(num_l / num_r),
                    _ => (),
                }
            } else {
                // 遇到数字就入栈
                let num = token.parse::<i32>().unwrap();
                stack.push_back(num);
            }
        }
        stack.pop_back().unwrap()
    }
}
```

### 6.滑动窗口最大值

https://leetcode.cn/problems/sliding-window-maximum/

```rust
// 使用单调队列（front() -> back(): max -> min）
use std::collections::VecDeque;
impl Solution {
    pub fn max_sliding_window(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let k = k as usize;

        let mut ans = Vec::<i32>::new();
        let mut deque = VecDeque::<i32>::new();
        for idx in 0..nums.len() {
            // 相等的元素不应该弹出，因为需要判断是否是该真正的队头（因为没有记录位置信息，只根据值是不能判断出是真正的队头的）
            while ! deque.is_empty() && nums[idx] > *deque.back().unwrap() {
                deque.pop_back();
            }
            deque.push_back(nums[idx]);
            if idx < k-1 { continue; }
            
            // idx >= k-1, 才进入下面的逻辑
            // 此时 deque.front() is the max val
            ans.push(deque.front().copied().unwrap());
            
            // 检查当前队头元素是否是真正的队头元素
            if nums[idx-k+1] == *deque.front().unwrap() {
                deque.pop_front();
            }
        }
        ans
    }
}
```

### 7.前K个高频元素

https://leetcode.cn/problems/top-k-frequent-elements/

```rust
use std::cmp::Ordering;
use std::collections::BinaryHeap;
use std::collections::HashMap;
struct MyInt {
    data: i32,
    freq: u32,
}
impl Eq for MyInt { }
impl PartialEq for MyInt {
    fn eq(&self, other: &Self) -> bool {
        self.freq == other.freq
    }
}
impl Ord for MyInt {
    fn cmp(&self, other: &Self) -> Ordering {
        self.freq.cmp(&other.freq) // max
        //other.data.cmp(&self.data) // min
    }
}
impl PartialOrd for MyInt {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
        Some(self.cmp(other))
    }
}

impl Solution {
    pub fn top_k_frequent(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let mut map = HashMap::<i32, MyInt>::new();    
        for num in nums {
            let tmp = map.entry(num).or_insert(MyInt { data:num, freq:0 });
            tmp.freq += 1;
        }

        let mut max_heap = BinaryHeap::<MyInt>::new();
        for pair in map{ 
            max_heap.push(pair.1);
        }

        let mut ans = Vec::<i32>::new();
        for i in 0..k {
           ans.push(max_heap.pop().unwrap().data);
        }

        ans
    }
}
```

## 树

### 1.二叉树的层序遍历

https://leetcode.cn/problems/binary-tree-level-order-traversal/

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        
        if (! root) return ans;
        q.push(root);

        while (! q.empty()) {
            int sz = q.size();
            vector<int> tmp;
            for (int i = 0; i < sz; ++i) {
                TreeNode* cur = q.front();
                q.pop();

                tmp.push_back(cur->val);

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            ans.push_back(tmp);
        }
        return ans;
    }
};
```

### 2.二叉树的层序遍历II

https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/description/

```c++
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        
        if (! root) return ans;
        q.push(root);

        while (! q.empty()) {
            int sz = q.size();
            vector<int> tmp;
            for (int i = 0; i < sz; ++i) {
                TreeNode* cur = q.front();
                q.pop();

                tmp.push_back(cur->val);

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            ans.push_back(tmp);
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

### 3.二叉树的右视图

https://leetcode.cn/problems/binary-tree-right-side-view/description/

```c++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        if (! root) return ans;

        queue<TreeNode*> q;
        q.push(root);

        while (! q.empty()) {
            int sz = q.size();
            for (int i = 0; i < sz; ++i) {
                TreeNode* cur = q.front();
                q.pop();

                if (i == sz-1) ans.push_back(cur->val);

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
        }
        return ans;
    }
};
```

### 4.二叉树的层平均值

https://leetcode.cn/problems/average-of-levels-in-binary-tree/description/

```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        if (! root) return ans;

        queue<TreeNode*> q;
        q.push(root);

        while (! q.empty()) {
            int sz = q.size();
            double sum = 0;
            for (int i = 0; i < sz; ++i) {
                TreeNode* cur = q.front();
                q.pop();

                sum += cur->val;

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            ans.push_back(sum / sz);
        }
        return ans;
    }
};
```

### 5.N叉树的层序遍历

https://leetcode.cn/problems/n-ary-tree-level-order-traversal/description/

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> ans;
        if (! root) return ans;

        queue<Node*> q;
        q.push(root);

        while (! q.empty()) {
            int sz = q.size();
            vector<int> tmp;
            for (int i = 0; i < sz; ++i) {
                Node* cur = q.front();
                q.pop();

                tmp.push_back(cur->val);

                for (Node* child : cur->children) {
                    if (child) q.push(child);
                }
            }
            ans.push_back(tmp);
        }
        return ans;
    }
};
```

### 6.在每个树行中找最大值

https://leetcode.cn/problems/find-largest-value-in-each-tree-row/description/

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        vector<int> ans;
        if (! root) return ans;

        queue<TreeNode*> q;
        q.push(root);

        while (! q.empty()) {
            int sz = q.size();
            int tmp = INT_MIN;
            for (int i = 0; i < sz; ++i) {
                TreeNode* cur = q.front();
                q.pop();

                tmp = max(tmp, cur->val);

                if (cur->left) q.push(cur->left);
                if (cur->right) q.push(cur->right);
            }
            ans.push_back(tmp);
        }
        return ans;
    }
};
```

### 7.
