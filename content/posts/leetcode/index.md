+++
title = "LeetCode Daily Notes"
date = "2025-05-03T11:12:30+08:00"
draft = false
math = true
tags = ["leetcode"]
+++
> 先把所有筆記都寫一起等哪天塞不下了再分開，練習用英文寫
## <span class="tag easy">Easy</span>

### build-array-from-permutation
- [Link](https://leetcode.com/problems/build-array-from-permutation/)
<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    vector<int> buildArray(vector<int>& nums) {
        int len=nums.size();
        vector<int> ans(len);
        for(int i=0; i < len; i++)
            ans[i]=nums[nums[i]];
        return ans;
    }
};
```

</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** This problem is trivial if we dont use O(1) memory
- **Improving:** We can use a slot to store two information, for example, if `nums[0]=5 and nums[5]=10`, `nums[0]=5+1024*10` can store both information of 5 and 10, so that we can get `5` by `nums[0]%1024` if `nums[i]=0` needs it, and in the end, we can get `10` by `nums[0]/1024` for the answer array. [Source](https://leetcode.com/problems/build-array-from-permutation/solutions/6718380/o-n-o-1-space-with-images-example-walkthrough-c-python-java)

### number-of-equivalent-domino-pairs
- [Link](https://leetcode.com/problems/number-of-equivalent-domino-pairs/)
<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        int len=dominoes.size();
        vector<vector<int>> table(9, vector<int>(9, 0));
        int ans=0;
        int sum=0;
        for(int i=0; i < len; i++)
            table[dominoes[i][0]-1][dominoes[i][1]-1]++;
        for(int i=0; i < 9; i++)
            for(int j=0; j < 9; j++){
                if(i<j){
                    sum=table[i][j]+table[j][i];
                    ans+=sum*(sum-1)/2;
                }
                if(i==j) ans+=table[i][j]*(table[i][j]-1)/2;
            }
        return ans;
    }
};
  ```

</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** I use a 9*9 table to count each possible pair and count the combination of `table[i][j]+table[j][i]`. 
- **Improving:** I find that I can map the value to a two-digit positive integer,`i.e., (x,y)→10x+y.` Thus, I can use a 100 array to count the pairs. Also, for the combination, I dont need to count the combination in the end with `com = sum*(sum-1)/2`, instead, I can count it during the iteration with `com += num[val]; num[val]++;`. That is, I should notice That
{{< katex >}}
\\(C(n, 2) = \sum_{i=1}^{n-1} i = \frac{n(n - 1)}{2}\\)

## <span class="tag medium">Medium</span>

### domino-and-tromino-tiling
- [Link](https://leetcode.com/problems/domino-and-tromino-tiling/)
<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    int numTilings(int n) {
        int ans;
        long long one=1;
        long long two=0;
        long long sum2=0;
        int mod=1000000007;
        for(int i = 0; i < n; i++){
            ans = (one + two + sum2) % mod;
            if(i == 0) two=1;
            if(i == 1){
                one=2;
                sum2=2*1;
            }
            if(i >= 2){
                sum2+=(2*two) % mod;
                two=one;
                one=ans;
            }
        }
        return ans;
    }
};
```

</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** This problem is quite easy if we know the recursion: 
{{< katex >}}
\\(a(n) = a(n-1) + a(n-2) + \sum_{i=0}^{n-3} 2a(i),\  for\ n >= 3 \\)

- **Improving:** For a prettier style, we can store the three values in an array and initialize them for n < 3 cases, so that we can avoid using many `if`

<details>
<summary>Example</summary>

  ```cpp
class Solution {
public:
    const int mod=1e9+7;
    //a[n]=2*a[n-1]+a[n-3] for n>=3
    int numTilings(int n) {
        array<int,3> a={1, 1, 2};
        if (n<3) return a[n];
        for(int i=3; i<=n; i++){
            long long x=(2LL*a[2]+a[0])% mod;
            a={a[1], a[2], (int)x};
        }
        return a[2];
    }
};

```
</details>

---

### minimum-domino-rotations-for-equal-row
- [Link](https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/)
<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    int minDominoRotations(vector<int>& tops, vector<int>& bottoms) {
        int c1=tops[0];
        int c2=bottoms[0];
        int len = tops.size();
        int count = 0;
        int min_swap = len;
        int idx=0;
        bool done = false;
        while(idx < len){
            if(tops[idx] != c1)
                if (bottoms[idx] != c1) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        while(idx < len){
            if(tops[idx] != c2)
                if(bottoms[idx] != c2) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        while(idx < len){
            if(bottoms[idx] != c2)
                if(tops[idx] != c2) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        while(idx < len){
            if(bottoms[idx] != c1)
                if(tops[idx] != c1) break;
                else count++;
            idx++;
        }
        if(idx == len){
            done=true;
            if(count < min_swap)
                min_swap = count;
        }
        count=0;
        idx=0;
        if(done) return min_swap;
        else return -1;
    }
};
```
</details> 

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** I think the 4-while-loops looks too ugly.
- **Improving:** I can track the c1 and count swap_c1_top&swap_c1_bottom in the same iteration. [Source](https://leetcode.com/problems/minimum-domino-rotations-for-equal-row/solutions/6709103/3-different-kinds-of-solutions-beats-100)

## <span class="tag hard">Hard</span>

### maximum-number-of-tasks-you-can-assign 
- [Link](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/) 

<details>
<summary>My First Submission</summary>

```cpp
class Solution {
public:
    int maxTaskAssign(vector<int>& tasks, vector<int>& workers, int pills, int strength) {
        sort(tasks.begin(), tasks.end(), greater<int>());
        sort(workers.begin(), workers.end(), greater<int>());
        int count_completed = 0;
        int first_assign_idx = 0;
        while(first_assign_idx < workers.size()
            && first_assign_idx < tasks.size()
            && workers[first_assign_idx] >= tasks[first_assign_idx])
            first_assign_idx++;
        if(first_assign_idx >= workers.size()
            || first_assign_idx >= tasks.size())
            return first_assign_idx;
        count_completed = first_assign_idx;
        int head_rest_workers = first_assign_idx;
        int head_rest_tasks = first_assign_idx;
        queue<int> q;
        while(head_rest_workers < workers.size()
            && head_rest_tasks < tasks.size()){
            if (workers[head_rest_workers] >= tasks[head_rest_tasks]){
                head_rest_workers++;
                head_rest_tasks++;
                count_completed++;
            }
            else{
                q.push(tasks[head_rest_tasks]);
                head_rest_tasks++;
            }
        }
        if (head_rest_workers >= workers.size()) return count_completed++;
        while (!q.empty() && pills > 0 && head_rest_workers < workers.size()){
            if (workers[head_rest_workers] + strength >= q.front()){
                head_rest_workers++;
                count_completed++;
                pills--;
            }
            q.pop();
        }
        return count_completed++;
    }
};
```
</details>

- **My result:** <span class="result wa">WA</span>  
- **Original Idea & Result Analyzing:** 
  - I tried to `find the first k strongest workers to complete k hardest tasks.` Once I find that there is a task that the worker cannot solve, I `push the task to a queue` because it can only be solved with pills, and then I check the next task. If `worker >= task[i], the worker is assigned to solve this task`, and we move to the next worker&task. Next, we would finish assigning workers or push all tasks to the queue, which means that even the current strongest worker cannot solve the easiest task. In the end, I check if the rest workers can solve tasks with pills.
  - I get `37 / 49 testcases passed`. I find that it is because I tried to assign stronger worker to a easier task in the loop that I check `worker >= task[i]`, but actually the worker may solve a harder task with pills.
- Community [Solution](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/solutions/6703695/binary-search-greedy-with-images-example-walkthrough-c-python-java)
  - Idea: Binary search k, and check if k is **posible**
  - Correctness: doing...


