+++
title = "LeetCode Daily Notes"
date = "2025-05-03T11:12:30+08:00"
draft = false
math = true
tags = ["leetcode"]
+++
> 先把所有筆記都寫一起等哪天塞不下了再分開，練習用英文寫
## <span class="tag easy">Easy</span>

### finding-3-digit-even-numbers
- [Link](https://leetcode.com/problems/finding-3-digit-even-numbers/)
<details>
<summary>My First Submission</summary>

  ```cpp
  class Solution {
public:
    vector<int> findEvenNumbers(vector<int>& digits) {
        int len=digits.size();
        vector<int> table(10,0);
        vector<int> ans(0);
        int d1,d2,d3;
        for(int i=0;i<len;i++)
            table[digits[i]]++;
        for(int i=100;i<1000;i++){
            if(i%2!=0)continue;
            d1=i/100;
            d2=(i%100)/10;
            d3=i%10;
            if(table[d1]==0)continue;
            if(table[d2]==0)continue;
            else if(d1==d2 && table[d1]==1)continue;
            if(table[d3]==0)continue;
            else if(table[d3]==1 && (d3==d2 || d3==d1)) continue;
            else if(table[d3]==2 && d3==d2 && d3 ==d1) continue;
            int num=d1*100+d2*10+d3;
            ans.push_back(num);
        }
        return ans;
    }
};
  ```

</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** Just run 100-999 and check if its valid.
- **Improving:** This [solution](https://leetcode.com/problems/finding-3-digit-even-numbers/solutions/6735629/count-test-100-999-beats-100/) is a cleaner version of mine. There are some takeaways:
  - If I want to iterate the value of an array, use `for(int x: digits)` and `x` is better than `for(int i=0;i<len;i++)` and `digits[i]`
  - There are actually two choice to avoid many `if` in my code:(1) I can use `auto freq0=freq` so that I can update `freq0` and not change `freq`. (2) I can directly do the minus operation on the freq array and add them back in the end.

---

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

---

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

---

## <span class="tag medium">Medium</span>

### total-characters-in-string-after-transformations-i
- [Link](https://leetcode.com/problems/total-characters-in-string-after-transformations-i/)
<details>
<summary>My First Submission</summary>

  ```cpp
  class Solution {
public:
    int lengthAfterTransformations(string s, int t) {
        int len=s.size();
        long long ans=len;
        long long z_keep;
        const int mod=1e9+7;
        vector<long long> alphabet(26,0);
        for(int i=0;i<len;i++)
            alphabet[s[i]-'a']++;
        for(int i=0;i<t;i++){
            for(int j=25 ;j>=0; j--){
                if(j==25){
                    z_keep=alphabet[j];
                    ans=(ans+z_keep)%mod;
                }
                if(j==0){
                    alphabet[0]=z_keep;
                    alphabet[1]=(alphabet[1]+z_keep)%mod;
                }
                else alphabet[j]=alphabet[j-1];
            }
        }
        return ans;
    }
};
  ```

</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** First compute the frequency of all characters and we just update this frequency t time. For each update, we check the count of z and add this number to answer.

### find-minimum-time-to-reach-last-room-ii
- [Link](https://leetcode.com/problems/find-minimum-time-to-reach-last-room-ii/)
<details>
<summary>My First Submission</summary>

```cpp
 class Solution {
public:
    int minTimeToReach(vector<vector<int>>& moveTime) {
        int n = moveTime.size();
        int m = moveTime[0].size();
        int cost = 0;
        vector<vector<int>> ans(n, vector<int>(m, INT_MAX));
        priority_queue<tuple<int, int, int, int>, vector<tuple<int, int, int, int>>, greater<>> pq;

        ans[0][0] = 0;
        pq.push({0, 0, 0, 0});

        vector<pair<int, int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        while (!pq.empty()) {
            auto [time, r, c, pop_cost] = pq.top();
            int cost = pop_cost;
            pq.pop();
            int move_cost= cost + 1;
            if (r == n - 1 && c == m - 1)
                return time;

            if (time > ans[r][c]) continue; // already found better path

            for (auto [dr, dc] : dirs) {
                int nr = r + dr, nc = c + dc;
                if (nr < 0 || nr >= n || nc < 0 || nc >= m) continue;

                int wait = max(moveTime[nr][nc], time);
                int arriveTime = wait + move_cost;

                if (arriveTime < ans[nr][nc]) {
                    ans[nr][nc] = arriveTime;
                    pq.push({arriveTime, nr, nc, (cost+1)%2});
                }
            }
        }

        return ans[n - 1][m - 1]; // unreachable case (problem guarantees reachability)
    }
};

  ```
</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** Just store one more "cost". Trivial if we've done problem i.

---

### find-minimum-time-to-reach-last-room-i
- [Link](https://leetcode.com/problems/find-minimum-time-to-reach-last-room-i/)
<details>
<summary>My First Submission</summary>

  ```cpp
  class Solution {
public:
    int minTimeToReach(vector<vector<int>>& moveTime) {
        int n=moveTime.size();
        int m=moveTime[0].size();
        vector<vector<int>> ans(n, vector<int>(m, 1e9+100));
        ans[0][0]=0;
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
        pq.push({0, 0, 0});
        int cur_r=0;
        int cur_c=0;
        while(!pq.empty()){
            if(cur_r>0){
                if(ans[cur_r][cur_c] >= moveTime[cur_r-1][cur_c] &&
                 ans[cur_r][cur_c]+1 < ans[cur_r-1][cur_c]){
                    ans[cur_r-1][cur_c] = ans[cur_r][cur_c]+1;
                    pq.push({ans[cur_r-1][cur_c],cur_r-1,cur_c});
                }
                else if(ans[cur_r][cur_c] < moveTime[cur_r-1][cur_c] &&
                 moveTime[cur_r-1][cur_c]+1 < ans[cur_r-1][cur_c]){
                    ans[cur_r-1][cur_c] = moveTime[cur_r-1][cur_c]+1;
                    pq.push({ans[cur_r-1][cur_c],cur_r-1,cur_c});
                }
            }
            if(cur_r<n-1){
                if(ans[cur_r][cur_c] >= moveTime[cur_r+1][cur_c] &&
                 ans[cur_r][cur_c]+1 < ans[cur_r+1][cur_c]){
                    ans[cur_r+1][cur_c] = ans[cur_r][cur_c]+1;
                    pq.push({ans[cur_r+1][cur_c],cur_r+1,cur_c});
                 }
                 else if(ans[cur_r][cur_c] < moveTime[cur_r+1][cur_c] &&
                 moveTime[cur_r+1][cur_c]+1 < ans[cur_r+1][cur_c]){
                    ans[cur_r+1][cur_c] = moveTime[cur_r+1][cur_c]+1;
                    pq.push({ans[cur_r+1][cur_c],cur_r+1,cur_c});
                }
            }
            if(cur_c>0){
                if(ans[cur_r][cur_c] >= moveTime[cur_r][cur_c-1] &&
                 ans[cur_r][cur_c]+1 < ans[cur_r][cur_c-1]){
                    ans[cur_r][cur_c-1] = ans[cur_r][cur_c]+1;
                    pq.push({ans[cur_r][cur_c-1],cur_r,cur_c-1});
                 }
                 else if(ans[cur_r][cur_c] < moveTime[cur_r][cur_c-1] &&
                 moveTime[cur_r][cur_c-1]+1 < ans[cur_r][cur_c-1]){
                    ans[cur_r][cur_c-1] = moveTime[cur_r][cur_c-1]+1;
                    pq.push({ans[cur_r][cur_c-1],cur_r,cur_c-1});
                }
            }
            if(cur_c<m-1){
                if(ans[cur_r][cur_c] >= moveTime[cur_r][cur_c+1] &&
                 ans[cur_r][cur_c]+1 < ans[cur_r][cur_c+1]){
                    ans[cur_r][cur_c+1]= ans[cur_r][cur_c]+1;
                    pq.push({ans[cur_r][cur_c+1],cur_r,cur_c+1});
                }
                else if(ans[cur_r][cur_c] < moveTime[cur_r][cur_c+1] &&
                 moveTime[cur_r][cur_c+1]+1 < ans[cur_r][cur_c+1]){
                    ans[cur_r][cur_c+1] = moveTime[cur_r][cur_c+1]+1;
                    pq.push({ans[cur_r][cur_c+1],cur_r,cur_c+1});
                }
            }
            auto [val, row, col] = pq.top();
            pq.pop(); 
            cur_r=row;
            cur_c=col;
            if(cur_r==n-1&&cur_c==m-1)
                return val;
        }
        return ans[n-1][m-1];
    }
};
  ```

</details>

- **My result:** <span class="result ac">AC</span>  
- **Original Idea & Result Analyzing:** Originally, I guess that it's a dp problem because the problem goes from top-left to bottom-right, and we need the minimum cost. However, I find that dp does not work because the values not only come from top or left. After seeing the hint, I know that I have to implement the `Dijkstra` alg but actually I forget the details about this algorithm. My friend told me that I can use a priority_queue, and I tried to finish this problem in the way that I think probably works. Fortunately, I get ac in the end, and gpt told me what I did is exactly the `Dijkstra` algorithm.
- **Dijkstra rewind:** Doing...
- **Improving:** I can improve the redundant direction conditions:

<details>
<summary>Code</summary>

  ```cpp
  class Solution {
public:
    int minTimeToReach(vector<vector<int>>& moveTime) {
        int n = moveTime.size();
        int m = moveTime[0].size();
        vector<vector<int>> ans(n, vector<int>(m, INT_MAX));
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;

        ans[0][0] = 0;
        pq.push({0, 0, 0});

        vector<pair<int, int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        while (!pq.empty()) {
            auto [time, r, c] = pq.top();
            pq.pop();

            if (r == n - 1 && c == m - 1)
                return time;

            if (time > ans[r][c]) continue; // already found better path

            for (auto [dr, dc] : dirs) {
                int nr = r + dr, nc = c + dc;
                if (nr < 0 || nr >= n || nc < 0 || nc >= m) continue;

                int wait = max(moveTime[nr][nc], time);
                int arriveTime = wait + 1;

                if (arriveTime < ans[nr][nc]) {
                    ans[nr][nc] = arriveTime;
                    pq.push({arriveTime, nr, nc});
                }
            }
        }

        return ans[n - 1][m - 1]; // unreachable case (problem guarantees reachability)
    }
};
  ```

  </details>

---
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

---

## <span class="tag hard">Hard</span>

### count-number-of-balanced-permutations
- [Link](https://leetcode.com/problems/count-number-of-balanced-permutations/)
- **No Submission**
- **Original Idea:** 
  - Only the frequency of the digits matters because of the permutation
  - Once we find a set of even digits and odd digits, we can use combination to find the number of this permutation 
  - We should use dp to find the valid set otherwise the complexity is too big. However, I did not come up with such dp relation.
- **Community** [Solution](https://leetcode.com/problems/count-number-of-balanced-permutations/solutions/6726791/dp-combinatorics-step-by-step-with-images-example-walkthrough-c-python-java/)
  - **Idea:**
    - `dp[i][j]` :Stores the number of ways to pick exactly `j digits` found so far that `sum to i`. Hence, `[halfsum][halflen]` would get the final ways of having the sum of halfsum with halflen. As we iterate the num array, we update: 
      - (1) the count of the digit `d`
      - (2) For the current digit `d`, some `sum` and `digit_len`, we know that `dp[sum][digit_len] += dp[sum-d][digit_len-1]`.
      - (3) This line `res = res * fact[halfLen] % mod * fact[n - halfLen] % mod;` means that the two half can permute, and next we should divide the duplicates which is stored in `digits[i]`.
 

---

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
- **Community** [Solution](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/solutions/6703695/binary-search-greedy-with-images-example-walkthrough-c-python-java)
  - **Idea:** Binary search k, and check if k is **posible**
  - **Correctness:** doing...


